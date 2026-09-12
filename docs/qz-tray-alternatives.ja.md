# QZ Tray 代替案

**QZ Tray 代替**を検索する人は、通常、API スタイル、価格、署名、または HTML/CSS ワークフローで別のトレードオフを望みながら、ブラウザからサイレント印刷したい場合です。

## QZ Tray を維持する場合

- Raw ESC/POS / ZPL が主 workload
- QZ 署名と証明書にすでに投資済み
- 長いグローバル POS 実績が必要
- チームが本番で QZ WebSocket 呼び出しをすでにラップしている

## 代替を評価する場合

| ニーズ | 検討先 |
|---|---|
| 広いファイルタイプの商用 JS クライアント | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| SPA から npm スタイル HTML/CSS | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/)、その他 HTML 対応ブリッジ |
| 多数プリンターへのクラウド API | [PrintNode](https://www.printnode.com/en) |
| デザイナー主導テンプレート | hiprint + electron-hiprint |
| 単一ハードウェアブランド | Zebra / Epson / Star ベンダー SDK |

## 移行メモ

1. raw vs HTML/PDF ジョブを棚卸し — raw ジョブが高コストな書き換え。
2. 署名 / ライセンス前提を再テスト；次ベンダーが「署名なしサイレント」とは限らない。
3. 代替をパイロット中も QZ デスクを 1 台維持。
4. 新エージェントポートで Chrome Local Network Access を再検証。

## 直接比較

- [QZ Tray vs web-print-pdf vs JSPrintManager](qz-vs-jspm-vs-web-print-pdf.ja.md)
- [サイレント印刷スタックの選び方](choose-silent-print-stack.ja.md)
