# How silent printing works

Browsers are designed to **stop** a webpage from quietly sending jobs to a physical printer. That is why `window.print()` shows a dialog, and why “silent print from pure JavaScript” is usually a dead end.

If you are building receipts, shipping labels, invoices, or kitchen tickets from a web app, you need a different architecture—not a browser flag.

## The real pattern

Almost every production silent-print setup looks like this:

```text
Web app (browser)
    → localhost HTTP / WebSocket / Native Messaging
    → local agent / vendor service / desktop shell
    → OS print spooler or raw device port
```

The page never owns the printer. A **trusted local component** does. That component is installed once per workstation (or baked into a kiosk image), then the web app talks to it as a local service.

### Why this exists

| Concern | Browser answer | Silent-print answer |
|---|---|---|
| Malicious sites printing spam | Block silent device access | User installs a known agent |
| Choosing the wrong printer | Force a dialog | Agent + named printer API |
| Raw ESC/POS / ZPL | Not available from the page | Agent or vendor SDK sends bytes |
| Batch / unattended jobs | Dialog breaks the flow | Queue on the agent or server |

## Five architectures

| Approach | Who installs what | Typical use | Trade-off |
|---|---|---|---|
| Local print bridge | Desktop agent + JS SDK | ERP, WMS, POS, labels | Best general web silent path |
| Extension + native host | Browser extension + host app | Locked-down fleets | Store review + trust friction |
| Enterprise / kiosk policy | Managed browser image | Kiosks only | Not for public SaaS users |
| Hardware vendor SDK | Vendor service or network printer API | Zebra / Epson / Star fleets | Hardware lock-in |
| Desktop shell (Electron…) | Your own desktop app | When you control the client | Not a pure browser app |

Most “silent print from Chrome” threads on the internet eventually land on **local print bridge** or **vendor SDK**.

## What happens on one print job

A typical HTML/PDF bridge job:

1. SPA builds HTML (or a PDF URL / bytes).
2. SDK opens `https://yoursite` → `ws://127.0.0.1:port` (or HTTP).
3. Agent accepts the job, optionally renders HTML in an embedded Chromium.
4. Agent submits to the OS spooler or opens a raw TCP/USB port.
5. SDK returns success / failure to the page.

A typical raw POS job skips step 3’s HTML render and sends ESC/POS or ZPL directly.

## Failure modes you will hit in production

| Symptom | Likely cause | Where to look |
|---|---|---|
| Works on laptop, fails after deploy | HTTP site blocked from loopback | [Chrome Local Network Access](chrome-local-network-access.md) |
| Connect OK, nothing prints | Wrong printer name / offline queue | Query printers; check OS spooler |
| Layout wrong vs screen | Different engine / missing fonts | Pin fonts; test on target OS |
| First job slow, later fine | Agent cold start / cert prompt | Prefight connect on login |
| Random drops on POS PCs | Sleep, antivirus, port conflict | Keep agent as a service; reserve port |

## Why localhost matters

Local bridges usually listen on `127.0.0.1`. Modern Chromium browsers also enforce **Local Network Access** rules: a public/production page often needs **HTTPS** before it can open `ws://127.0.0.1…`. Dev on `http://localhost` is a different security context, which is why “it worked on my machine” is so common.

## What silent print is not

- Generating a PDF for download (jsPDF, html2pdf…) — useful, but not printing
- Opening the system print dialog (Print.js, `window.print()`)
- Server-side PDF rendering alone (Puppeteer) without a path to a **local** printer
- “Silent” only inside Electron while still using `window.print()` in the browser build

## Security baseline

Treat the local agent like privileged software:

- Prefer authenticated / signed channels where the vendor supports them
- Do not expose the agent port to the LAN or internet
- Scope printers and templates; avoid “print any URL” from untrusted input
- Log job id → user/station → printer → result for audits

## Next

- [Choose a silent print stack](choose-silent-print-stack.md)
- [window.print vs silent print](window-print-vs-silent-print.md)
- [Chrome Local Network Access & 127.0.0.1](chrome-local-network-access.md)
- Curated tools: [Awesome Silent Printing](../README.md)
