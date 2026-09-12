# Remote / server-pushed silent print

## When local browser call is not enough

- Packing stations should not open the business SPA
- Jobs are created in a central WMS/OMS
- Multiple desks should consume the same queue
- Night shifts print from jobs created on another site’s UI

## Common architecture

```text
Business server
    → queue / webhook / websocket feed
    → desk print agent
    → local printer
```

The browser may only be used for configuration (bind station ↔ printers). The agent pulls or receives jobs continuously.

### Two transport styles

| Style | How it works | Watch for |
|---|---|---|
| Cloud relay (e.g. PrintNode-like) | Server API → vendor cloud → local client | Account security, per-site mapping |
| Self-hosted pull | Agent polls your API with a station token | Auth, backoff, durable queue |
| Transit / relay for designer stacks | Extra hop for hiprint-style clients | Operational complexity |

## Design notes

- **Authenticate the agent**; do not expose a raw print port to the internet
- Keep the job payload identical to your local SDK shape when possible (same HTML/PDF/raw)
- Handle offline desks with durable queues and dead-letter for poison jobs
- Log `job id → station → printer → result` for ops
- Version templates; a bad deploy should be rollbackable without reprinting history twice
- Rate-limit per station so one desk cannot starve others

## Security checklist

- [ ] Station credentials are rotatable
- [ ] TLS to your API
- [ ] Agent binds only to localhost for browser features; remote channel is outbound
- [ ] No “print arbitrary URL” from untrusted job fields
- [ ] Audit who can enqueue to which station

## Ops runbook (minimum)

1. Station offline → page on-call with last heartbeat time
2. Printer paper out → surface on station UI / LED if you have one
3. Poison template → quarantine job; alert template owners
4. Replay → only for explicitly failed ids

## Related

- [Batch & label printing](batch-label-printing.md)
- [Choose a stack](choose-silent-print-stack.md)
- Examples in the wild: PrintNode, hiprint transit relays, and local agents with remote job pull (see the [main list](../README.md#cloud--remote-print))
