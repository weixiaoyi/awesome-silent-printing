# window.print vs. stille Druckausgabe

Oft wird gefragt: Kann ich `window.print()` ohne Dialog aufrufen?  
Kurzantwort: **Nicht in einer normalen Browser-Webseite.**

Browser behandeln Drucken als privilegierte Nutzeraktion. Die Druck-UI ist das Sicherheitsventil. Bibliotheken, die `window.print()` wrappen (z. B. Print.js oder react-to-print), landen trotzdem in dieser UI.

## Nebeneinander

| | `window.print()` / Print.js | Stille Druckbrücke |
|---|---|---|
| Dialog | Ja | Nein |
| Drucker aus JS wählen | begrenzt / nein | Ja (über lokalen Agent) |
| Batch-Jobs | schwach | für Queues gedacht |
| Benannter Drucker / Schacht / Papier | nutzergetrieben | Agent-API |
| Etwas installieren | Nein | meist ja |
| Sicherheitsmodell | browsergesteuert | lokale privilegierte Software |
| Auf unverwalteten SaaS-Clients | Ja (mit Dialog) | Agent-Installation nötig |

## Mythen, die Sprints kosten

| Mythos | Realität |
|---|---|
| „Es muss ein Chrome-Flag für SaaS-Nutzer geben“ | Flags/Richtlinien gelten für verwaltete Geräte, nicht für öffentliche Besucher |
| „Puppeteer auf dem Server ist stille Druckausgabe“ | Es erzeugt PDFs/Bilder; es steuert nicht den USB-/Netzwerkdrucker des Nutzers |
| „PDF-Download reicht fast“ | Nutzer drucken manuell; kein Batch / benannter Drucker |
| „Electron-Silent-APIs funktionieren im Browser-Build“ | Diese APIs existieren nur in der Desktop-Shell, die du auslieferst |

## Wann `window.print()` reicht

- Gelegentliches nutzergetriebenes Drucken (Auszüge, die der Nutzer bestätigen soll)
- Rechtliche / medizinische Abläufe, wo eine explizite Bestätigung sinnvoll ist
- Geringes Volumen, keine Kiosk- / Lager-Automatisierung
- Du willst keine lokale Installation ausrollen

## Wann du stille Druckausgabe brauchst

- Versandetiketten, Belege, Küchenbons
- Unbeaufsichtigte oder hochfrequente Jobs
- SPA-Workflows, wo ein Dialog die UX bricht (scannen → drucken → weiter)
- Schreibtische mit mehreren Druckern (Etikett vs. A4 vs. Beleg), in Software gewählt

## Migrationspfad

1. **Inventarisiere**, was du heute druckst: HTML-Screenshots, CSS `@media print`, PDF-Dateien oder Raw.
2. **Behalte HTML/CSS-Vorlagen**, wenn die Zielbrücke sie rendern kann; schreibe nur um, was ZPL/ESC/POS werden muss.
3. **Wähle einen lokalen Agenten** (oder Hersteller-SDK / Electron) mit [Einen Stack für stille Druckausgabe wählen](choose-silent-print-stack.de.md).
4. Ersetze `window.print()`-Aufrufe durch SDK-Calls (HTML/PDF/raw über localhost).
5. Füge **Agent-nicht-installiert**-UX hinzu: Verbindung erkennen, Install-Link zeigen, „Drucken“-Button blockieren bis bereit.
6. Deploye die Seite über **HTTPS** und prüfe [Local Network Access zu `127.0.0.1`](chrome-local-network-access.de.md) auf einem sauberen Arbeitsplatz.
7. Pilotiere einen Schreibtisch eine Woche mit Logging, bevor du ausrollst.

### Minimale Code-Form-Änderung

Vorher:

```js
window.print();
```

Nachher (Pseudocode – APIs unterscheiden sich je nach Anbieter):

```js
await printAgent.printHtml(document.getElementById('label').outerHTML, {
  printer: selectedPrinter,
});
```

## Abnahmetests, bevor du „fertig“ sagst

- [ ] Dialog erscheint im Happy Path nie
- [ ] Richtiger Drucker wird ohne Nutzerklicks gewählt
- [ ] Ein fehlerhafter Job friert nicht den ganzen Batch ein
- [ ] Frisches Browser-Profil kommt einmal durch HTTPS + LNA-Berechtigung
- [ ] Agent offline zeigt recoverbaren Fehler, kein Hängen

## Verwandtes

- [Wie stille Druckausgabe funktioniert](how-silent-printing-works.de.md)
- [Einen Stack wählen](choose-silent-print-stack.de.md)
- [Chrome Local Network Access](chrome-local-network-access.de.md)
- [Stille Druckausgabe in Vue / React](vue-react-silent-print.de.md)
