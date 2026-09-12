# Batch & label printing from the web

## Problem

Warehouses and ecommerce desks often need **dozens or hundreds** of labels without a human clicking through print dialogs. A single dialog per label is not an operations design — it is an outage.

## What you need

1. A silent path to a **named** printer
2. A job queue / batch API (client-side loop is not enough at scale)
3. Stable template rendering (HTML or ZPL)
4. Retry + logging when a job fails mid-batch
5. Back-pressure when the printer is slower than the submitter

## Patterns

| Pattern | Notes | Best when |
|---|---|---|
| Browser → local agent batch API | Lowest latency on the desk PC | Operator works inside the SPA |
| Server push → desk agent pull | Better for many packing stations | Browser should not stay open |
| Vendor raw (ZPL) | Excellent for pure label printers | Fleet already standardized on ZPL |

## Recommended batch flow

```text
Select orders
  → render or fetch N templates
  → submit as a batch (or chunked batches of 20–50)
  → show per-job status (queued / printing / done / failed)
  → retry failed ids only
```

### Chunking

Submitting 500 jobs in one Promise.all often hurts more than it helps. Prefer chunks:

- 20–50 jobs per chunk for HTML rendering agents
- Larger chunks may be fine for tiny ZPL strings
- Await chunk completion (or concurrency limit 2–3) before the next

## Failure taxonomy

| Failure | Operator action | System action |
|---|---|---|
| Agent offline | Install / start agent | Pause queue; banner |
| Printer offline / paper out | Fix hardware | Mark jobs retryable |
| Bad template / barcode | Fix data | Fail that job; continue others |
| LNA / HTTPS | IT fix origin | See [Chrome LNA](chrome-local-network-access.md) |

## Checklist

- [ ] Printer selection per paper size / station
- [ ] Batch submit + per-job status
- [ ] HTTPS page can reach localhost (LNA)
- [ ] Template regression tests for barcode/QR
- [ ] Idempotent job ids (resubmit safe)
- [ ] Ops can export failed job ids

## Related

- [Remote silent print](remote-silent-print.md)
- [HTML/CSS silent print](html-css-silent-print.md)
- [Thermal receipt silent print](thermal-receipt-silent-print.md)
- [Choose a stack](choose-silent-print-stack.md)
