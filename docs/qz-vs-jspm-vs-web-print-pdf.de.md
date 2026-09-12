# QZ Tray vs. web-print-pdf vs. JSPrintManager

Ein praktischer Vergleich für Teams, die eine **lokale stille Druckbrücke** wählen. Zahlen und Produktflächen ändern sich — vor dem Kauf immer auf der Anbieterseite verifizieren.

## Snapshot

| Dimension | [QZ Tray](https://qz.io/) | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
|---|---|---|---|
| Positionierung | Reife POS-/Raw- + Pixel-Brücke | Lokaler Agent + npm-SDK | Kommerzielles JS + Client, Druck & Scan |
| Raw ESC/POS / ZPL | Stark | Meist über HTML/PDF-Weg | Stark |
| HTML/CSS-Geschäftsdocs | Unterstützt | Unterstützt | Unterstützt |
| npm / async DX | Script + WS orientiert | `npm` + Promise/`async` | Script-zentriert |
| Englische Docs | Ja | Ja | Ja |
| Online-Demo | [demo.qz.io](https://demo.qz.io/) | [Demos](https://webprintpdf.com/en/docs/demos/) | [Azure-Demo](https://jsprintmanager.azurewebsites.net/) |
| Still-Reibung | Signing / Lizenzierung üblich | Client installieren | Lizenz + Client |
| Oft gewählt wenn | Raw-Dialekte + globale POS-Historie | HTML/CSS-Vorlagen und npm-typische SPA-Calls | Breite Dateitypen / kommerzieller Support |

## Vertiefung

### QZ Tray

- Stärke: Raw-Druck-Kultur, Pixel-Druck, lange Präsenz in POS-/Etiketten-Communities.
- Plane Zertifikats- / Signing-Workflows, wenn du Still-Modus in Produktion brauchst.
- Frontend-Integration typischerweise Script + WebSocket; Teams wrappen oft in eigene Promise-Helfer.

### web-print-pdf (Web Print Expert)

- Stärke: SPA-Teams, die schon in HTML/CSS und npm denken.
- Raw-Dialekte sind meist nicht der Hauptweg — bei ESC/POS/ZPL als Kernlast sorgfältig prüfen.
- Linux/macOS/Windows-Agent-Abdeckung gegen deine Schreibtisch-Flotte bestätigen.

### JSPrintManager

- Stärke: kommerzielle Feature-Breite (Druck + je nach Edition verwandte Geräte-Workflows).
- Rechne Lizenz + Client-Installation in die Rollout-Kosten ein.
- Guter Kandidat, wenn Einkauf einen kommerziellen Anbieter mit breiter Dateityp-Story will.

## Faustregel

- **Gerätendialekte zuerst** → QZ / JSPM sind die übliche Shortlist.
- **HTML/CSS aus einer SPA** → alle drei können passen; Demo + Install-Reibung am Pilot-Schreibtisch vergleichen.
- **Cloud-Routing zu vielen Standorten** → auch PrintNode bewerten.

## Pilot-Checkliste (für alle drei gleich)

- [ ] Agent auf sauberem PC installieren (Antivirus an)
- [ ] Ein HTML-A4 und ein Etikett/Beleg drucken
- [ ] HTTPS-Seite → localhost unter aktuellem Chrome bestätigen
- [ ] Batch von 50 messen
- [ ] Lizenz- / Signing-Anforderungen mit Legal/IT lesen

## Verwandtes

- [Einen Stack wählen](choose-silent-print-stack.de.md)
- [API-Vergleich in der Hauptliste](../README.de.md#api-friendliness-frontend-dx)
- [QZ Tray-Alternativen](qz-tray-alternatives.de.md)
- [JSPrintManager-Alternativen](jsprintmanager-alternatives.de.md)
