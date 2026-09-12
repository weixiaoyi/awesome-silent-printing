# Chrome Local Network Access & 127.0.0.1

## 症状

開発では動く。本番では次のようなエラー：

```text
WebSocket connection to 'ws://127.0.0.1:…' failed
Failed to connect to print agent
ERR_CONNECTION_REFUSED / net::ERR_FAILED
```

ローカル印刷クライアントは動いている。ファイアウォールも問題なさそう。**デプロイ済み Web サイトだけ**がループバックに届かない。

これは localhost ブリッジ全般で最も多い「本番稼働後にサイレント印刷が壊れた」インシデントの一つです。

## 原因

Chromium の **Local Network Access (LNA)** は、公開ページがユーザーのローカルネットワーク / ループバックと通信することを制限します。`127.0.0.1` で待ち受ける印刷エージェントは、まさにその制限ゾーンにあります。

現場チームが遭遇する実務ルール：

| コンテキスト | 典型的な結果 |
|---|---|
| 開発で `http://localhost` のアプリ | 多くの場合動作（特別なコンテキスト） |
| 本番のプレーン **HTTP** | 多くの場合**静かに拒否** |
| 信頼できる証明書の **HTTPS** | Local Network 権限のプロンプトが出る可能性 |
| 新しい Chrome + ループバック WebSocket | `ws://127.0.0.1…` にも同じ LNA ルール |
| Edge / その他 Chromium | 同様のポリシー |

エージェントが「起動している」ことは必要だが十分ではありません。**ブラウザのオリジン**がループバックと通信できる必要があります。

## 決定フローチャート

```text
Can the page reach ws://127.0.0.1 / http://127.0.0.1?
│
├─ No, and site is HTTP
│     → Put the site on HTTPS first. Stop here until that ships.
│
├─ No, and site is HTTPS
│     → Check Local Network permission / prompt
│     → Confirm agent port + process
│     → Test from the same machine with a tiny WS client
│
└─ Yes, but print still fails
      → Printer name, driver, spooler, template — not LNA
```

## 対処（チェックリスト）

1. **Web アプリを HTTPS** で提供し、ワークステーションが信頼する証明書を使う（公開 CA または社内 PKI）。ユーザーが信頼していない自己署名証明書は失敗し続けます。
2. 初回印刷時にブラウザの **Local Network** 権限プロンプトを確認し、オリジンを許可。
3. デスクトップ印刷エージェントが `127.0.0.1`（および SDK が期待するポート）で待ち受けていることを確認。
4. DevTools → Network で再現：WS/HTTP 呼び出しがブロック、拒否、リセットのどれか。
5. **管理端末群のみ**：ブラウザフラグ / エンタープライズポリシーでチェックを緩和可能。一般 SaaS エンドユーザー向け戦略ではない。
6. インストール手順書に権限ステップを記載；さもないとヘルプデスクがエージェントを永遠に再インストールします。

## LNA と「エージェントダウン」の見分け

| 確認 | エージェントダウン | LNA / オリジン問題 |
|---|---|---|
| ローカルツールから `127.0.0.1:port` | 失敗 | 成功 |
| 同一マシン、HTTP サイト | 失敗の可能性 | 多くの場合失敗 |
| 同一マシン、HTTPS + 権限 | エージェント起動なら動作 | 動作 |
| Chrome の別ユーザープロファイル | 同じ | 権限が欠けている可能性 |

## 影響を受ける対象

localhost 印刷ブリッジ全般：QZ Tray、web-print-pdf、JSPrintManager、Lodop 系ローカルサービス、カスタムエージェント。単一ベンダーのバグではありません。

## 詳しい解説

- English: [WebSocket to 127.0.0.1 failed after deploy](https://webprintpdf.com/en/docs/production-print-troubleshoot/)
- 中文: [上线后连接 127.0.0.1 失败](https://webprintpdf.com/docs/production-print-troubleshoot/)

## 関連

- [サイレント印刷の仕組み](how-silent-printing-works.ja.md)
- [Vue / React サイレント印刷](vue-react-silent-print.ja.md)
- [スタックの選び方](choose-silent-print-stack.ja.md)
