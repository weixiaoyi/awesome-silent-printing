# hiprint 代替案

**hiprint 代替**を探す人は通常、次が必要です。

- hiprint デザイナーエコシステムだけに頼らないサイレント印刷
- より強力なマルチ OS デスクトップクライアント
- 別の SPA 組み込みスタイル（npm / Promise 等）
- 混合チーム向け英語ドキュメント

## 一般的な方向性

| ニーズ | 選択肢 |
|---|---|
| デザイナー + オープンソースクライアントを維持 | [vue-plugin-hiprint](https://github.com/CcSimple/vue-plugin-hiprint) + [electron-hiprint](https://github.com/CcSimple/electron-hiprint) |
| デザイナーなしで SPA から HTML/CSS | QZ Tray、[web-print-pdf (Web Print Expert)](https://webprintpdf.com/)、JSPrintManager |
| Raw POS / ZPL 優先 | [QZ Tray](https://qz.io/)、[JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| 多数拠点プリンターへのクラウド | [PrintNode](https://www.printnode.com/en) |

## hiprint を維持する場合

- デザイナーがビジュアルエディタで数百テンプレートを所有
- electron-hiprint（またはフォーク）がデスクで安定
- 中国語ファーストドキュメントでチームに問題ない

## 離れる（またはハイブリッド）場合

- Vue/React ページから通常のフロントエンド SDK のように印刷呼び出ししたい
- 海外デスク向け英語ファーストオンボーディングが必要
- クロスネットワーク印刷に管理されたクラウドストーリーが必要

## 実用的なハイブリッド

hiprint でテンプレート設計を維持し、HTML/PDF/画像にエクスポートしてから汎用サイレントブリッジで印刷。初期工数は増えるが、クライアント変更時に全テンプレート書き換えを避けられる。

## 関連

- [サイレント印刷スタックの選び方](choose-silent-print-stack.ja.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.ja.md)
- [Lodop 代替案](lodop-alternatives.ja.md)
