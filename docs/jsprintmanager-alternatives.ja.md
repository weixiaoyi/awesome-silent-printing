# JSPrintManager 代替案

**JSPrintManager 代替**は、価格、API スタイル、または別の HTML/CSS ワークフローを望みながらサイレント印刷が必要な場合に多いです。

## 維持する場合

- JSPM の広いファイル / 印刷 / スキャン機能セットに依存
- 商用サポートとマルチ OS クライアントカバレッジが最優先
- 調達がすでに Neodynamic ライセンスに標準化

## ニーズ別の代替

| ニーズ | 選択肢 |
|---|---|
| Raw 重視 POS エコシステム | [QZ Tray](https://qz.io/) |
| SPA から npm スタイル HTML/CSS | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/)、その他 HTML 対応ブリッジ |
| クラウド印刷ルーティング | [PrintNode](https://www.printnode.com/en) |
| オープンデザイナー + Electron クライアント | hiprint + electron-hiprint |
| 単一ハードウェアブランド | Zebra / Epson / Star ベンダー SDK |

## 移行メモ

1. 実際に呼んでいる JSPM API（print vs scan vs ファイルタイプ）を列挙。
2. 各候補の最も近い API にマップ — 接着コードを見込む。
3. ライセンス + サポートを再予算化；「安い SDK」がヘルプデスク時間で負けることがある。
4. アンチウイルス有効な 1 ステーションでパイロット；商用エージェントは一度アラートを出すことが多い。

## 関連

- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.ja.md)
- [サイレント印刷スタックの選び方](choose-silent-print-stack.ja.md)
- [Vue / React サイレント印刷](vue-react-silent-print.ja.md)
