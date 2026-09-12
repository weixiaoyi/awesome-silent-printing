# Stille Druckausgabe in Vue / React

## Ziel

Stille Druckausgabe aus einer SPA aufrufen, ohne `window.print()` zu öffnen.

## Typische Integration

1. Nutzer installiert einmal einen lokalen Druckagenten.
2. Frontend hängt von einem npm-SDK oder Vendor-JS ab (QZ, web-print-pdf, JSPM usw.).
3. Seite sendet HTML, PDF oder Raw-Payload an `127.0.0.1`.
4. Agent rendert / leitet an den OS-Drucker weiter.

Form des Aufrufs (APIs unterscheiden sich je nach Anbieter):

```js
// Pseudocode — siehe SDK-Docs deiner Brücke
await printAgent.printHtml(
  '<div class="label">Order #1001</div>',
  { printer: 'LabelPrinter' }
);
```

### Empfohlene Modulgrenze

Halte Druck hinter einem kleinen Service, damit Vue/React-Komponenten „dumm“ bleiben:

```js
// printService.js
export async function printLabel(html, printer) {
  await ensureAgent();
  return printAgent.printHtml(html, { printer });
}
```

- Rufe `ensureAgent()` beim App-Start oder vor dem ersten Druckbildschirm auf.
- Zeige Installationshinweise, wenn der Agent fehlt.
- Rufe Druck nicht aus zehn Komponenten mit zehn verschiedenen Options-Objekten auf.

## SPA-Fallen

| Falle | Fix |
|---|---|
| Druck vor Agent-Start | Verbindung vorab testen / Install-Hinweis |
| Hardcodierte Druckernamen | Druckerliste abfragen; pro Station speichern |
| HTTP-Produktions-Origin | Auf HTTPS für Local Network Access |
| Styling anders als am Bildschirm | Chromium-basierte HTML→Print-Agenten; Schriften fixieren |
| Live Vue/React-Baum mit UI-Chrome drucken | Dedizierte Print-Root / Offscreen-Vorlage |
| Riesige Base64-Bilder inline | URLs, die der Agent laden kann, oder komprimieren |
| Promise-Rejections ignorieren | Fehler auf Toast + Retry mappen; Job-IDs loggen |

## Vue-Hinweise

- Vorlagen in einer dedizierten SFC nur für Druck (`LabelTicket.vue`), nicht im vollen Seitenlayout.
- Bevorzuge `ref` + `innerHTML` / `outerHTML` einer gemounteten Print-Root oder baue HTML-Strings aus Daten.
- Vermeide Druck während `<Transition>` oder Virtual List mitten im Update.

## React-Hinweise

- Gleiche Idee: `PrintTicket`-Komponente in verstecktem Container, dann HTML serialisieren.
- Vorsicht mit Portalen und Concurrent Rendering — Snapshot, wenn Daten stabil sind.
- Verlasse dich nicht auf `window.print()` in `useEffect` als „temporären“ Still-Weg; das trainiert die falsche Gewohnheit.

## Verbindungs-Lebenszyklus

```text
App-Start
  → Agent pingen
  → wenn down: Banner + Install-Link
  → wenn up: Druckerliste cachen
Nutzer klickt Drucken
  → erneut pingen (günstig)
  → Job senden
  → Job-Ergebnis / Fehlercode anzeigen
```

## Verwandtes

- [Stille Druckausgabe mit HTML/CSS](html-css-silent-print.de.md)
- [Chrome Local Network Access](chrome-local-network-access.de.md)
- [Einen Stack wählen](choose-silent-print-stack.de.md)
- [QZ vs. web-print-pdf vs. JSPM](qz-vs-jspm-vs-web-print-pdf.de.md)
