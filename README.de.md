<div align="center">

# Awesome Silent Printing

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [Español](README.es.md) | [Português (Brasil)](README.pt-BR.md) | [한국어](README.ko.md) | **Deutsch** | [Русский](README.ru.md)

</div>

> Eine kuratierte Liste von Tools, Bibliotheken, Bridges und Ressourcen für **stillen Druck (Silent Printing) aus Webanwendungen** — Drucken ohne den Browser-Druckdialog.
>
> Ebenfalls abgedeckt: `window.print`-Einschränkungen, Chrome Local Network Access zu `127.0.0.1`, Vue/React Silent Printing, HTML/CSS-Print-Agents, Lodop-Alternativen, QZ Tray / web-print-pdf / JSPrintManager-Vergleiche, Batch-Labels und Remote-Druck.

---

## Warum diese Liste

Browser blockieren stillen Druck absichtlich aus Sicherheitsgründen. Teams, die Belege, Versandetiketten, Rechnungen oder Küchenbons brauchen, landen meist bei einer **lokalen Bridge**, **Browser-Erweiterung**, **Kiosk-Richtlinie** oder **Hersteller-SDK**.

Diese Liste konzentriert sich auf genau dieses enge Problem: **Wie druckt man aus dem Web ohne `window.print()`-Dialoge?**

---

## Leitfäden

Praktische ausführliche Hinweise. Vollständiger Index: [docs/](docs/README.de.md).

- [Browser Silent Print Hub](docs/browser-silent-print.de.md)
- [How silent printing works](docs/how-silent-printing-works.de.md)
- [Choose a silent print stack](docs/choose-silent-print-stack.de.md)
- [window.print vs silent print](docs/window-print-vs-silent-print.de.md)
- [Chrome Local Network Access & 127.0.0.1](docs/chrome-local-network-access.de.md)
- [Silent print in Vue / React](docs/vue-react-silent-print.de.md)
- [HTML/CSS silent print](docs/html-css-silent-print.de.md)
- [Batch & label printing from the web](docs/batch-label-printing.de.md)
- [Remote / server-pushed silent print](docs/remote-silent-print.de.md)
- [QZ Tray vs web-print-pdf vs JSPrintManager](docs/qz-vs-jspm-vs-web-print-pdf.de.md)
- [Lodop alternatives](docs/lodop-alternatives.de.md)
- [hiprint alternatives](docs/hiprint-alternatives.de.md)
- [QZ Tray alternatives](docs/qz-tray-alternatives.de.md)
- [JSPrintManager alternatives](docs/jsprintmanager-alternatives.de.md)
- [Thermal receipt silent print from the browser](docs/thermal-receipt-silent-print.de.md)

---

## Inhalt

