# Silent print in Vue / React

## Goal

Call silent print from a SPA without opening `window.print()`.

## Typical integration

1. User installs a local print agent once.
2. Frontend depends on an npm SDK or vendor JS (QZ, web-print-pdf, JSPM, etc.).
3. Page sends HTML, PDF, or raw payload to `127.0.0.1`.
4. Agent renders / forwards to the OS printer.

Shape of the call (APIs differ by vendor):

```js
// Pseudocode — check your bridge’s SDK docs
await printAgent.printHtml(
  '<div class="label">Order #1001</div>',
  { printer: 'LabelPrinter' }
);
```

### Suggested module boundary

Keep print behind a small service so Vue/React components stay dumb:

```js
// printService.js
export async function printLabel(html, printer) {
  await ensureAgent();
  return printAgent.printHtml(html, { printer });
}
```

- Call `ensureAgent()` on app boot or before the first print screen.
- Surface install instructions when the agent is missing.
- Never call print directly from ten different components with ten different options objects.

## SPA pitfalls

| Pitfall | Fix |
|---|---|
| Calling print before the agent is up | Prefight connection / show install guidance |
| Hard-coded printer names | Query printer list; persist per station |
| HTTP production origin | Move to HTTPS for Local Network Access |
| Styling differs from screen | Prefer Chromium-based HTML→print agents; pin fonts |
| Printing a live Vue/React tree full of UI chrome | Render a dedicated print root / offscreen template |
| Huge base64 images inline | Prefer URLs the agent can fetch, or compress |
| Ignoring Promise rejections | Map errors to toast + retry; log job ids |

## Vue notes

- Put templates in a dedicated SFC used only for print (`LabelTicket.vue`), not your full page layout.
- Prefer `ref` + `innerHTML` / `outerHTML` of a mounted print root, or build HTML strings from data.
- Avoid printing while a `<Transition>` or virtual list is mid-update.

## React notes

- Same idea: a `PrintTicket` component rendered to a hidden container, then serialize HTML.
- Be careful with portals and concurrent rendering — snapshot when data is stable.
- Do not rely on `window.print()` inside `useEffect` as a “temporary” silent path; it trains the wrong habit.

## Connection lifecycle

```text
App start
  → ping agent
  → if down: banner + install link
  → if up: cache printer list
User clicks Print
  → re-ping (cheap)
  → submit job
  → show job result / error code
```

## Related

- [HTML/CSS silent print](html-css-silent-print.md)
- [Chrome Local Network Access](chrome-local-network-access.md)
- [Choose a stack](choose-silent-print-stack.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.md)
