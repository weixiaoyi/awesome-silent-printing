# Stille Druckausgabe mit HTML/CSS

## Warum Teams das wollen

Die meisten Business-UIs sind schon HTML/CSS. Wenn stille Druckausgabe dieselben Vorlagen nutzen kann, bleibt Frontend-Tempo hoch – und du musst nicht für jede Rechnung einen zweiten ZPL/ESC/POS-Skill pflegen.

## Zwei Layout-Modelle

| Modell | Vorteile | Nachteile |
|---|---|---|
| HTML/CSS über lokalen Chromium/Agent | Web-Teams kennen es; gute Treue | Lokaler Agent nötig |
| Raw ESC/POS / ZPL | Präzise Gerätesteuerung | Anderer Skill; hardwarespezifisch |

Viele Produkte mischen beides: A4-Docs in HTML, Thermobeleg in Raw.

## Was „Print CSS“ hier bedeutet

Browser-`@media print` allein ist **kein** stiller Weg — es geht trotzdem durch `window.print()`. Mit lokalem Agent machst du typischerweise:

1. Self-contained HTML-String (oder URL) bauen.
2. CSS einbinden, das der Agent braucht (inline, absolute URL oder gebündelt).
3. Papiergröße / Ränder / Druckername in SDK-Optionen übergeben.
4. Engine des Agents (oft Chromium) paginieren und spoolen lassen.

### Vorlagen-Checkliste

- [ ] Explizite Seitengröße (A4, 100×150 mm, 80-mm-Rolle …)
- [ ] Ränder passend zum physischen Material
- [ ] Schriften am Arbeitsplatz installiert oder eingebettet
- [ ] Barcode/QR als SVG oder hochauflösendes Bild (Scan testen)
- [ ] Tabellen schneiden die letzte Zeile nicht ab
- [ ] Keine Abhängigkeit von rein viewport-basierten Layouts (`100vh`-Fallen)

## Praktische Tipps

- Entwirf ein **nur-für-Druck**-Stylesheet; kopiere nicht blind das volle App-Chrome-CSS.
- Bevorzuge `mm` / `in` für Etiketten; `px` allein driftet über DPI.
- Teste chinesische / CJK-Schriften unter Windows **und** Linux, wenn du beides unterstützt.
- Vor Batch-Migration eine Job-Vorschau drucken.
- Bilder klein halten; riesige PNGs killen Batch-Durchsatz.
- Für exakte Thermoschneider / Kassenladen brauchst du evtl. trotzdem Raw-Befehle auf einer raw-fähigen Brücke.

## Gute Passung

Rechnungen, Auszüge, Packlisten, A4-Reports, viele Etikett-Layouts als HTML.

## Schlechte Passung (Raw / Hersteller-SDK erwägen)

- Ultra-schnelle Küchendrucker, die nur ESC/POS erwarten
- Zebra-Flotten mit ZPL-Vorlagen in der Drucker-Firmware
- Geräte ohne brauchbaren Windows/macOS/Linux-Treiberweg außer Hersteller-Raw

## Verwandtes

- [Stille Druckausgabe in Vue / React](vue-react-silent-print.de.md)
- [Batch- & Etikettendruck](batch-label-printing.de.md)
- [Thermobeleg stille Druckausgabe](thermal-receipt-silent-print.de.md)
- [Einen Stack wählen](choose-silent-print-stack.de.md)
