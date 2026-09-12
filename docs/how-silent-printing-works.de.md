# Wie stille Druckausgabe funktioniert

Browser sind so gebaut, dass Webseiten **nicht** unbemerkt Druckaufträge an physische Drucker senden können. Deshalb zeigt `window.print()` einen Dialog – und „stille Druckausgabe mit purem JavaScript“ ist meist eine Sackgasse.

Wenn du Belege, Versandetiketten, Rechnungen oder Küchenbons aus einer Web-App druckst, brauchst du eine andere Architektur – kein Browser-Flag.

## Das eigentliche Muster

Fast jedes produktive Setup für stille Druckausgabe sieht so aus:

```text
Web-App (Browser)
    → localhost HTTP / WebSocket / Native Messaging
    → lokaler Agent / Herstellerdienst / Desktop-Shell
    → OS-Druckspooler oder Raw-Geräteport
```

Die Seite „besitzt“ den Drucker nie. Eine **vertrauenswürdige lokale Komponente** tut das. Sie wird einmal pro Arbeitsplatz installiert (oder in ein Kiosk-Image eingebaut), danach spricht die Web-App sie als lokalen Dienst an.

### Warum das so ist

| Anliegen | Browser-Antwort | Antwort bei stiller Druckausgabe |
|---|---|---|
| Böswillige Seiten drucken Spam | Stiller Gerätezugriff blockiert | Nutzer installiert einen bekannten Agenten |
| Falscher Drucker gewählt | Dialog erzwingen | Agent + benannte Drucker-API |
| Raw ESC/POS / ZPL | Von der Seite nicht verfügbar | Agent oder Hersteller-SDK sendet Bytes |
| Batch / unbeaufsichtigte Jobs | Dialog bricht den Ablauf | Warteschlange am Agent oder Server |

## Fünf Architekturen

| Ansatz | Wer installiert was | Typischer Einsatz | Kompromiss |
|---|---|---|---|
| Lokale Druckbrücke | Desktop-Agent + JS-SDK | ERP, WMS, POS, Etiketten | Bester allgemeiner Web-Weg für stille Druckausgabe |
| Extension + Native Host | Browser-Extension + Host-App | Festgelegte Geräteflotten | Store-Review + Vertrauensreibung |
| Enterprise / Kiosk-Richtlinie | Verwaltetes Browser-Image | Nur Kioske | Nicht für öffentliche SaaS-Nutzer |
| Hardware-Hersteller-SDK | Herstellerdienst oder Netzwerkdrucker-API | Zebra / Epson / Star Flotten | Hardware-Lock-in |
| Desktop-Shell (Electron …) | Eigene Desktop-App | Wenn du den Client kontrollierst | Keine reine Browser-App |

Die meisten „stille Druckausgabe aus Chrome“-Threads im Netz landen am Ende bei **lokaler Druckbrücke** oder **Hersteller-SDK**.

## Was bei einem Druckauftrag passiert

Ein typischer HTML/PDF-Brücken-Job:

1. SPA baut HTML (oder eine PDF-URL / Bytes).
2. SDK öffnet `https://deineseite` → `ws://127.0.0.1:port` (oder HTTP).
3. Agent nimmt den Job an, rendert HTML optional in eingebettetem Chromium.
4. Agent übergibt an den OS-Spooler oder öffnet einen Raw-TCP/USB-Port.
5. SDK meldet Erfolg / Fehler an die Seite zurück.

Ein typischer Raw-POS-Job überspringt Schritt 3s HTML-Render und sendet ESC/POS oder ZPL direkt.

## Fehlermuster in der Produktion

| Symptom | Wahrscheinliche Ursache | Wo nachschauen |
|---|---|---|
| Am Laptop ok, nach Deploy Fehler | HTTP-Seite blockiert von Loopback | [Chrome Local Network Access](chrome-local-network-access.de.md) |
| Verbindung ok, nichts druckt | Falscher Druckername / offline Queue | Drucker abfragen; OS-Spooler prüfen |
| Layout anders als am Bildschirm | Andere Engine / fehlende Schriften | Schriften fixieren; auf Ziel-OS testen |
| Erster Job langsam, danach ok | Agent Cold Start / Zertifikatsabfrage | Verbindung beim Login vorab testen |
| Zufällige Abbrüche an POS-PCs | Sleep, Antivirus, Port-Konflikt | Agent als Dienst; Port reservieren |

## Warum localhost wichtig ist

Lokale Brücken lauschen meist auf `127.0.0.1`. Moderne Chromium-Browser setzen auch **Local Network Access**-Regeln durch: Eine öffentliche/Produktionsseite braucht oft **HTTPS**, bevor sie `ws://127.0.0.1…` öffnen darf. Dev unter `http://localhost` ist ein anderer Sicherheitskontext – deshalb ist „bei mir ging es“ so verbreitet.

## Was stille Druckausgabe nicht ist

- PDF zum Download erzeugen (jsPDF, html2pdf …) – nützlich, aber kein Drucken
- System-Druckdialog öffnen (Print.js, `window.print()`)
- Serverseitiges PDF-Rendering allein (Puppeteer) ohne Weg zum **lokalen** Drucker
- „Still“ nur in Electron, während der Browser-Build weiter `window.print()` nutzt

## Sicherheits-Baseline

Behandle den lokalen Agenten wie privilegierte Software:

- Bevorzuge authentifizierte / signierte Kanäle, wo der Anbieter sie unterstützt
- Exponiere den Agent-Port nicht im LAN oder Internet
- Begrenze Drucker und Vorlagen; vermeide „beliebige URL drucken“ aus untrusted Input
- Protokolliere Job-ID → Nutzer/Station → Drucker → Ergebnis für Audits

## Weiter

- [Einen Stack für stille Druckausgabe wählen](choose-silent-print-stack.de.md)
- [window.print vs. stille Druckausgabe](window-print-vs-silent-print.de.md)
- [Chrome Local Network Access & 127.0.0.1](chrome-local-network-access.de.md)
- Kuratierte Tools: [Awesome Silent Printing](../README.de.md)
