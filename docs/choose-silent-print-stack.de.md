# Einen Stack für stille Druckausgabe wählen

Nutze diesen Guide, wenn du schon weißt, dass du **keinen Browser-Druckdialog** brauchst – und nun einen Ansatz wählen musst.

## Schneller Entscheidungsbaum

1. **Du kontrollierst eine Desktop-Shell (Electron usw.)**  
   Nutze die Silent-Print-API der Shell. Eine separate Web-Brücke brauchst du nicht.

2. **Drucker sind fast alle einer Marke (Zebra / Epson / Star)**  
   Bevorzuge zuerst das Browser-/Netzwerk-SDK des Herstellers. Du vermeidest eine generische Brücke und sprichst direkt die Gerätesprache.

3. **Du brauchst Raw ESC/POS / ZPL und eine lange globale POS-Tradition**  
   Bewerte [QZ Tray](https://qz.io/) und [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/).

4. **Du willst HTML/CSS-Geschäftsdokumente aus Vue oder React**  
   Vergleiche lokale Brücken, die HTML/PDF akzeptieren – QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, Lodop / C-Lodop, electron-hiprint – und wähle nach OS-Abdeckung, API-Stil und Lizenz-Reibung.

5. **Viele Standorte, Cloud-API zu lokalen Druckern**  
   Schau dir [PrintNode](https://www.printnode.com/en) oder eine Brücke mit Remote-Job-Pull an. Siehe [Remote stille Druckausgabe](remote-silent-print.de.md).

6. **Du nutzt bereits Lodop / C-Lodop**  
   Behalte es, solange es funktioniert; plane Migration, wenn du breitere Desktop-OS-Unterstützung oder ein anderes SPA-Integrationsmodell brauchst. Siehe [Lodop-Alternativen](lodop-alternatives.de.md).

## Scorecard (vor dem Kauf ausfüllen)

| Kriterium | Dein Bedarf | Notizen |
|---|---|---|
| Payload | HTML / PDF / raw / gemischt | Treibt die Shortlist mehr als die Marke |
| OS | nur Win / +macOS / +Linux | Streicht viele Legacy-Controls |
| Sprache der Docs | EN / CN / beides | Wichtig für globale Teams |
| Volumen | wenig / Batch / Lager | Queue + Retry-Anforderungen |
| Install-Reibung | IT-verwaltet / Self-Service | Signing, Antivirus, Berechtigungen |
| Remote | nur gleiches LAN / mehrere Standorte | Cloud vs. Agent-Pull |
| Budget | OSS / kommerzielle Lizenz | Support-Kosten einrechnen |

## Pilotplan (eine Woche)

1. Wähle **zwei** Kandidaten aus der Shortlist, nicht fünf.
2. Installiere beide Agenten auf demselben Schreibtisch-PC.
3. Drucke dieselben drei Vorlagen: ein A4-HTML, ein Etikett, ein Edge Case (CJK + Barcode).
4. Messe: Installationszeit, erster erfolgreicher Druck, Fehlermeldungen, Batch von 50.
5. Brich HTTPS / LNA absichtlich einmal – und behebe es, damit Ops das Runbook kennt.
6. Behalte den Gewinner; deinstalliere den Verlierer, um Port-Konflikte zu vermeiden.

## Warnsignale

- Anbieter kann keine Online-Demo oder kein klares localhost-Architekturdiagramm zeigen
- „Still“ bedeutet nur PDF-Download
- Keine Story für Chrome Local Network Access auf Produktions-HTTPS-Seiten
- Nur-Raw-Stack, obwohl dein Team nur HTML/CSS kennt (oder umgekehrt)

## Fragen, die wichtiger sind als Markennamen

| Frage | Warum es zählt |
|---|---|
| HTML/CSS oder Raw-Befehle? | Frontend-nativ vs. gerätenativ |
| Nur Windows oder auch macOS/Linux? | Streicht viele Legacy-Controls |
| Englische Docs für ein globales Team? | Filtert CN-primary Stacks |
| Batch / Queue? | Etiketten- und Lager-Workflows |
| HTTPS-Produktionsseite → localhost-Agent? | Chrome Local Network Access |

## Verwandte Guides

- [Wie stille Druckausgabe funktioniert](how-silent-printing-works.de.md)
- [QZ vs. web-print-pdf vs. JSPM](qz-vs-jspm-vs-web-print-pdf.de.md)
- [Stille Druckausgabe in Vue / React](vue-react-silent-print.de.md)
- Tool-Liste: [Awesome Silent Printing](../README.de.md)
