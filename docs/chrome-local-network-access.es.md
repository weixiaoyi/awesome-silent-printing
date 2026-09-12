# Chrome Local Network Access y 127.0.0.1

## Síntoma

En desarrollo funciona. En producción falla con algo como:

```text
WebSocket connection to 'ws://127.0.0.1:…' failed
Failed to connect to print agent
ERR_CONNECTION_REFUSED / net::ERR_FAILED
```

El cliente de impresión local está en marcha. El firewall parece bien. Solo el **sitio web desplegado** no puede llegar a loopback.

Es uno de los incidentes más comunes de «la impresión silenciosa se rompió tras el go-live» para cualquier puente localhost.

## Causa

**Local Network Access (LNA)** de Chromium restringe que las páginas públicas hablen con la red local / loopback del usuario. Los agentes de impresión que escuchan en `127.0.0.1` caen justo en esa zona restringida.

Reglas prácticas que los equipos encuentran en el campo:

| Contexto | Resultado típico |
|---|---|
| App en `http://localhost` en dev | Suele funcionar (contexto especial) |
| App en **HTTP** plano en producción | Suele ser **denegado en silencio** |
| App en **HTTPS** con certificado de confianza | Puede pedir permiso de Local Network |
| Chrome más reciente + WebSocket a loopback | Mismas reglas LNA para `ws://127.0.0.1…` |
| Edge / otros Chromium | Políticas similares |

Así que el agente «activo» es necesario pero no suficiente. El **origen del navegador** debe poder hablar con loopback.

## Diagrama de flujo de decisión

```text
¿Puede la página llegar a ws://127.0.0.1 / http://127.0.0.1?
│
├─ No, y el sitio es HTTP
│     → Pon el sitio en HTTPS primero. Para aquí hasta que se despliegue.
│
├─ No, y el sitio es HTTPS
│     → Revisa permiso / aviso de Local Network
│     → Confirma puerto + proceso del agente
│     → Prueba desde la misma máquina con un cliente WS mínimo
│
└─ Sí, pero la impresión sigue fallando
      → Nombre de impresora, driver, cola, plantilla — no es LNA
```

## Qué hacer (checklist)

1. **Sirve la aplicación web por HTTPS** con un certificado en el que la estación confíe (CA pública o tu PKI interna). Certificados autofirmados no confiados seguirán fallando.
2. En la primera impresión, observa el aviso de permiso **Local Network** del navegador y permítelo para tu origen.
3. Confirma que el agente de impresión de escritorio escucha en `127.0.0.1` (y el puerto que espera tu SDK).
4. Reproduce con DevTools → Network: ¿la llamada WS/HTTP está bloqueada, rechazada o reseteada?
5. Solo para **flotas gestionadas**: banderas del navegador / política empresarial pueden relajar comprobaciones. No es estrategia para usuarios finales SaaS públicos.
6. Documenta el paso de permiso en tu runbook de instalación; helpdesks reinstalarán el agente para siempre si no.

## Cómo distinguir LNA de «agente caído»

| Comprobación | Agente caído | LNA / problema de origen |
|---|---|---|
| `127.0.0.1:puerto` desde herramienta local | Falla | Funciona |
| Misma máquina, sitio en HTTP | Puede fallar | Suele fallar |
| Misma máquina, sitio HTTPS + permiso | Funciona si el agente está arriba | Funciona |
| Perfil de usuario distinto en Chrome | Igual | Puede faltar permiso |

## Quién se ve afectado

Cualquier puente de impresión localhost: QZ Tray, web-print-pdf, JSPrintManager, servicios locales estilo Lodop, agentes personalizados. No es un bug de un solo fabricante.

## Guía más detallada

- English: [WebSocket to 127.0.0.1 failed after deploy](https://webprintpdf.com/en/docs/production-print-troubleshoot/)
- 中文: [上线后连接 127.0.0.1 失败](https://webprintpdf.com/docs/production-print-troubleshoot/)

## Relacionado

- [Cómo funciona la impresión silenciosa](how-silent-printing-works.es.md)
- [Impresión silenciosa en Vue / React](vue-react-silent-print.es.md)
- [Elegir un stack](choose-silent-print-stack.es.md)
