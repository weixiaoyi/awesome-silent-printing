<div align="center">

# Awesome Silent Printing

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[English](README.md) | [中文](README.zh-CN.md) | **日本語** | [Español](README.es.md) | [Português (Brasil)](README.pt-BR.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Русский](README.ru.md)

</div>

> Web アプリケーションから**サイレント印刷**（ブラウザの印刷ダイアログを表示せずに印刷）を行うための、ツール・ライブラリ・ブリッジ・資料の厳選リスト。
>
> 対象範囲：`window.print` の制限、Chrome の `127.0.0.1` への Local Network Access、Vue/React でのサイレント印刷、HTML/CSS 印刷エージェント、Lodop の代替、QZ Tray / web-print-pdf / JSPrintManager の比較、バッチラベル印刷、リモート印刷など。

---

## このリストの目的

ブラウザはセキュリティ上の理由から、意図的にサイレント印刷をブロックします。レシート、配送ラベル、請求書、キッチン伝票などが必要なチームは、通常 **ローカルブリッジ**、**拡張機能**、**キオスクポリシー**、または **ベンダー SDK** のいずれかを導入します。

本リストは、その狭い問題に焦点を当てています：**`window.print()` のダイアログなしで Web から印刷する方法**。

---

## ガイド

長文ガイド（日本語）:

- [ブラウザサイレント印刷ハブ](docs/browser-silent-print.ja.md)
- [サイレント印刷の仕組み](docs/how-silent-printing-works.ja.md)
- [サイレント印刷スタックの選び方](docs/choose-silent-print-stack.ja.md)
- [window.print とサイレント印刷](docs/window-print-vs-silent-print.ja.md)
- [Chrome Local Network Access と 127.0.0.1](docs/chrome-local-network-access.ja.md)
- [Vue / React でのサイレント印刷](docs/vue-react-silent-print.ja.md)
- [HTML/CSS サイレント印刷](docs/html-css-silent-print.ja.md)
- [Web からのバッチ / ラベル印刷](docs/batch-label-printing.ja.md)
- [リモート / サーバー配信サイレント印刷](docs/remote-silent-print.ja.md)
- [QZ Tray、web-print-pdf、JSPrintManager 比較](docs/qz-vs-jspm-vs-web-print-pdf.ja.md)
- [Lodop の代替](docs/lodop-alternatives.ja.md)
- [hiprint の代替](docs/hiprint-alternatives.ja.md)
- [QZ Tray の代替](docs/qz-tray-alternatives.ja.md)
- [JSPrintManager の代替](docs/jsprintmanager-alternatives.ja.md)
- [ブラウザからのサーマルレシートサイレント印刷](docs/thermal-receipt-silent-print.ja.md)

---

## 目次

