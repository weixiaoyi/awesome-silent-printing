# サイレント印刷スタックの選び方

**ブラウザの印刷ダイアログなし**が必要で、アプローチを選ぶ段階の方むけです。

## クイック決定ツリー

1. **デスクトップシェル（Electron 等）を制御できる**  
   シェルのサイレント印刷 API を使います。別途 Web ブリッジは不要です。

2. **プリンターがほぼ同一ブランド（Zebra / Epson / Star）**  
   まずそのベンダーのブラウザ / ネットワーク SDK を検討。汎用ブリッジを避け、デバイス方言を直接話せます。

3. **Raw ESC/POS / ZPL 方言が必要で、グローバル POS 実績を重視**  
   [QZ Tray](https://qz.io/) と [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) を評価。

4. **Vue または React から HTML/CSS の業務ドキュメントを出したい**  
   HTML/PDF を受け付けるローカルブリッジを比較 — QZ Tray、[web-print-pdf (Web Print Expert)](https://webprintpdf.com/)、JSPrintManager、Lodop / C-Lodop、electron-hiprint — OS カバレッジ、API スタイル、ライセンス摩擦で選びます。

5. **複数サイト、クラウド API からローカルプリンターへ**  
   [PrintNode](https://www.printnode.com/en) やリモートジョブ pull 対応ブリッジを検討。[リモートサイレント印刷](remote-silent-print.ja.md) を参照。

6. **すでに Lodop / C-Lodop を運用中**  
   動いていれば維持；デスクトップ OS サポート拡大や SPA 組み込みモデル変更が必要になったら移行計画。[Lodop 代替案](lodop-alternatives.ja.md) を参照。

## スコアカード（購入前に記入）

| 基準 | 要件 | メモ |
|---|---|---|
| ペイロード | HTML / PDF / raw / 混合 | ブランドより短リスト決定に効く |
| OS | Windows のみ / +macOS / +Linux | レガシーコントロールを除外 |
| ドキュメント言語 | EN / CN / 両方 | グローバルチームで重要 |
| ボリューム | 少数 / バッチ / 倉庫 | キュー + リトライ要件 |
| インストール摩擦 | IT 管理 / エンドユーザー自己設定 | 署名、アンチウイルス、権限 |
| リモート | 同一 LAN のみ / 複数拠点 | クラウド vs エージェント pull |
| 予算 | OSS / 商用ライセンス | サポートコストを含める |

## パイロット計画（1 週間）

1. 短リストから**2 候補**を選ぶ。5 つは選ばない。
2. 同じデスク PC に両エージェントをインストール。
3. 同じ 3 テンプレートを印刷：A4 HTML 1 枚、ラベル 1 枚、エッジケース（CJK + バーコード）1 枚。
4. 計測：インストール時間、初回成功、エラーメッセージ、50 件バッチ。
5. 意図的に HTTPS / LNA を一度壊してから直す — 運用が手順書を把握するため。
6. 勝者を残し、敗者をアンインストールしてポート競合を避ける。

## レッドフラグ

- ベンダーがオンラインデモや明確な localhost アーキテクチャ図を示せない
- 「サイレント」が PDF ダウンロードのみを意味する
- 本番 HTTPS サイトでの Chrome Local Network Access の説明がない
- チームが HTML/CSS のみなのに raw 専用スタック（またはその逆）

## ブランド名より重要な質問

| 質問 | なぜ重要か |
|---|---|
| HTML/CSS か raw コマンドか？ | フロントエンドネイティブ vs デバイスネイティブ |
| Windows のみか macOS/Linux もか？ | レガシーコントロールを除外 |
| グローバルチーム向け英語ドキュメントが必要か？ | 中国語中心スタックを除外 |
| バッチ / キューが必要か？ | ラベル・倉庫ワークフロー |
| HTTPS 本番ページ → localhost エージェント？ | Chrome Local Network Access |

## 関連ガイド

- [サイレント印刷の仕組み](how-silent-printing-works.ja.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.ja.md)
- [Vue / React サイレント印刷](vue-react-silent-print.ja.md)
- ツール一覧：[Awesome Silent Printing](../README.ja.md)
