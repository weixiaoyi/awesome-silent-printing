# Browser silent print

**Browser silent print** means a web page sends a job to a local printer **without** the browser print dialog.

This hub covers the usual paths people hit in practice:

- silent printing from a web app
- print without a dialog in Chrome
- Vue / React silent print
- alternatives to `window.print()`

## Short answer

A normal website cannot silently drive an arbitrary printer by itself. You need a **local agent**, **vendor SDK**, **kiosk policy**, or a **desktop shell**.

If someone claims “pure JavaScript silent print in Chrome for any printer,” ask which local component they install. That component is the real printer driver path.

## Mental model in one minute

```text
Page (HTTPS)
  → localhost bridge
  → OS spooler or raw port
  → physical printer
```

Details: [How silent printing works](how-silent-printing-works.md).

## Choose your next page

| Your situation | Read |
|---|---|
| Need the architecture | [How silent printing works](how-silent-printing-works.md) |
| Need to pick a stack | [Choose a silent print stack](choose-silent-print-stack.md) |
| Coming from `window.print` | [window.print vs silent print](window-print-vs-silent-print.md) |
| Production cannot reach `127.0.0.1` | [Chrome Local Network Access](chrome-local-network-access.md) |
| SPA integration | [Vue / React silent print](vue-react-silent-print.md) |
| HTML templates | [HTML/CSS silent print](html-css-silent-print.md) |
| Labels / warehouse volume | [Batch & label printing](batch-label-printing.md) |
| WMS pushes jobs to desks | [Remote silent print](remote-silent-print.md) |
| Receipt / kitchen tickets | [Thermal receipt silent print](thermal-receipt-silent-print.md) |
| Comparing major bridges | [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.md) |

## Common search → guide

| People look for | Start here |
|---|---|
| browser silent print / webpage silent print | This page |
| window.print without dialog | [window.print vs silent print](window-print-vs-silent-print.md) |
| Chrome websocket 127.0.0.1 failed | [Chrome LNA](chrome-local-network-access.md) |
| Lodop / hiprint / QZ alternative | [Lodop](lodop-alternatives.md) · [hiprint](hiprint-alternatives.md) · [QZ](qz-tray-alternatives.md) · [JSPM](jsprintmanager-alternatives.md) |

## Tool list

See the curated list: [Awesome Silent Printing](../README.md).
