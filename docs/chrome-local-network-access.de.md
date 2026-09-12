# Chrome Local Network Access & 127.0.0.1

## Symptom

Dev funktioniert. Produktion schlägt fehl mit etwas wie:

```text
WebSocket connection to 'ws://127.0.0.1:…' failed
Failed to connect to print agent
ERR_CONNECTION_REFUSED / net::ERR_FAILED
```

Der lokale Druckclient läuft. Firewall wirkt ok. Nur die **deployte Website** erreicht Loopback nicht.

Das ist einer der häufigsten „stille Druckausgabe ging nach Go-Live kaputt“-Vorfälle für jede localhost-Brücke.

## Ursache

Chromiums **Local Network Access (LNA)** schränkt ein, dass öffentliche Seiten mit dem lokalen Netz / Loopback sprechen. Druckagenten auf `127.0.0.1` liegen genau in dieser Zone.

Praktische Regeln, die Teams im Feld treffen:

| Kontext | Typisches Ergebnis |
|---|---|
| App unter `http://localhost` in Dev | Oft ok (Sonderkontext) |
| App unter plain **HTTP** in Produktion | Oft **still verweigert** |
| App unter **HTTPS** mit vertrauenswürdigem Zertifikat | Kann Local-Network-Berechtigung anfragen |
| Neueres Chrome + WebSocket zu Loopback | Gleiche LNA-Regeln für `ws://127.0.0.1…` |
| Edge / andere Chromium | Ähnliche Richtlinien |

Der Agent „läuft“ ist also nötig, aber nicht ausreichend. Der **Browser-Origin** muss Loopback ansprechen dürfen.

## Entscheidungs-Flowchart

```text
Kann die Seite ws://127.0.0.1 / http://127.0.0.1 erreichen?
│
├─ Nein, und Seite ist HTTP
│     → Seite zuerst auf HTTPS. Hier stoppen, bis das live ist.
│
├─ Nein, und Seite ist HTTPS
│     → Local-Network-Berechtigung / Prompt prüfen
│     → Agent-Port + Prozess bestätigen
│     → Auf derselben Maschine mit kleinem WS-Client testen
│
└─ Ja, aber Druck schlägt trotzdem fehl
      → Druckername, Treiber, Spooler, Vorlage — nicht LNA
```

## Was tun (Checkliste)

1. **Web-App über HTTPS** ausliefern mit einem Zertifikat, dem der Arbeitsplatz vertraut (öffentliche CA oder interne PKI). Self-signed Zertifikate ohne Nutzervertrauen scheitern weiter.
2. Beim ersten Druck auf den **Local Network**-Prompt des Browsers achten und für deinen Origin erlauben.
3. Bestätigen, dass der Desktop-Druckagent auf `127.0.0.1` lauscht (und dem Port, den dein SDK erwartet).
4. Mit DevTools → Network reproduzieren: WS/HTTP blockiert, refused oder reset?
5. **Nur für verwaltete Flotten**: Browser-Flags / Enterprise-Policy können Checks lockern. Keine Strategie für öffentliche SaaS-Endnutzer.
6. Den Berechtigungsschritt im Install-Runbook dokumentieren; sonst installiert der Helpdesk den Agent endlos neu.

## LNA von „Agent down“ unterscheiden

| Check | Agent down | LNA / Origin-Problem |
|---|---|---|
| `127.0.0.1:port` von lokalem Tool | Fehlgeschlagen | Erfolgreich |
| Gleiche Maschine, Seite HTTP | Kann fehlschlagen | Oft fehlgeschlagen |
| Gleiche Maschine, HTTPS + Berechtigung | Ok wenn Agent läuft | Ok |
| Anderes Chrome-Nutzerprofil | Gleich | Berechtigung evtl. fehlend |

## Wer betroffen ist

Jede localhost-Druckbrücke: QZ Tray, web-print-pdf, JSPrintManager, Lodop-ähnliche lokale Dienste, Custom-Agenten. Kein Single-Vendor-Bug.

## Ausführlicher Walkthrough

- Englisch: [WebSocket to 127.0.0.1 failed after deploy](https://webprintpdf.com/en/docs/production-print-troubleshoot/)
- 中文: [上线后连接 127.0.0.1 失败](https://webprintpdf.com/docs/production-print-troubleshoot/)

## Verwandtes

- [Wie stille Druckausgabe funktioniert](how-silent-printing-works.de.md)
- [Stille Druckausgabe in Vue / React](vue-react-silent-print.de.md)
- [Einen Stack wählen](choose-silent-print-stack.de.md)