- [Leitfäden](#leitfäden)
- [Browser-Limits (zuerst lesen)](#browser-limits-zuerst-lesen)
- [So funktioniert stiller Druck](#so-funktioniert-stiller-druck)
- [Auswahlhilfe](#auswahlhilfe)
- [Vergleichsmatrix](#vergleichsmatrix)
- [Lokale Print-Bridges](#lokale-print-bridges)
- [Hardware-Hersteller-SDKs](#hardware-hersteller-sdks)
- [Cloud / Remote-Druck](#cloud--remote-druck)
- [Desktop / Electron](#desktop--electron)
- [Open-Source-Projekte](#open-source-projekte)
- [Nicht still (häufige Verwechslungen)](#nicht-still-häufige-verwechslungen)
- [Sicherheitshinweise](#sicherheitshinweise)
- [Mitwirken](#mitwirken)
- [Übersetzungen](#übersetzungen)

---

## Browser-Limits (zuerst lesen)

| Mechanismus | Still? | Hinweise |
|---|---|---|
| `window.print()` | Nein (standardmäßig) | Der Browser zeigt einen Druckdialog; Seiten-JS kann kein Gerät vollständig still ansteuern |
| Print.js / react-to-print | Nein | Öffnet weiterhin die Browser-Druckoberfläche |
| Chrome-Kiosk- / Enterprise-Druckrichtlinien | Bedingt | Funktioniert nur auf verwalteten / Kiosk-Geräten |
| Chrome / Edge **Local Network Access (LNA)** zu `127.0.0.1` | Betrifft lokale Bridges | Nicht-lokale Seiten brauchen einen **sicheren Kontext (HTTPS)**, um Loopback zu erreichen; reines HTTP wird oft **still abgelehnt**. Neuere Chromium-Builds wenden dies auch auf **WebSocket** an (`ws://127.0.0.1…`). Nutzer sehen ggf. eine Local-Network-Berechtigungsabfrage. Entwicklung auf `localhost` ist meist unproblematisch; HTTP in Produktion bricht viele Agents. |
| Echter siteübergreifender stiller Druck | Braucht lokalen Agent | Typisches Muster: localhost HTTP/WebSocket / Native Messaging → OS-Spooler oder Raw-Port |

Dieses LNA-Verhalten betrifft **jede** localhost-Print-Bridge (QZ, web-print-pdf, JSPM, Lodop-Cloud-to-Local-Muster usw.), nicht nur einen Anbieter. Ausführlicher: [WebSocket to 127.0.0.1 failed after deploy](https://webprintpdf.com/en/docs/production-print-troubleshoot/).

---

## So funktioniert stiller Druck

| Ansatz | Idee | Typischer Kompromiss |
|---|---|---|
| Lokale Bridge / Agent | Seite spricht mit einem localhost-Dienst, der den Drucker steuert | Client-Installation nötig |
| Browser-Erweiterung + Native Host | Erweiterung ruft Native-Messaging-Host auf | Store-Review + Vertrauenshürde |
| Enterprise / Kiosk-Richtlinie | Browser-Druckeinstellungen sperren | Am besten für kontrollierte Geräte |
| Hersteller-SDK | Anbindung an Epson / Zebra / Star | Hardware-Lock-in |
| Desktop-Shell (Electron usw.) | Chromium einbetten; native Print-APIs nutzen | Keine reine Browser-App |

---

## Auswahlhilfe

1. **Ausgereiftes globales POS- / Raw- + Pixel-Ökosystem** → [QZ Tray](https://qz.io/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/).
2. **HTML/CSS-Geschäftsdokumente aus einer SPA (Vue / React usw.)** → Lokale Bridges vergleichen, die HTML/PDF akzeptieren, z. B. QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, [Lodop / C-Lodop](http://www.c-lodop.com/), [electron-hiprint](https://github.com/CcSimple/electron-hiprint).
3. **Bereits ein Legacy-Print-Control im Einsatz** → Lodop / hiprint-Stacks weiter evaluieren; nur migrieren, wenn OS-Abdeckung oder SPA-DX zum Engpass wird.
4. **Linux-Desktops (inkl. Kylin / UOS falls nötig)** → Bridges mit echten Linux-Clients bevorzugen (QZ Tray, web-print-pdf, JSPrintManager und ähnliche).
5. **Nur Zebra / Epson / Star-Drucker** → Passendes [Hersteller-SDK](#hardware-hersteller-sdks) bevorzugen.
6. **Englisch-first-Docs / UI für globales Team** → Tools mit ✅ unter **English** in der [Vergleichsmatrix](#vergleichsmatrix) bevorzugen; Lodop und viele hiprint-Materialien sind primär chinesisch.
7. **Cloud-API → viele Standort-Drucker** → [PrintNode](https://www.printnode.com/en) oder ein remote-fähiger lokaler Agent.
8. **Open-Source-SDK / Lernen** → Siehe [Open-Source-Projekte](#open-source-projekte); kleinere Repos können ungleichmäßig gepflegt sein.

---

## Vergleichsmatrix

### Plattform & Nutzlast

| Werkzeug | Win | macOS | Linux | Englisch | Online-Demo | HTML/CSS | PDF | Raw (ESC/POS, ZPL…) | Batch | Remote |
|---|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://demo.qz.io/) | ✅ | ✅ | ✅ Stark | ✅ | Über App |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ | ✅ | Über HTML/PDF | ✅ | ✅ |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | ✅ | ✅ | ✅ Stark | ✅ | Über Produkt |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ✅ | — | Teilweise | ⚠️ CN-primär | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ✅ | ✅ | Teilweise | ✅ | Cloud-Modi |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ✅ | ✅ | ✅ | ⚠️ CN-primär | ⚠️ Designer-Demos | ✅ | ✅ | — | ✅ | Über Transit |
| [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) | ✅ | ✅ | — | ✅ | ⚠️ Samples / lokal | — | Image | ZPL/raw | Begrenzt | — |
| [PrintNode](https://www.printnode.com/en) | ✅ | ✅ | ✅ | ✅ | ⚠️ API-Docs | — | ✅ | ✅ | ✅ | ✅ |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | ✅ | — | — | — (**nicht still**) |

### API-Freundlichkeit (Frontend-DX)

Bewertungen sind indikativ für **SPA- / npm-Ära**-Teams. Raw-POS-Spezialisten bevorzugen ggf. trotzdem QZ / JSPM.

| Werkzeug | Englisch | Online-Demo | npm-Paket | Promise / `async` | Einzeiler HTML-Druck | Vue / React-Tauglichkeit | Layout-Modell | Lernkurve | Zertifikat- / Signatur-Hürde |
|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ [demo](https://demo.qz.io/) | ❌ (Script + WS) | Wrapper üblich | Möglich, mehr Setup | Manuell | Pixel + raw zuerst | Mittel–hoch | Hoch für still |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ `web-print-pdf` | ✅ | ✅ | ✅ | HTML/CSS | Niedrig–mittel | Client-Installation |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | Teilweise / script-zentriert | Gemischt | Ja | Manuell | Gemischte Nutzlast | Mittel | Lizenz + Client |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ⚠️ CN-primär | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ❌ | Callback-Stil | Legacy-APIs | Manuell | Proprietär + HTML | Mittel | Service / Plugin-Installation |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ⚠️ CN-primär | ⚠️ Designer-Demos | Ökosystem-Pakete | Socket.IO-Events | Über Templates | Stark mit vue-plugin-hiprint | Designer-Templates | Mittel | Client-Installation |
| [Zebra](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) / [Epson](https://download.epson-biz.com/) / [Star](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) SDKs | ✅ | ⚠️ Hersteller-Samples | Hersteller-Scripts | Variiert | Nein | Manuell | Gerätebefehlssätze | Hardware-spezifisch | Hersteller-Stack |
| [PrintNode](https://www.printnode.com/en) | ✅ | ⚠️ API-Docs | REST / Bindings | ✅ | PDF/raw-orientiert | Backend-freundlich | Dateien / raw | Mittel | Konto + Client |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | Dünne Helfer | Löst Dialog aus | Einfach | Browser-Print-CSS | Niedrig | N/A — **nicht still** |

Stack an Nutzlast (HTML vs. raw), OS-Abdeckung und Signatur- / Lizenz-Hürde anpassen. Symbole sind indikativ; immer auf der Herstellerseite prüfen.

---

## Lokale Print-Bridges

Browserübergreifende Lösungen, die eine kleine lokale Runtime installieren und HTTP / WebSocket / native APIs an die Seite exponieren.

- [QZ Tray](https://qz.io/) — Ausgereifte lokale Bridge; Raw- + Pixel-Druck; weit verbreitet in POS / Labeling. Stiller Modus braucht typischerweise Signierung / Lizenzierung.
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — Lokaler Agent + npm-SDK für HTML/PDF-Silent Printing; Windows, macOS und Linux.
- [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) — Kommerzielles JS + Client; starke Multi-OS-Story; WebSocket Silent Print / Scan.
- [Lodop / C-Lodop](http://www.c-lodop.com/) — Lang etabliertes lokales Print-Control; verbreitet in Windows-ERP/HIS; Lodop7 erweitert Linux-Support.
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) (+ [vue-plugin-hiprint](https://github.com/CcSimple/vue-plugin-hiprint)) — Open-Source-hiprint-Designer + Electron-Silent-Client.
- [PortixOne](https://github.com/portixhq/portixone) — Open-Source-Edge-Runtime zur Verbindung von Web-Apps mit lokaler Hardware (früh).
- [PrintBridge](https://printbridge.app/) — Kommerzieller Windows-Tray-Agent mit lokaler REST-Silent-Print-API. *(Nicht dasselbe wie das OSS-Repo unten.)*
- [SilentPrint](https://github.com/wxingheng/SilentPrint) — Windows-Middleware für stillen Druck aus Webseiten.
- [PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket) — Python-WebSocket-Server + JS-Client für POS- / Thermo-Silent Printing.
- [silent-print](https://github.com/atefe-aa/silent-print) — Windows-Dienst mit lokaler HTTP-API für stillen HTML-Druck.

---

## Hardware-Hersteller-SDKs

Am besten, wenn Ihre Flotte überwiegend eine Hardwaremarke nutzt.

- [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) — Zebra-fokussiertes Browser-Printing (lokaler Dienst + JS).
- [Epson ePOS SDK for JavaScript](https://download.epson-biz.com/) — Epson TM über das Netzwerk von der Seite aus ansteuern.
- [Star Micronics webPRNT](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) — JS einbetten, um Star-Drucker zu steuern.

---

## Cloud / Remote-Druck

- [PrintNode](https://www.printnode.com/en) — Cloud-API → lokaler Client → Drucker; gängiger Google Cloud Print-Ersatz.
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — Lokaler Agent mit optionalem Remote-Job-Pull.
- [node-hiprint-transit](https://github.com/Xavier9896/node-hiprint-transit) — Relay für hiprint-Clients über Netzwerke.
- Google Cloud Print — **Eingestellt**; nur als historischer Kontext aufgeführt.

---

## Desktop / Electron

- Electron `webContents.print({ silent: true })` — Funktioniert in einer Desktop-Shell, die Sie kontrollieren; nicht für beliebige Websites verfügbar.
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) — Electron-Client als Silent-Print-Bridge.
- [electron-silent-print](https://github.com/mpoapostolis/electron-silent-print) — Frühes Electron-Silent-Print-Beispiel.

---

## Open-Source-Projekte

MIT- / Community-Repos, nützlich als SDK oder Startpunkt (Qualität und Wartung variieren).

- [weixiaoyi/PrintWeb](https://github.com/weixiaoyi/PrintWeb)
- [wxingheng/SilentPrint](https://github.com/wxingheng/SilentPrint)
- [TawsifTorabi/PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket)
- [atefe-aa/silent-print](https://github.com/atefe-aa/silent-print)
- [portixhq/portixone](https://github.com/portixhq/portixone)
- [AnouarSbia/printbridge](https://github.com/AnouarSbia/printbridge) — OSS-Agent (PDF / TSPL); **nicht** printbridge.app
- [CcSimple/electron-hiprint](https://github.com/CcSimple/electron-hiprint) — Auch unter [Lokale Print-Bridges](#lokale-print-bridges) aufgeführt.

---

## Nicht still (häufige Verwechslungen)

Diese tauchen in denselben Suchen auf, bieten aber **keinen** echten stillen Druck allein:

- [Print.js](https://printjs.crabbly.com/) — Helfer um den Browser-Druckdialog
- jsPDF / html2pdf.js — PDFs erzeugen oder herunterladen; steuern keinen lokalen stillen Drucker
- `window.print()` — Siehe [Browser-Limits](#browser-limits-zuerst-lesen)

---

## Sicherheitshinweise

- Stiller Druck umgeht eine Nutzerbestätigungs-UI — behandeln Sie die lokale Bridge als **privilegierte Software**.
- Authentifizierte localhost-APIs, festgelegte Origins und signierte Clients bevorzugen.
- Niemals einen Raw-Print-Agent ohne starke Authentifizierung ins öffentliche Internet stellen.

---

## Mitwirken

PRs willkommen. Bitte Einträge sachlich halten: Name, Link, Einzeiler-Beschreibung und relevante Einschränkungen (OS, Lizenz, Hardware-Lock-in). Siehe [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Übersetzungen

| Sprache | Datei | Status |
|---|---|---|
| English | [README.md](README.md) | Fertig (kanonisch) |
| 中文 | [README.zh-CN.md](README.zh-CN.md) | Fertig |
| 日本語 | [README.ja.md](README.ja.md) | Fertig |
| Español | [README.es.md](README.es.md) | Fertig |
| Português (Brasil) | [README.pt-BR.md](README.pt-BR.md) | Fertig |
| 한국어 | [README.ko.md](README.ko.md) | Fertig |
| Deutsch | [README.de.md](README.de.md) | Fertig |
| Русский | [README.ru.md](README.ru.md) | Fertig |

Die Sprachumschaltung oben verlinkt alle verfügbaren Übersetzungen.

---

## Lizenz

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

Soweit rechtlich möglich, wird diese Liste unter [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/) veröffentlicht.
