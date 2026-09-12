# Choose a silent print stack

Use this when you already know you need **no browser print dialog**, and you need to pick an approach.

## Quick decision tree

1. **You control a desktop shell (Electron, etc.)**  
   Use the shell’s silent print API. You do not need a separate web bridge.

2. **Printers are almost all one brand (Zebra / Epson / Star)**  
   Prefer that vendor’s browser/network SDK first. You avoid a generic bridge and speak the device dialect directly.

3. **You need raw ESC/POS / ZPL dialects and a long global POS track record**  
   Evaluate [QZ Tray](https://qz.io/) and [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/).

4. **You want HTML/CSS business docs from Vue or React**  
   Compare local bridges that accept HTML/PDF — QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, Lodop / C-Lodop, electron-hiprint — and pick by OS coverage, API style, and license friction.

5. **Many sites, cloud API to local printers**  
   Look at [PrintNode](https://www.printnode.com/en) or a bridge with remote job pull. See [Remote silent print](remote-silent-print.md).

6. **You already run Lodop / C-Lodop**  
   Keep it if it works; plan migration when you need broader desktop OS support or a different SPA integration model. See [Lodop alternatives](lodop-alternatives.md).

## Scorecard (fill this before buying)

| Criterion | Your need | Notes |
|---|---|---|
| Payload | HTML / PDF / raw / mixed | Drives shortlist more than brand |
| OS | Win only / +macOS / +Linux | Drops many legacy controls |
| Language of docs | EN / CN / both | Matters for global teams |
| Volume | Few / batch / warehouse | Queue + retry requirements |
| Install friction | IT-managed / end-user self-serve | Signing, antivirus, permissions |
| Remote | Same LAN only / multi-site | Cloud vs agent pull |
| Budget | OSS / commercial license | Include support cost |

## Pilot plan (one week)

1. Pick **two** candidates from the shortlist, not five.
2. Install both agents on the same desk PC.
3. Print the same three templates: one A4 HTML, one label, one edge case (CJK + barcode).
4. Measure: install time, first successful print, failure messages, batch of 50.
5. Break HTTPS / LNA on purpose once, then fix it — so ops knows the runbook.
6. Keep the winner; uninstall the loser to avoid port fights.

## Red flags

- Vendor cannot show an online demo or a clear localhost architecture diagram
- “Silent” only means PDF download
- No story for Chrome Local Network Access on production HTTPS sites
- Raw-only stack when your team only knows HTML/CSS (or the reverse)

## Questions that matter more than brand names

| Question | Why it matters |
|---|---|
| HTML/CSS or raw commands? | Picks frontend-native vs device-native stacks |
| Windows only, or also macOS/Linux? | Eliminates many legacy controls |
| Need English docs for a global team? | Filters CN-primary stacks |
| Batch / queue? | Label and warehouse workflows |
| HTTPS production page → localhost agent? | Chrome Local Network Access |

## Related guides

- [How silent printing works](how-silent-printing-works.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.md)
- [Vue / React silent print](vue-react-silent-print.md)
- Tool list: [Awesome Silent Printing](../README.md)
