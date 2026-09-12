# Vue / React サイレント印刷

## 目標

`window.print()` を開かずに SPA からサイレント印刷を呼び出す。

## 典型的な組み込み

1. ユーザーがローカル印刷エージェントを一度インストール。
2. フロントエンドが npm SDK またはベンダー JS（QZ、web-print-pdf、JSPM 等）に依存。
3. ページが HTML、PDF、または raw ペイロードを `127.0.0.1` へ送信。
4. エージェントがレンダリング / OS プリンターへ転送。

呼び出しの形（API はベンダーにより異なる）:

```js
// Pseudocode — check your bridge’s SDK docs
await printAgent.printHtml(
  '<div class="label">Order #1001</div>',
  { printer: 'LabelPrinter' }
);
```

### 推奨モジュール境界

印刷を小さなサービスに閉じ込め、Vue/React コンポーネントはシンプルに保つ：

```js
// printService.js
export async function printLabel(html, printer) {
  await ensureAgent();
  return printAgent.printHtml(html, { printer });
}
```

- `ensureAgent()` をアプリ起動時、または最初の印刷画面の前に呼ぶ。
- エージェント欠落時はインストール手順を表示。
- 10 コンポーネントから 10 種類の options で直接 print を呼ばない。

## SPA の落とし穴

| 落とし穴 | 対策 |
|---|---|
| エージェント起動前に print を呼ぶ | 接続をプリフライト / インストール案内を表示 |
| プリンター名のハードコード | プリンター一覧を取得；ステーションごとに永続化 |
| HTTP 本番オリジン | Local Network Access のため HTTPS へ |
| 画面とスタイルが異なる | Chromium ベース HTML→print エージェントを優先；フォントを固定 |
| UI クローム付きの live Vue/React ツリーを印刷 | 専用 print root / オフスクリーンテンプレートをレンダリング |
| 巨大な base64 インライン画像 | エージェントが取得できる URL を優先、または圧縮 |
| Promise rejection を無視 | トースト + リトライにマップ；job id をログ |

## Vue の注意点

- 印刷専用 SFC（`LabelTicket.vue`）にテンプレートを置き、ページレイアウト全体は使わない。
- `ref` + マウント済み print root の `innerHTML` / `outerHTML`、またはデータから HTML 文字列を組み立てを優先。
- `<Transition>` や仮想リストの更新中に印刷しない。

## React の注意点

- 同様に：非表示コンテナにレンダリングする `PrintTicket` コンポーネント → HTML をシリアライズ。
- portal と concurrent rendering に注意 — データが安定してからスナップショット。
- `useEffect` 内の `window.print()` を「一時的な」サイレント経路として頼らない。誤った習慣を植え付ける。

## 接続ライフサイクル

```text
App start
  → ping agent
  → if down: banner + install link
  → if up: cache printer list
User clicks Print
  → re-ping (cheap)
  → submit job
  → show job result / error code
```

## 関連

- [HTML/CSS サイレント印刷](html-css-silent-print.ja.md)
- [Chrome Local Network Access](chrome-local-network-access.ja.md)
- [スタックの選び方](choose-silent-print-stack.ja.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.ja.md)
