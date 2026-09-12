# HTML/CSS silent print

## Why teams want this

Most business UIs are already HTML/CSS. If silent print can reuse the same templates, frontend velocity stays high and you avoid maintaining a second ZPL/ESC/POS skill set for every invoice.

## Two layout models

| Model | Pros | Cons |
|---|---|---|
| HTML/CSS via local Chromium/agent | Familiar to web teams; good fidelity | Needs a local agent |
| Raw ESC/POS / ZPL | Precise device control | Different skill set; hardware-specific |

Many products mix both: A4 docs in HTML, thermal tickets in raw.

## What “print CSS” means here

Browser `@media print` alone is **not** a silent path — it still goes through `window.print()`. With a local agent you typically:

1. Build a self-contained HTML string (or URL).
2. Include the CSS the agent needs (inline, linked absolute URL, or bundled).
3. Pass paper size / margins / printer name in SDK options.
4. Let the agent’s engine (often Chromium) paginate and spool.

### Template checklist

- [ ] Explicit page size (A4, 100×150 mm, 80 mm roll, …)
- [ ] Margins that match the physical stock
- [ ] Fonts installed on the workstation or embedded
- [ ] Barcode/QR as SVG or high-res image (test scan)
- [ ] Tables that do not clip on the last row
- [ ] No dependency on viewport-only layouts (`100vh` traps)

## Practical tips

- Design a **print-only** stylesheet; do not reuse the full app chrome CSS blindly.
- Prefer `mm` / `in` units for labels; `px` alone drifts across DPI.
- Test Chinese / CJK fonts on Windows **and** Linux desks if you support both.
- Preview one job before enabling batch when migrating templates.
- Keep images small; giant PNGs kill batch throughput.
- For exact thermal cutters / cash drawers, you may still need raw commands on a raw-capable bridge.

## Good fit

Invoices, statements, packing lists, A4 reports, many label layouts rendered as HTML.

## Poor fit (consider raw / vendor SDK)

- Ultra-high-speed kitchen printers that expect ESC/POS only
- Zebra fleets standardized on ZPL templates in the printer firmware
- Devices with no usable Windows/macOS/Linux driver path except vendor raw

## Related

- [Vue / React silent print](vue-react-silent-print.md)
- [Batch & label printing](batch-label-printing.md)
- [Thermal receipt silent print](thermal-receipt-silent-print.md)
- [Choose a stack](choose-silent-print-stack.md)
