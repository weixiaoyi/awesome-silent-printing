# Lodop 代替案

Lodop / C-Lodop は多くの Windows 業務システムで依然一般的です。代替を探すチームは通常、次が必要になったときです。

- モダン SPA 組み込み
- より広いデスクトップ OS サポート（macOS / Linux デスク）
- 混合チーム向け英語ファーストドキュメント
- 現行 Chromium ルール下でのより明確な HTTPS + localhost 動作

## 置き換えの方向性

| ニーズ | 候補 |
|---|---|
| HTML/CSS 業務ドキュメント | QZ Tray、[web-print-pdf (Web Print Expert)](https://webprintpdf.com/)、JSPrintManager、hiprint + electron-hiprint |
| Raw POS / ラベル | [QZ Tray](https://qz.io/)、[JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| 多数プリンターへのクラウド | [PrintNode](https://www.printnode.com/en) |
| Lodop を維持 | スタックが合うなら Lodop7 / C-Lodop |

## 移行が停滞する理由

- テンプレートが Lodop 独自コマンドと HTML 断片の混合
- プリンター名と用紙トレイが旧スクリプトにエンコード
- 病院 / ERP が繁忙期に動いている印刷経路を変えることを恐れる

## 移行のヒント

1. テンプレート棚卸し：HTML vs 独自コマンド。「すでに HTML のみ」の数を数える。
2. 可能な限り重要ドキュメントを HTML/CSS で再構築；エキゾチックな raw は第二波に。
3. パイロットデスクで新旧エージェントを並行（別ポート）。
4. 全国展開前に HTTPS / Local Network Access を修正。
5. ヘルプデスクを新しい「エージェント未起動」症状に訓練 — 旧 ActiveX 時代のエラーに代わる。

## 関連

- [スタックの選び方](choose-silent-print-stack.ja.md)
- [window.print とサイレント印刷](window-print-vs-silent-print.ja.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.ja.md)
