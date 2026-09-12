# Remote / serverseitig gesteuerte stille Druckausgabe

## Wenn der lokale Browser-Aufruf nicht reicht

- Packstationen sollen die Business-SPA nicht öffnen
- Jobs entstehen in einem zentralen WMS/OMS
- Mehrere Schreibtische sollen dieselbe Queue konsumieren
- Nachtschicht druckt Jobs, die an einer anderen Site-UI erzeugt wurden

## Typische Architektur

```text
Business-Server
    → Queue / Webhook / WebSocket-Feed
    → Schreibtisch-Druckagent
    → lokaler Drucker
```

Der Browser dient evtl. nur zur Konfiguration (Station ↔ Drucker binden). Der Agent zieht oder empfängt Jobs kontinuierlich.

### Zwei Transport-Stile

| Stil | Funktionsweise | Achte auf |
|---|---|---|
| Cloud-Relay (z. B. PrintNode-ähnlich) | Server-API → Vendor-Cloud → lokaler Client | Account-Sicherheit, Mapping pro Standort |
| Self-hosted Pull | Agent pollt deine API mit Station-Token | Auth, Backoff, durable Queue |
| Transit / Relay für Designer-Stacks | Extra-Hop für hiprint-ähnliche Clients | Operative Komplexität |

## Design-Hinweise

- **Agent authentifizieren**; keinen rohen Druckport ins Internet stellen
- Job-Payload möglichst identisch zur lokalen SDK-Form (gleiches HTML/PDF/raw)
- Offline-Schreibtische mit durable Queues und Dead-Letter für Poison-Jobs
- `Job-ID → Station → Drucker → Ergebnis` für Ops loggen
- Vorlagen versionieren; schlechtes Deploy soll rollbackbar sein ohne doppelte History
- Rate-Limit pro Station, damit ein Schreibtisch andere nicht aushungert

## Sicherheits-Checkliste

- [ ] Station-Credentials rotierbar
- [ ] TLS zur API
- [ ] Agent bindet nur localhost für Browser-Features; Remote-Kanal ist outbound
- [ ] Kein „beliebige URL drucken“ aus untrusted Job-Feldern
- [ ] Audit, wer an welche Station enqueuen darf

## Ops-Runbook (Minimum)

1. Station offline → On-Call mit letztem Heartbeat
2. Drucker kein Papier → auf Station-UI / LED anzeigen, falls vorhanden
3. Poison-Template → Job quarantänisieren; Template-Owner alarmieren
4. Replay → nur für explizit fehlgeschlagene IDs

## Verwandtes

- [Batch- & Etikettendruck](batch-label-printing.de.md)
- [Einen Stack wählen](choose-silent-print-stack.de.md)
- Beispiele in freier Wildbahn: PrintNode, hiprint-Transit-Relays und lokale Agenten mit Remote-Job-Pull (siehe die [Hauptliste](../README.de.md#cloud--remote-print))
