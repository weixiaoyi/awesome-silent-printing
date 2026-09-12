# Batch- & Etikettendruck aus dem Web

## Problem

Lager und E-Commerce-Schreibtische brauchen oft **Dutzende oder Hunderte** Etiketten, ohne dass jemand durch Druckdialoge klickt. Ein Dialog pro Etikett ist kein Operations-Design — es ist ein Ausfall.

## Was du brauchst

1. Einen stillen Weg zu einem **benannten** Drucker
2. Eine Job-Queue / Batch-API (Client-seitige Schleife reicht in Scale nicht)
3. Stabiles Template-Rendering (HTML oder ZPL)
4. Retry + Logging, wenn ein Job mitten im Batch scheitert
5. Back-pressure, wenn der Drucker langsamer ist als der Submitter

## Muster

| Muster | Notizen | Am besten wenn |
|---|---|---|
| Browser → lokale Agent-Batch-API | Niedrigste Latenz am Schreibtisch-PC | Operator arbeitet in der SPA |
| Server-Push → Desk-Agent-Pull | Besser für viele Packstationen | Browser muss nicht offen bleiben |
| Hersteller-Raw (ZPL) | Exzellent für reine Etikettendrucker | Flotte bereits auf ZPL standardisiert |

## Empfohlener Batch-Flow

```text
Aufträge wählen
  → N Vorlagen rendern oder laden
  → als Batch senden (oder Chunks à 20–50)
  → Status pro Job (queued / printing / done / failed)
  → nur fehlgeschlagene IDs erneut versuchen
```

### Chunking

500 Jobs in einem `Promise.all` schadet oft mehr als es hilft. Bevorzuge Chunks:

- 20–50 Jobs pro Chunk für HTML-Rendering-Agenten
- Größere Chunks können für winzige ZPL-Strings ok sein
- Chunk-Abschluss abwarten (oder Concurrency-Limit 2–3) vor dem nächsten

## Fehler-Taxonomie

| Fehler | Operator-Aktion | System-Aktion |
|---|---|---|
| Agent offline | Agent installieren / starten | Queue pausieren; Banner |
| Drucker offline / kein Papier | Hardware fixen | Jobs als retryable markieren |
| Schlechte Vorlage / Barcode | Daten fixen | Job failen; andere fortsetzen |
| LNA / HTTPS | IT fixt Origin | Siehe [Chrome LNA](chrome-local-network-access.de.md) |

## Checkliste

- [ ] Druckerwahl pro Papiergröße / Station
- [ ] Batch-Submit + Status pro Job
- [ ] HTTPS-Seite erreicht localhost (LNA)
- [ ] Template-Regressionstests für Barcode/QR
- [ ] Idempotente Job-IDs (Resubmit sicher)
- [ ] Ops kann fehlgeschlagene Job-IDs exportieren

## Verwandtes

- [Remote stille Druckausgabe](remote-silent-print.de.md)
- [Stille Druckausgabe mit HTML/CSS](html-css-silent-print.de.md)
- [Thermobeleg stille Druckausgabe](thermal-receipt-silent-print.de.md)
- [Einen Stack wählen](choose-silent-print-stack.de.md)