- [ガイド](#ガイド)
- [ブラウザの制限（先に読む）](#ブラウザの制限先に読む)
- [サイレント印刷の仕組み](#サイレント印刷の仕組み)
- [選び方](#選び方)
- [比較表](#比較表)
- [ローカル印刷ブリッジ](#ローカル印刷ブリッジ)
- [ハードウェアベンダー SDK](#ハードウェアベンダー-sdk)
- [クラウド / リモート印刷](#クラウド--リモート印刷)
- [デスクトップ / Electron](#デスクトップ--electron)
- [オープンソースプロジェクト](#オープンソースプロジェクト)
- [サイレントではない（よくある混同）](#サイレントではないよくある混同)
- [セキュリティ](#セキュリティ)
- [貢献](#貢献)
- [翻訳](#翻訳)

---

## ブラウザの制限（先に読む）

| 方式 | サイレント？ | 備考 |
|---|---|---|
| `window.print()` | いいえ（デフォルト） | ブラウザが印刷ダイアログを表示。ページの JS から選択したデバイスへ完全にサイレント印刷することはできない |
| Print.js / react-to-print | いいえ | ブラウザの印刷 UI を開く |
| Chrome キオスク / エンタープライズ印刷ポリシー | 条件付き | 管理端末 / キオスク端末でのみ有効 |
| Chrome / Edge の **`127.0.0.1` への Local Network Access (LNA)** | ローカルブリッジに影響 | 非ローカルページはループバックへ到達するために **セキュアコンテキスト（HTTPS）** が必要。プレーン HTTP は多くの場合 **サイレントに拒否** される。新しい Chromium ビルドでは **WebSocket**（`ws://127.0.0.1…`）にも適用。Local Network の権限プロンプトが表示される場合あり。`localhost` での開発は通常問題なし。本番 HTTP では多くのエージェントが動作しない |
| 真のクロスサイトサイレント印刷 | ローカルエージェントが必要 | 典型的なパターン：localhost HTTP/WebSocket / Native Messaging → OS スプーラーまたは raw ポート |

この LNA の挙動は、特定ベンダーだけでなく **すべての** localhost 印刷ブリッジ（QZ、web-print-pdf、JSPM、Lodop のクラウド→ローカルパターンなど）に影響します。詳細：[デプロイ後に 127.0.0.1 への WebSocket が失敗する](https://webprintpdf.com/en/docs/production-print-troubleshoot/)。

---

## サイレント印刷の仕組み

| アプローチ | 概要 | よくあるトレードオフ |
|---|---|---|
| ローカルブリッジ / エージェント | ページがプリンターを所有する localhost サービスと通信 | クライアントのインストールが必要 |
| ブラウザ拡張 + ネイティブホスト | 拡張機能が Native Messaging ホストを呼び出す | ストア審査と信頼のハードル |
| エンタープライズ / キオスクポリシー | ブラウザの印刷設定を固定 | 管理端末向け |
| ベンダー SDK | Epson / Zebra / Star スタックと通信 | ハードウェアロックイン |
| デスクトップシェル（Electron など） | Chromium を組み込み、ネイティブ印刷 API を使用 | 純粋なブラウザアプリではない |

---

## 選び方

1. **成熟したグローバル POS / raw + ピクセルエコシステム** → [QZ Tray](https://qz.io/)、[JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/)。
2. **SPA（Vue / React など）から HTML/CSS の業務帳票** → HTML/PDF を受け付けるローカルブリッジを比較。例：QZ Tray、[web-print-pdf (Web Print Expert)](https://webprintpdf.com/)、JSPrintManager、[Lodop / C-Lodop](http://www.c-lodop.com/)、[electron-hiprint](https://github.com/CcSimple/electron-hiprint)。
3. **レガシー印刷コントロールを既に利用中** → Lodop / hiprint スタックの継続評価。OS カバレッジや SPA の DX が障壁になったときのみ移行。
4. **Linux デスクトップ（必要に応じて Kylin / UOS を含む）** → 実際の Linux クライアントがあるブリッジを優先（QZ Tray、web-print-pdf、JSPrintManager など）。
5. **Zebra / Epson / Star プリンターのみ** → 対応する [ベンダー SDK](#ハードウェアベンダー-sdk) を優先。
6. **グローバルチーム向けに英語優先のドキュメント / UI** → [比較表](#比較表) の **English** 列で ✅ のツールを優先。Lodop や多くの hiprint 資料は中国語が主。
7. **クラウド API → 複数拠点のプリンター** → [PrintNode](https://www.printnode.com/en) またはリモート対応のローカルエージェント。
8. **オープンソース SDK / 学習目的** → [オープンソースプロジェクト](#オープンソースプロジェクト) を参照。小規模リポジトリはメンテナンスが不均一な場合あり。

---

## 比較表

### プラットフォームとペイロード

| ツール | Win | macOS | Linux | 英語対応 | オンラインデモ | HTML/CSS | PDF | Raw (ESC/POS, ZPL…) | バッチ | リモート |
|---|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://demo.qz.io/) | ✅ | ✅ | ✅ 強い | ✅ | アプリ経由 |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ | ✅ | HTML/PDF 経由 | ✅ | ✅ |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | ✅ | ✅ | ✅ 強い | ✅ | 製品経由 |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ✅ | — | 一部 | ⚠️ 中国語主 | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ✅ | ✅ | 一部 | ✅ | クラウド系 |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ✅ | ✅ | ✅ | ⚠️ 中国語主 | ⚠️ デザイナーデモ中心 | ✅ | ✅ | — | ✅ | 中継経由 |
| [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) | ✅ | ✅ | — | ✅ | ⚠️ サンプル / ローカル | — | 画像 | ZPL/raw | 限定的 | — |
| [PrintNode](https://www.printnode.com/en) | ✅ | ✅ | ✅ | ✅ | ⚠️ API ドキュメント | — | ✅ | ✅ | ✅ | ✅ |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | ✅ | — | — | —（**非サイレント**） |

### API の使いやすさ（フロントエンド DX）

スコアは **SPA / npm 時代** のチーム向けの目安。Raw POS 専門家は QZ / JSPM を好む場合もあり。

| ツール | 英語対応 | オンラインデモ | npm パッケージ | Promise / `async` | 一行 HTML 印刷 | Vue / React 適合 | レイアウト | 学習コスト | 証明書 / 署名のハードル |
|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ [demo](https://demo.qz.io/) | ❌（スクリプト + WS） | Promise 化が一般的 | 可能、設定多め | 自前ラップ | ピクセル + raw 優先 | 中〜高 | サイレント時は高い |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ `web-print-pdf` | ✅ | ✅ | ✅ | HTML/CSS | 低〜中 | クライアント導入 |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | 部分的 / スクリプト中心 | 混合 | あり | 自前ラップ | 混合ペイロード | 中 | ライセンス + クライアント |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ⚠️ 中国語主 | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ❌ | コールバック寄り | レガシー寄りの API | 自前ラップ | 独自指令 + HTML | 中 | サービス / プラグイン導入 |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ⚠️ 中国語主 | ⚠️ デザイナーデモ中心 | エコシステムパッケージ | Socket.IO イベント | テンプレート経由 | vue-plugin-hiprint と相性良 | デザイナーテンプレート | 中 | クライアント導入 |
| [Zebra](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) / [Epson](https://download.epson-biz.com/) / [Star](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) SDKs | ✅ | ⚠️ ベンダーサンプル | ベンダースクリプト | 製品による | 否 | 自前ラップ | デバイス指令セット | ハードウェア依存 | ベンダースタック |
| [PrintNode](https://www.printnode.com/en) | ✅ | ⚠️ API ドキュメント | REST / バインディング | ✅ | PDF/raw 寄り | バックエンド向き | ファイル / raw | 中 | アカウント + クライアント |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | 薄いヘルパー | ダイアログを開く | 容易 | ブラウザ印刷 CSS | 低 | 非適用 — **非サイレント** |

ペイロード（HTML vs raw）、OS カバレッジ、署名 / ライセンスのハードルに合わせてスタックを選んでください。記号は目安です。必ずベンダーサイトで確認してください。

---

## ローカル印刷ブリッジ

小さなローカルランタイムをインストールし、HTTP / WebSocket / ネイティブ API をページに公開するクロスブラウザソリューション。

- [QZ Tray](https://qz.io/) — 成熟したローカルブリッジ。raw + ピクセル印刷。POS / ラベル印刷で広く利用。サイレントモードには通常署名 / ライセンスが必要。
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — HTML/PDF サイレント印刷向けのローカルエージェント + npm SDK。Windows、macOS、Linux 対応。
- [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) — 商用 JS + クライアント。マルチ OS に強い。WebSocket サイレント印刷 / スキャン。
- [Lodop / C-Lodop](http://www.c-lodop.com/) — 長年使われてきたローカル印刷コントロール。Windows ERP/HIS で一般的。Lodop7 で Linux サポート拡大。
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint)（+[vue-plugin-hiprint](https://github.com/CcSimple/vue-plugin-hiprint)）— オープンソース hiprint デザイナー + Electron サイレントクライアント。
- [PortixOne](https://github.com/portixhq/portixone) — Web アプリとローカルハードウェアを接続するオープンソースエッジランタイム（初期段階）。
- [PrintBridge](https://printbridge.app/) — ローカル REST サイレント印刷 API を提供する商用 Windows トレイエージェント。*（下記 OSS リポジトリとは別物。）*
- [SilentPrint](https://github.com/wxingheng/SilentPrint) — Web ページからサイレント印刷する Windows ミドルウェア。
- [PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket) — POS / サーマルサイレント印刷向け Python WebSocket サーバー + JS クライアント。
- [silent-print](https://github.com/atefe-aa/silent-print) — HTML サイレント印刷用のローカル HTTP API を公開する Windows サービス。

---

## ハードウェアベンダー SDK

プリンターフリートが主に同一ブランドの場合に最適。

- [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) — Zebra 向けブラウザ印刷（ローカルサービス + JS）。
- [Epson ePOS SDK for JavaScript](https://download.epson-biz.com/) — ページからネットワーク経由で Epson TM を制御。
- [Star Micronics webPRNT](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) — Star プリンターを制御する JS を組み込み。

---

## クラウド / リモート印刷

- [PrintNode](https://www.printnode.com/en) — クラウド API → ローカルクライアント → プリンター。Google Cloud Print の代替として一般的。
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — オプションのリモートジョブ取得付きローカルエージェント。
- [node-hiprint-transit](https://github.com/Xavier9896/node-hiprint-transit) — ネットワーク越しの hiprint クライアント向けリレー。
- Google Cloud Print — **サービス終了**。歴史的な文脈としてのみ記載。

---

## デスクトップ / Electron

- Electron `webContents.print({ silent: true })` — 制御可能なデスクトップシェル内では有効。任意の Web サイトからは利用不可。
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) — サイレント印刷ブリッジとして使う Electron クライアント。
- [electron-silent-print](https://github.com/mpoapostolis/electron-silent-print) — 初期の Electron サイレント印刷サンプル。

---

## オープンソースプロジェクト

SDK や出発点として有用な MIT / コミュニティリポジトリ（品質とメンテナンスは様々）。

- [weixiaoyi/PrintWeb](https://github.com/weixiaoyi/PrintWeb)
- [wxingheng/SilentPrint](https://github.com/wxingheng/SilentPrint)
- [TawsifTorabi/PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket)
- [atefe-aa/silent-print](https://github.com/atefe-aa/silent-print)
- [portixhq/portixone](https://github.com/portixhq/portixone)
- [AnouarSbia/printbridge](https://github.com/AnouarSbia/printbridge) — OSS エージェント（PDF / TSPL）。**printbridge.app とは別物**
- [CcSimple/electron-hiprint](https://github.com/CcSimple/electron-hiprint) — [ローカル印刷ブリッジ](#ローカル印刷ブリッジ) にも記載。

---

## サイレントではない（よくある混同）

同じ検索結果に出てきますが、**単体では**真のサイレント印刷を提供しません：

- [Print.js](https://printjs.crabbly.com/) — ブラウザ印刷ダイアログのヘルパー
- jsPDF / html2pdf.js — PDF の生成またはダウンロード。ローカルサイレントプリンターは制御しない
- `window.print()` — [ブラウザの制限](#ブラウザの制限先に読む) を参照

---

## セキュリティ

- サイレント印刷はユーザー確認 UI をバイパスする — ローカルブリッジを **特権ソフトウェア** として扱うこと。
- 認証付き localhost API、固定オリジン、署名付きクライアントを推奨。
- 強固な認証なしに raw 印刷エージェントをパブリックインターネットに公開しないこと。

---

## 貢献

PR 歓迎。エントリは事実ベースで：名称、リンク、一行説明、主な制約（OS、ライセンス、ハードウェアロックイン）。詳細は [CONTRIBUTING.md](CONTRIBUTING.md)。

---

## 翻訳

| 言語 | ファイル | 状態 |
|---|---|---|
| English | [README.md](README.md) | 完了（正本） |
| 中文 | [README.zh-CN.md](README.zh-CN.md) | 完了 |
| 日本語 | [README.ja.md](README.ja.md) | 完了 |
| Español | [README.es.md](README.es.md) | 完了 |
| Português (Brasil) | [README.pt-BR.md](README.pt-BR.md) | 完了 |
| 한국어 | [README.ko.md](README.ko.md) | 完了 |
| Deutsch | [README.de.md](README.de.md) | 完了 |
| Русский | [README.ru.md](README.ru.md) | 完了 |

上部の言語切り替えには、翻訳が完了したすべての言語が表示されています。

---

## ライセンス

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

法律で認められる範囲において、本リストは [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) の下で公開されています。
