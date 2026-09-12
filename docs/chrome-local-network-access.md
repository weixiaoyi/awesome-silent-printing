# Chrome Local Network Access & 127.0.0.1

## Symptom

Dev works. Production fails with something like:

```text
WebSocket connection to 'ws://127.0.0.1:…' failed
Failed to connect to print agent
ERR_CONNECTION_REFUSED / net::ERR_FAILED
```

The local print client is running. Firewall looks fine. Only the **deployed website** cannot reach loopback.

This is one of the most common “silent print broke after go-live” incidents for every localhost bridge.

## Cause

Chromium’s **Local Network Access (LNA)** restricts public pages from talking to the user’s local network / loopback. Print agents that listen on `127.0.0.1` sit exactly in that restricted zone.

Practical rules teams hit in the field:

| Context | Typical result |
|---|---|
| App on `http://localhost` in dev | Often works (special context) |
| App on plain **HTTP** in production | Often **silently denied** |
| App on **HTTPS** with a trusted cert | Can prompt for Local Network permission |
| Newer Chrome + WebSocket to loopback | Same LNA rules apply to `ws://127.0.0.1…` |
| Edge / other Chromium | Similar policies |

So the agent being “up” is necessary but not sufficient. The **browser origin** must be allowed to talk to loopback.

## Decision flowchart

```text
Can the page reach ws://127.0.0.1 / http://127.0.0.1?
│
├─ No, and site is HTTP
│     → Put the site on HTTPS first. Stop here until that ships.
│
├─ No, and site is HTTPS
│     → Check Local Network permission / prompt
│     → Confirm agent port + process
│     → Test from the same machine with a tiny WS client
│
└─ Yes, but print still fails
      → Printer name, driver, spooler, template — not LNA
```

## What to do (checklist)

1. **Serve the web app over HTTPS** with a certificate the workstation trusts (public CA or your internal PKI). Self-signed certs that users have not trusted will keep failing.
2. On first print, watch for the browser’s **Local Network** permission prompt and allow it for your origin.
3. Confirm the desktop print agent is listening on `127.0.0.1` (and the port your SDK expects).
4. Reproduce with DevTools → Network: is the WS/HTTP call blocked, refused, or reset?
5. For **managed fleets only**: browser flags / enterprise policy can relax checks. That is not a strategy for public SaaS end users.
6. Document the permission step in your install runbook; helpdesks will otherwise reinstall the agent forever.

## How to tell LNA apart from “agent down”

| Check | Agent down | LNA / origin issue |
|---|---|---|
| `127.0.0.1:port` from a local tool | Fails | Succeeds |
| Same machine, site on HTTP | May fail | Often fails |
| Same machine, site on HTTPS + permission | Works if agent up | Works |
| Different user profile in Chrome | Same | Permission may be missing |

## Who is affected

Any localhost print bridge: QZ Tray, web-print-pdf, JSPrintManager, Lodop-style local services, custom agents. This is not a single-vendor bug.

## Deeper walkthrough

- English: [WebSocket to 127.0.0.1 failed after deploy](https://webprintpdf.com/en/docs/production-print-troubleshoot/)
- 中文: [上线后连接 127.0.0.1 失败](https://webprintpdf.com/docs/production-print-troubleshoot/)

## Related

- [How silent printing works](how-silent-printing-works.md)
- [Vue / React silent print](vue-react-silent-print.md)
- [Choose a stack](choose-silent-print-stack.md)
