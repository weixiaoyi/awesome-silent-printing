<div align="center">

# Awesome Silent Printing

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

**English** | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [Español](README.es.md) | [Português (Brasil)](README.pt-BR.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Русский](README.ru.md)

</div>

> A curated list of tools, libraries, bridges, and resources for **silent printing from web applications** — printing without the browser print dialog.
>
> Also covered: `window.print` limitations, Chrome Local Network Access to `127.0.0.1`, Vue/React silent print, HTML/CSS print agents, Lodop alternatives, QZ Tray / web-print-pdf / JSPrintManager comparisons, batch labels, and remote print.

---

## Why this exists

Browsers intentionally block silent printing for security. Teams that need receipts, shipping labels, invoices, or kitchen tickets usually end up with a **local bridge**, **extension**, **kiosk policy**, or **vendor SDK**.

This list focuses on that narrow problem: **how to print from the web without `window.print()` dialogs**.

---

## Guides

Practical long-form notes. Full index: [docs/](docs/README.md).

- [Browser silent print hub](docs/browser-silent-print.md)
- [How silent printing works](docs/how-silent-printing-works.md)
- [Choose a silent print stack](docs/choose-silent-print-stack.md)
- [window.print vs silent print](docs/window-print-vs-silent-print.md)
- [Chrome Local Network Access & 127.0.0.1](docs/chrome-local-network-access.md)
- [Silent print in Vue / React](docs/vue-react-silent-print.md)
- [HTML/CSS silent print](docs/html-css-silent-print.md)
- [Batch & label printing from the web](docs/batch-label-printing.md)
- [Remote / server-pushed silent print](docs/remote-silent-print.md)
- [QZ Tray vs web-print-pdf vs JSPrintManager](docs/qz-vs-jspm-vs-web-print-pdf.md)
- [Lodop alternatives](docs/lodop-alternatives.md)
- [hiprint alternatives](docs/hiprint-alternatives.md)
- [QZ Tray alternatives](docs/qz-tray-alternatives.md)
- [JSPrintManager alternatives](docs/jsprintmanager-alternatives.md)
- [Thermal receipt silent print from the browser](docs/thermal-receipt-silent-print.md)

---

## Contents

