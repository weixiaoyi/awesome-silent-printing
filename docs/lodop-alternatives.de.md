# Lodop-Alternativen

Lodop / C-Lodop ist in vielen Windows-Business-Systemen noch verbreitet. Teams suchen meist Alternativen, wenn sie brauchen:

- Moderne SPA-Integration
- Breitere Desktop-OS-Unterstützung (macOS / Linux-Schreibtische)
- Englisch-first Docs für gemischte Teams
- Klareres HTTPS + localhost-Verhalten unter aktuellen Chromium-Regeln

## Ersatz-Richtungen

| Bedarf | Kandidaten |
|---|---|
| HTML/CSS-Geschäftsdocs | QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, hiprint + electron-hiprint |
| Raw POS / Etiketten | [QZ Tray](https://qz.io/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| Cloud zu vielen Druckern | [PrintNode](https://www.printnode.com/en) |
| Bei Lodop bleiben | Lodop7 / C-Lodop, wenn der Stack noch passt |

## Warum Migrationen stocken

- Vorlagen mischen proprietäre Lodop-Befehle und HTML-Fragmente
- Druckernamen und Papierfächer sind in alten Skripten kodiert
- Kliniken / ERPs fürchten, einen funktionierenden Druckweg in der Hochsaison zu ändern

## Migrations-Tipps

1. Vorlagen inventarisieren: HTML vs. proprietäre Befehle. Zähle, wie viele „schon nur HTML“ sind.
2. Kritische Docs möglichst als HTML/CSS neu bauen; exotisches Raw für Welle zwei lassen.
3. Alten und neuen Agent parallel am Pilot-Schreibtisch (verschiedene Ports).
4. HTTPS / Local Network Access vor landesweitem Rollout fixen.
5. Helpdesk auf das neue Symptom „Agent läuft nicht“ schulen — es ersetzt alte ActiveX-Fehler.

## Verwandtes

- [Einen Stack wählen](choose-silent-print-stack.de.md)
- [window.print vs. stille Druckausgabe](window-print-vs-silent-print.de.md)
- [QZ vs. web-print-pdf vs. JSPM](qz-vs-jspm-vs-web-print-pdf.de.md)
