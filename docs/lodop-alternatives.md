# Lodop alternatives

Lodop / C-Lodop is still common in many Windows business systems. Teams usually look for alternatives when they need:

- Modern SPA integration
- Broader desktop OS support (macOS / Linux desks)
- English-first docs for mixed teams
- Clearer HTTPS + localhost behavior under current Chromium rules

## Replacement directions

| Need | Candidates |
|---|---|
| HTML/CSS business docs | QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, hiprint + electron-hiprint |
| Raw POS / labels | [QZ Tray](https://qz.io/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| Cloud to many printers | [PrintNode](https://www.printnode.com/en) |
| Stay on Lodop | Lodop7 / C-Lodop if the stack still fits |

## Why migrations stall

- Templates are a mix of proprietary Lodop commands and HTML fragments
- Printer names and paper bins are encoded in old scripts
- Hospitals / ERPs fear changing a working print path during peak season

## Migration tips

1. Inventory templates: HTML vs proprietary commands. Count how many are “HTML-only already.”
2. Rebuild critical docs as HTML/CSS when possible; leave exotic raw for a second wave.
3. Run old and new agents in parallel on a pilot desk (different ports).
4. Fix HTTPS / Local Network Access before nationwide rollout.
5. Train helpdesk on the new “agent not running” symptom — it replaces old ActiveX-era errors.

## Related

- [Choose a stack](choose-silent-print-stack.md)
- [window.print vs silent print](window-print-vs-silent-print.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.md)