- [Guides](#guides)
- [Browser limits (read first)](#browser-limits-read-first)
- [How silent printing works](#how-silent-printing-works)
- [How to choose](#how-to-choose)
- [Comparison matrix](#comparison-matrix)
- [Local print bridges](#local-print-bridges)
- [Hardware vendor SDKs](#hardware-vendor-sdks)
- [Cloud / remote print](#cloud--remote-print)
- [Desktop / Electron](#desktop--electron)
- [Open source projects](#open-source-projects)
- [Not silent (common mix-ups)](#not-silent-common-mix-ups)
- [Security notes](#security-notes)
- [Contributing](#contributing)
- [Translations](#translations)

---

## Browser limits (read first)

| Mechanism | Silent? | Notes |
|---|---|---|
| `window.print()` | No (by default) | Browser shows a print dialog; page JS cannot fully drive a chosen device silently |
| Print.js / react-to-print | No | Still opens the browser print UI |
| Chrome kiosk / enterprise print policies | Conditional | Works on managed / kiosk devices only |
| Chrome / Edge **Local Network Access (LNA)** to `127.0.0.1` | Affects local bridges | Non-local pages need a **secure context (HTTPS)** to reach loopback; plain HTTP is often **silently denied**. Newer Chromium builds also apply this to **WebSocket** (`ws://127.0.0.1…`). Users may see a Local Network permission prompt. Dev on `localhost` is usually fine; production HTTP breaks many agents. |
| True cross-site silent print | Needs a local agent | Typical pattern: localhost HTTP/WebSocket / Native Messaging → OS spooler or raw port |

This LNA behavior matters for **every** localhost print bridge (QZ, web-print-pdf, JSPM, Lodop cloud-to-local patterns, etc.), not one vendor. Deeper walkthrough: [WebSocket to 127.0.0.1 failed after deploy](https://webprintpdf.com/en/docs/production-print-troubleshoot/).

---

## How silent printing works

| Approach | Idea | Common trade-off |
|---|---|---|
| Local bridge / agent | Page talks to a localhost service that owns the printer | Requires installing a client |
| Browser extension + native host | Extension calls a native messaging host | Store review + trust friction |
| Enterprise / kiosk policy | Lock browser print settings | Best for controlled devices |
| Vendor SDK | Talk to Epson / Zebra / Star stack | Hardware lock-in |
| Desktop shell (Electron etc.) | Embed Chromium; use native print APIs | Not a pure browser app |

---

## How to choose

1. **Mature global POS / raw + pixel ecosystem** → [QZ Tray](https://qz.io/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/).
2. **HTML/CSS business docs from a SPA (Vue / React, etc.)** → compare local bridges that accept HTML/PDF, e.g. QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, [Lodop / C-Lodop](http://www.c-lodop.com/), [electron-hiprint](https://github.com/CcSimple/electron-hiprint).
3. **Already on a legacy print control** → keep evaluating Lodop / hiprint stacks; migrate only when OS coverage or SPA DX becomes a blocker.
4. **Linux desktops (incl. Kylin / UOS where needed)** → prefer bridges with real Linux clients (QZ Tray, web-print-pdf, JSPrintManager, and similar).
5. **Only Zebra / Epson / Star printers** → prefer the matching [vendor SDK](#hardware-vendor-sdks).
6. **Need English-first docs / UI for a global team** → prefer tools marked ✅ under **English** in the [comparison matrix](#comparison-matrix); Lodop and many hiprint materials are Chinese-primary.
7. **Cloud API → many site printers** → [PrintNode](https://www.printnode.com/en) or a remote-capable local agent.
8. **Open-source SDK / learning** → see [Open source projects](#open-source-projects); smaller repos may be unevenly maintained.

---

## Comparison matrix

### Platform & payload

| Tool | Win | macOS | Linux | English | Online demo | HTML/CSS | PDF | Raw (ESC/POS, ZPL…) | Batch | Remote |
|---|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://demo.qz.io/) | ✅ | ✅ | ✅ Strong | ✅ | Via app |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ | ✅ | Via HTML/PDF | ✅ | ✅ |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | ✅ | ✅ | ✅ Strong | ✅ | Via product |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ✅ | — | Partial | ⚠️ CN-primary | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ✅ | ✅ | Partial | ✅ | Cloud modes |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ✅ | ✅ | ✅ | ⚠️ CN-primary | ⚠️ Designer demos | ✅ | ✅ | — | ✅ | Via transit |
| [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) | ✅ | ✅ | — | ✅ | ⚠️ Samples / local | — | Image | ZPL/raw | Limited | — |
| [PrintNode](https://www.printnode.com/en) | ✅ | ✅ | ✅ | ✅ | ⚠️ API docs | — | ✅ | ✅ | ✅ | ✅ |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | ✅ | — | — | — (**not silent**) |

### API friendliness (frontend DX)

Scores are indicative for **SPA / npm-era** teams. Raw-POS specialists may prefer QZ / JSPM regardless.

| Tool | English | Online demo | npm package | Promise / `async` | One-line HTML print | Vue / React fit | Layout model | Learning curve | Cert / signing friction |
|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ [demo](https://demo.qz.io/) | ❌ (script + WS) | Wrappers common | Possible, more setup | Manual | Pixel + raw first | Medium–high | High for silent |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ `web-print-pdf` | ✅ | ✅ | ✅ | HTML/CSS | Low–medium | Client install |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | Partial / script-centric | Mixed | Yes | Manual | Mixed payloads | Medium | License + client |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ⚠️ CN-primary | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ❌ | Callback-style | Legacy-style APIs | Manual | Proprietary + HTML | Medium | Service / plugin install |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ⚠️ CN-primary | ⚠️ Designer demos | Ecosystem packages | Socket.IO events | Via templates | Strong with vue-plugin-hiprint | Designer templates | Medium | Client install |
| [Zebra](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) / [Epson](https://download.epson-biz.com/) / [Star](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) SDKs | ✅ | ⚠️ Vendor samples | Vendor scripts | Varies | No | Manual | Device command sets | Hardware-specific | Vendor stack |
| [PrintNode](https://www.printnode.com/en) | ✅ | ⚠️ API docs | REST / bindings | ✅ | PDF/raw oriented | Backend-friendly | Files / raw | Medium | Account + client |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | Thin helpers | Triggers dialog | Easy | Browser print CSS | Low | N/A — **not silent** |

Match the stack to payload (HTML vs raw), OS coverage, and signing / license friction. Symbols are indicative; always verify on the vendor site.

---

## Local print bridges

Cross-browser solutions that install a small local runtime and expose HTTP / WebSocket / native APIs to the page.

- [QZ Tray](https://qz.io/) — Mature local bridge; raw + pixel printing; widely used in POS / labeling. Silent mode typically needs signing / licensing.
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — Local agent + npm SDK for HTML/PDF silent print; Windows, macOS, and Linux.
- [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) — Commercial JS + client; strong multi-OS story; WebSocket silent print / scan.
- [Lodop / C-Lodop](http://www.c-lodop.com/) — Long-standing local print control; common in Windows ERP/HIS deployments; Lodop7 adds more Linux support.
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) (+ [vue-plugin-hiprint](https://github.com/CcSimple/vue-plugin-hiprint)) — Open-source hiprint designer + Electron silent client.
- [PortixOne](https://github.com/portixhq/portixone) — Open-source edge runtime for connecting web apps to local hardware (early).
- [PrintBridge](https://printbridge.app/) — Commercial Windows tray agent with local REST silent print API. *(Not the same as the OSS repo below.)*
- [SilentPrint](https://github.com/wxingheng/SilentPrint) — Windows middleware for silent print from web pages.
- [PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket) — Python WebSocket server + JS client for POS / thermal silent print.
- [silent-print](https://github.com/atefe-aa/silent-print) — Windows service exposing a local HTTP API for silent HTML printing.

---

## Hardware vendor SDKs

Best when your fleet is mostly one hardware brand.

- [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) — Zebra-focused browser printing (local service + JS).
- [Epson ePOS SDK for JavaScript](https://download.epson-biz.com/) — Drive Epson TM over network from the page.
- [Star Micronics webPRNT](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) — Embed JS to control Star printers.

---

## Cloud / remote print

- [PrintNode](https://www.printnode.com/en) — Cloud API → local client → printer; common Google Cloud Print replacement.
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — Local agent with optional remote job pull.
- [node-hiprint-transit](https://github.com/Xavier9896/node-hiprint-transit) — Relay for hiprint clients across networks.
- Google Cloud Print — **Shut down**; listed only as historical context.

---

## Desktop / Electron

- Electron `webContents.print({ silent: true })` — Works inside a desktop shell you control; not available to arbitrary websites.
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) — Electron client used as a silent print bridge.
- [electron-silent-print](https://github.com/mpoapostolis/electron-silent-print) — Early Electron silent-print example.

---

## Open source projects

MIT / community repos useful as SDKs or starting points (quality and maintenance vary).

- [weixiaoyi/PrintWeb](https://github.com/weixiaoyi/PrintWeb)
- [wxingheng/SilentPrint](https://github.com/wxingheng/SilentPrint)
- [TawsifTorabi/PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket)
- [atefe-aa/silent-print](https://github.com/atefe-aa/silent-print)
- [portixhq/portixone](https://github.com/portixhq/portixone)
- [AnouarSbia/printbridge](https://github.com/AnouarSbia/printbridge) — OSS agent (PDF / TSPL); **not** printbridge.app
- [CcSimple/electron-hiprint](https://github.com/CcSimple/electron-hiprint) — Also listed under [Local print bridges](#local-print-bridges).

---

## Not silent (common mix-ups)

These show up in the same searches but **do not** provide true silent printing by themselves:

- [Print.js](https://printjs.crabbly.com/) — Helper around the browser print dialog
- jsPDF / html2pdf.js — Generate or download PDFs; do not drive a local silent printer
- `window.print()` — See [Browser limits](#browser-limits-read-first)

---

## Security notes

- Silent printing bypasses a user confirmation UI — treat the local bridge as **privileged software**.
- Prefer authenticated localhost APIs, pinned origins, and signed clients.
- Never expose a raw print agent to the public internet without strong auth.

---

## Contributing

PRs welcome. Please keep entries factual: name, link, one-line description, and notable constraints (OS, license, hardware lock-in). See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Translations

| Language | File | Status |
|---|---|---|
| English | [README.md](README.md) | Done (canonical) |
| 中文 | [README.zh-CN.md](README.zh-CN.md) | Done |
| 日本語 | [README.ja.md](README.ja.md) | Done |
| Español | [README.es.md](README.es.md) | Done |
| Português (Brasil) | [README.pt-BR.md](README.pt-BR.md) | Done |
| 한국어 | [README.ko.md](README.ko.md) | Done |
| Deutsch | [README.de.md](README.de.md) | Done |
| Русский | [README.ru.md](README.ru.md) | Done |

Long-form guides under `docs/` are available in all README languages.

---

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, this list is released under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/).
