# Thermobeleg stille Druckausgabe aus dem Browser

**Thermobeleg-Druck aus einer Webseite** braucht meist stille Druckausgabe: Kassierer und Küchenstationen können nicht bei jedem Bon durch einen Dialog klicken.

## Was funktioniert

1. **Lokale Druckbrücke** — HTML/Bild/PDF für einfache Belege oder ESC/POS-Raw für volle Gerätesteuerung
2. **Hersteller-SDK** für Epson / Star-ähnliche Netzwerkdrucker (Seite spricht Drucker oder Herstellerdienst an)
3. **Desktop-Shell** mit Silent Print, wenn du eine POS-Electron-App auslieferst

## Was nicht funktioniert

- `window.print()` als produktiver Still-Weg
- Reine PDF-Download-Bibliotheken ohne lokalen Druckagenten
- Annehmen, jede „80-mm-CSS-Seite“ schneidet und öffnet die Kassenlade ohne Raw-Befehle

## Architektur-Entscheidungen

| Bedarf | Tendenz |
|---|---|
| Logo + variables HTML-Layout, geringes Volumen | HTML über lokalen Agent |
| Hohes Küchenvolumen, Schneider, Lade, Buzzer | ESC/POS-Raw auf QZ / JSPM / Hersteller-SDK |
| Nur Epson/Star im LAN | Hersteller ePOS / webPRNT-ähnliches SDK |
| Du lieferst schon eine POS-Desktop-App | Electron Silent Print |

## Vorlagen-Tipps

- Feste Breite bevorzugen (z. B. **58 mm / 80 mm**)
- Barcodes kontrastreich; mit dem Scanner testen, den du wirklich nutzt
- Schwere CSS-Grids vermeiden; Thermo-Engines und Treiber sind gnadenlos
- Schneider- / Kassenladen-Befehle **nur** auf raw-fähigen Stacks testen
- Codepages für CJK / Akzente bei ESC/POS korrekt kodieren
- Nach Treiber- oder Agent-Update einen Kalibrierungsbon drucken

## Küche vs. Front-Theke

| | Front-Theke | Küche |
|---|---|---|
| Latenz-Toleranz | Niedrig | Sehr niedrig |
| Typischer Payload | HTML oder ESC/POS | Oft ESC/POS |
| Fehler-UX | Kassierer Retry zeigen | Auto-Retry + laute Warnung |
| Multi-Drucker | Beleg + Etikett | Routing nach Station / Artikel |

## Verwandtes

- [Batch- & Etikettendruck](batch-label-printing.de.md)
- [Stille Druckausgabe mit HTML/CSS](html-css-silent-print.de.md)
- [Einen Stack für stille Druckausgabe wählen](choose-silent-print-stack.de.md)
- Tool-Liste: [Awesome Silent Printing](../README.de.md)
