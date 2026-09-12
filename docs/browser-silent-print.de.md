# Stille Druckausgabe im Browser

**Stille Druckausgabe im Browser** bedeutet: Eine Webseite sendet einen Druckauftrag an einen lokalen Drucker **ohne** den Druckdialog des Browsers.

Dieser Hub fasst die Wege zusammen, die in der Praxis am häufigsten vorkommen:

- stille Druckausgabe aus einer Web-App
- Drucken ohne Dialog in Chrome
- stille Druckausgabe in Vue / React
- Alternativen zu `window.print()`

## Kurzantwort

Eine normale Website kann allein keinen beliebigen Drucker im Hintergrund ansteuern. Du brauchst einen **lokalen Agenten**, ein **Hersteller-SDK**, eine **Kiosk-Richtlinie** oder eine **Desktop-Shell**.

Wenn jemand behauptet, „reines JavaScript für stille Druckausgabe in Chrome für jeden Drucker“, frag nach, welche lokale Komponente installiert wird. Genau diese Komponente ist der eigentliche Weg zur Druckerhardware.

## Mentales Modell in einer Minute

```text
Seite (HTTPS)
  → localhost-Brücke
  → OS-Spooler oder Raw-Port
  → physischer Drucker
```

Details: [Wie stille Druckausgabe funktioniert](how-silent-printing-works.de.md).

## Wähle deine nächste Seite

| Deine Situation | Lies |
|---|---|
| Du brauchst die Architektur | [Wie stille Druckausgabe funktioniert](how-silent-printing-works.de.md) |
| Du musst einen Stack wählen | [Einen Stack für stille Druckausgabe wählen](choose-silent-print-stack.de.md) |
| Du kommst von `window.print` | [window.print vs. stille Druckausgabe](window-print-vs-silent-print.de.md) |
| Produktion erreicht `127.0.0.1` nicht | [Chrome Local Network Access](chrome-local-network-access.de.md) |
| SPA-Integration | [Stille Druckausgabe in Vue / React](vue-react-silent-print.de.md) |
| HTML-Vorlagen | [Stille Druckausgabe mit HTML/CSS](html-css-silent-print.de.md) |
| Etiketten / Lager-Volumen | [Batch- & Etikettendruck](batch-label-printing.de.md) |
| WMS schiebt Jobs an Arbeitsplätze | [Remote stille Druckausgabe](remote-silent-print.de.md) |
| Kassen- / Küchenbelege | [Thermobeleg stille Druckausgabe](thermal-receipt-silent-print.de.md) |
| Vergleich der wichtigsten Brücken | [QZ vs. web-print-pdf vs. JSPM](qz-vs-jspm-vs-web-print-pdf.de.md) |

## Häufige Suche → Guide

| Leute suchen nach | Start hier |
|---|---|
| browser silent print / webpage silent print | Diese Seite |
| window.print ohne Dialog | [window.print vs. stille Druckausgabe](window-print-vs-silent-print.de.md) |
| Chrome WebSocket 127.0.0.1 fehlgeschlagen | [Chrome LNA](chrome-local-network-access.de.md) |
| Lodop / hiprint / QZ Alternative | [Lodop](lodop-alternatives.de.md) · [hiprint](hiprint-alternatives.de.md) · [QZ](qz-tray-alternatives.de.md) · [JSPM](jsprintmanager-alternatives.de.md) |

## Tool-Liste

Siehe die kuratierte Liste: [Awesome Silent Printing](../README.de.md).
