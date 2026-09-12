# QZ Tray vs web-print-pdf vs JSPrintManager

**ローカルサイレント印刷ブリッジ**を選ぶチーム向けの実践比較。数値や製品面は変わる — 購入前に必ずベンダーサイトで確認してください。

## スナップショット

| 観点 | [QZ Tray](https://qz.io/) | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
|---|---|---|---|
| ポジショニング | 成熟した POS / raw + ピクセルブリッジ | ローカルエージェント + npm SDK | 商用 JS + クライアント、印刷 & スキャン |
| Raw ESC/POS / ZPL | 強い | 通常 HTML/PDF 経路 | 強い |
| HTML/CSS 業務ドキュメント | サポート | サポート | サポート |
| npm / async DX | スクリプト + WS 指向 | `npm` + Promise/`async` | スクリプト中心 |
| 英語ドキュメント | あり | あり | あり |
| オンラインデモ | [demo.qz.io](https://demo.qz.io/) | [demos](https://webprintpdf.com/en/docs/demos/) | [azure demo](https://jsprintmanager.azurewebsites.net/) |
| サイレント摩擦 | 署名 / ライセンスが一般的 | クライアントインストール | ライセンス + クライアント |
| よく選ばれる場合 | raw 方言 + グローバル POS 実績 | HTML/CSS テンプレートと npm スタイル SPA 呼び出し | 広いファイルタイプ / 商用サポート |

## 詳細メモ

### QZ Tray

- 強み：raw 印刷文化、ピクセル印刷、POS / ラベルコミュニティでの長い実績。
- 本番サイレントモードには証明書 / 署名ワークフローを計画。
- フロントエンド組み込みは通常スクリプト + WebSocket；チームは独自 Promise ヘルパーでラップすることが多い。

### web-print-pdf (Web Print Expert)

- 強み：すでに HTML/CSS と npm で考えている SPA チーム。
- raw 方言は通常主経路ではない — ESC/POS/ZPL がコア workload なら慎重に評価。
- デスク端末群に対する Linux/macOS/Windows エージェントカバレッジを確認。

### JSPrintManager

- 強み：商用機能の幅（エディションにより印刷 + 関連デバイスワークフロー）。
- 展開コストの一部としてライセンス + クライアントインストールを見込む。
- 調達が広いファイルタイプストーリーの単一商用ベンダーを望む場合の有力候補。

## 経験則

- **デバイス方言優先** → QZ / JSPM が通常の短リスト。
- **SPA から HTML/CSS** → 3 つとも可能；パイロットデスクでデモ + インストール摩擦を比較。
- **多数拠点へのクラウドルーティング** → PrintNode も評価。

## パイロットチェックリスト（3 つ共通）

- [ ] クリーン PC（アンチウイルス ON）にエージェントをインストール
- [ ] HTML A4 1 枚とラベル / 伝票 1 枚を印刷
- [ ] 現在の Chrome で HTTPS サイト → localhost を確認
- [ ] 50 件バッチを計測
- [ ] 法務 / IT とライセンス / 署名要件を確認

## 関連

- [スタックの選び方](choose-silent-print-stack.ja.md)
- [メインリストの API 比較](../README.ja.md#api-friendliness-frontend-dx)
- [QZ Tray 代替案](qz-tray-alternatives.ja.md)
- [JSPrintManager 代替案](jsprintmanager-alternatives.ja.md)
