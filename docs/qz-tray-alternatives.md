# QZ Tray alternatives

Searching for a **QZ Tray alternative** usually means you want silent printing from the browser, but with a different trade-off on API style, pricing, signing, or HTML/CSS workflow.

## When to stay on QZ Tray

- Raw ESC/POS / ZPL is the main workload
- You already invested in QZ signing and certificates
- You need a long global POS track record
- Your team already wraps QZ WebSocket calls in production

## When to evaluate alternatives

| Need | Look at |
|---|---|
| Commercial JS client with broad file types | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| npm-style HTML/CSS from a SPA | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), and other HTML-capable bridges |
| Cloud API to many printers | [PrintNode](https://www.printnode.com/en) |
| Designer-led templates | hiprint + electron-hiprint |
| Single hardware brand | Zebra / Epson / Star vendor SDKs |

## Migration notes

1. Inventory which jobs are raw vs HTML/PDF — raw jobs are the costly rewrite.
2. Re-test signing / license assumptions; do not assume the next vendor is “unsigned silent.”
3. Keep one QZ desk alive while piloting the alternative.
4. Re-validate Chrome Local Network Access on the new agent port.

## Compare directly

- [QZ Tray vs web-print-pdf vs JSPrintManager](qz-vs-jspm-vs-web-print-pdf.md)
- [Choose a silent print stack](choose-silent-print-stack.md)
