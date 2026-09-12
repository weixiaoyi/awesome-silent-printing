# window.print vs silent print

People often ask: can I call `window.print()` without a dialog?  
Short answer: **not in a normal browser webpage.**

Browsers treat printing as a privileged user action. The print UI is the safety valve. Libraries that wrap `window.print()` (for example Print.js or react-to-print) still end in that UI.

## Side-by-side

| | `window.print()` / Print.js | Silent print bridge |
|---|---|---|
| Dialog | Yes | No |
| Choose printer from JS | Limited / no | Yes (via local agent) |
| Batch jobs | Poor | Designed for queues |
| Named printer / tray / paper | User-driven | Agent API |
| Install something | No | Usually yes |
| Security model | Browser-controlled | Local privileged software |
| Works on unmanaged SaaS clients | Yes (with dialog) | Needs agent install |

## Myths that waste sprints

| Myth | Reality |
|---|---|
| “There must be a Chrome flag for SaaS users” | Flags/policies are for managed devices, not public visitors |
| “Puppeteer on the server is silent print” | It makes PDFs/images; it does not drive the user’s USB/network printer |
| “PDF download is close enough” | Users still manually print; no batch / named printer |
| “Electron silent APIs work in the browser build” | Those APIs exist only inside the desktop shell you ship |

## When `window.print()` is enough

- Occasional user-driven printing (statements the user expects to confirm)
- Legal / medical flows where an explicit confirm is desirable
- Low volume, no kiosk / warehouse automation
- You refuse to ship a local install

## When you need silent print

- Shipping labels, receipts, kitchen tickets
- Unattended or high-frequency jobs
- SPA workflows where a dialog breaks the UX (scan → print → next)
- Multi-printer desks (label vs A4 vs receipt) selected in software

## Migration path

1. **Inventory** what you print today: HTML screenshots, CSS `@media print`, PDF files, or raw.
2. **Keep HTML/CSS templates** when the target bridge can render them; rewrite only what must become ZPL/ESC/POS.
3. **Pick a local agent** (or vendor SDK / Electron) using [Choose a silent print stack](choose-silent-print-stack.md).
4. Replace `window.print()` call sites with an SDK call (HTML/PDF/raw over localhost).
5. Add **agent-not-installed** UX: detect connection, show install link, block the “Print” button until ready.
6. Deploy the site on **HTTPS** and verify [Local Network Access to `127.0.0.1`](chrome-local-network-access.md) on a clean workstation.
7. Pilot one desk for a week with logging before rolling out.

### Minimal code-shape change

Before:

```js
window.print();
```

After (pseudocode — APIs differ by vendor):

```js
await printAgent.printHtml(document.getElementById('label').outerHTML, {
  printer: selectedPrinter,
});
```

## Acceptance tests before you call it done

- [ ] Dialog never appears on the happy path
- [ ] Correct printer is selected without user clicks
- [ ] One bad job does not freeze the whole batch
- [ ] Fresh browser profile gets through HTTPS + LNA permission once
- [ ] Agent offline shows a recoverable error, not a hang

## Related

- [How silent printing works](how-silent-printing-works.md)
- [Choose a stack](choose-silent-print-stack.md)
- [Chrome Local Network Access](chrome-local-network-access.md)
- [Vue / React silent print](vue-react-silent-print.md)
