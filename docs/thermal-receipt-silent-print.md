# Thermal receipt silent print from the browser

**Thermal receipt printing from a webpage** usually needs silent print: cashiers and kitchen stations cannot click through a dialog on every ticket.

## What works

1. **Local print bridge** — HTML/image/PDF for simple tickets, or ESC/POS raw for full device control
2. **Vendor SDK** for Epson / Star-class network printers (page talks to printer or vendor service)
3. **Desktop shell** with silent print if you ship a POS Electron app

## What does not work

- `window.print()` as a production silent path
- Pure PDF download libraries with no local printer agent
- Assuming every “80mm CSS page” will cut and open the drawer without raw commands

## Architecture choices

| Need | Lean toward |
|---|---|
| Logo + variable HTML layout, low volume | HTML via local agent |
| High volume kitchen, cutter, drawer, buzzer | ESC/POS raw on QZ / JSPM / vendor SDK |
| Only Epson/Star on LAN | Vendor ePOS / webPRNT style SDK |
| You already ship a POS desktop app | Electron silent print |

## Template tips

- Prefer fixed width layouts (e.g. **58mm / 80mm**)
- Keep barcodes high-contrast; test with the scanner you actually use
- Avoid heavy CSS grids; thermal engines and drivers are unforgiving
- Test cutter / cash drawer commands **only** on raw-capable stacks
- Encode code pages correctly for CJK / accented text on ESC/POS
- Print a calibration ticket after driver or agent updates

## Kitchen vs front counter

| | Front counter | Kitchen |
|---|---|---|
| Latency tolerance | Low | Very low |
| Typical payload | HTML or ESC/POS | Often ESC/POS |
| Failure UX | Show cashier a retry | Auto-retry + loud alert |
| Multi-printer | Receipt + label | Route by station / item |

## Related

- [Batch & label printing](batch-label-printing.md)
- [HTML/CSS silent print](html-css-silent-print.md)
- [Choose a silent print stack](choose-silent-print-stack.md)
- Tool list: [Awesome Silent Printing](../README.md)
