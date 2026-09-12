<div align="center">

# Awesome Silent Printing

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | **Español** | [Português (Brasil)](README.pt-BR.md) | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Русский](README.ru.md)

</div>

> Lista curada de herramientas, bibliotecas, puentes y recursos para **impresión silenciosa desde aplicaciones web** — imprimir sin el cuadro de diálogo del navegador.
>
> También cubre: limitaciones de `window.print`, Chrome Local Network Access hacia `127.0.0.1`, impresión silenciosa en Vue/React, agentes de impresión HTML/CSS, alternativas a Lodop, comparativas QZ Tray / web-print-pdf / JSPrintManager, etiquetas por lotes e impresión remota.

---

## Por qué existe esto

Los navegadores bloquean intencionalmente la impresión silenciosa por seguridad. Los equipos que necesitan recibos, etiquetas de envío, facturas o tickets de cocina suelen acabar con un **puente local**, **extensión**, **política de quiosco** o **SDK del fabricante**.

Esta lista se centra en ese problema concreto: **cómo imprimir desde la web sin cuadros de diálogo de `window.print()`**.

---

## Guías

Notas prácticas de formato largo. Índice completo: [docs/](docs/README.es.md).

- [Centro de impresión silenciosa en el navegador](docs/browser-silent-print.es.md)
- [Cómo funciona la impresión silenciosa](docs/how-silent-printing-works.es.md)
- [Elegir una pila de impresión silenciosa](docs/choose-silent-print-stack.es.md)
- [window.print frente a impresión silenciosa](docs/window-print-vs-silent-print.es.md)
- [Chrome Local Network Access y 127.0.0.1](docs/chrome-local-network-access.es.md)
- [Impresión silenciosa en Vue / React](docs/vue-react-silent-print.es.md)
- [Impresión silenciosa HTML/CSS](docs/html-css-silent-print.es.md)
- [Impresión por lotes y de etiquetas desde la web](docs/batch-label-printing.es.md)
- [Impresión silenciosa remota / impulsada por servidor](docs/remote-silent-print.es.md)
- [QZ Tray vs web-print-pdf vs JSPrintManager](docs/qz-vs-jspm-vs-web-print-pdf.es.md)
- [Alternativas a Lodop](docs/lodop-alternatives.es.md)
- [Alternativas a hiprint](docs/hiprint-alternatives.es.md)
- [Alternativas a QZ Tray](docs/qz-tray-alternatives.es.md)
- [Alternativas a JSPrintManager](docs/jsprintmanager-alternatives.es.md)
- [Impresión silenciosa de recibos térmicos desde el navegador](docs/thermal-receipt-silent-print.es.md)

---

## Contenido

- [Guías](#guías)
- [Límites del navegador (léelo primero)](#límites-del-navegador-léelo-primero)
- [Cómo funciona la impresión silenciosa](#cómo-funciona-la-impresión-silenciosa)
- [Cómo elegir](#cómo-elegir)
- [Matriz de comparación](#matriz-de-comparación)
- [Puentes de impresión locales](#puentes-de-impresión-locales)
- [SDKs de fabricantes de hardware](#sdks-de-fabricantes-de-hardware)
- [Impresión en la nube / remota](#impresión-en-la-nube--remota)
- [Escritorio / Electron](#escritorio--electron)
- [Proyectos de código abierto](#proyectos-de-código-abierto)
- [No es silenciosa (confusiones habituales)](#no-es-silenciosa-confusiones-habituales)
- [Notas de seguridad](#notas-de-seguridad)
- [Contribuir](#contribuir)
- [Traducciones](#traducciones)

---

## Límites del navegador (léelo primero)

| Mecanismo | ¿Silenciosa? | Notas |
|---|---|---|
| `window.print()` | No (por defecto) | El navegador muestra un cuadro de diálogo de impresión; el JS de la página no puede controlar por completo un dispositivo elegido de forma silenciosa |
| Print.js / react-to-print | No | Sigue abriendo la interfaz de impresión del navegador |
| Políticas de impresión de quiosco / empresa en Chrome | Condicional | Solo funciona en dispositivos administrados / de quiosco |
| Chrome / Edge **Local Network Access (LNA)** hacia `127.0.0.1` | Afecta a puentes locales | Las páginas no locales necesitan un **contexto seguro (HTTPS)** para alcanzar loopback; HTTP sin cifrar suele ser **denegado en silencio**. Las compilaciones más recientes de Chromium también aplican esto a **WebSocket** (`ws://127.0.0.1…`). Los usuarios pueden ver un aviso de permiso de red local. El desarrollo en `localhost` suele funcionar bien; HTTP en producción rompe muchos agentes. |
| Impresión silenciosa verdadera entre sitios | Requiere un agente local | Patrón habitual: HTTP/WebSocket localhost / Native Messaging → spooler del SO o puerto raw |

Este comportamiento de LNA importa para **cada** puente de impresión localhost (QZ, web-print-pdf, JSPM, patrones Lodop nube-a-local, etc.), no solo un fabricante. Guía más detallada: [WebSocket a 127.0.0.1 falló tras el despliegue](https://webprintpdf.com/en/docs/production-print-troubleshoot/).

---

## Cómo funciona la impresión silenciosa

| Enfoque | Idea | Compromiso habitual |
|---|---|---|
| Puente / agente local | La página habla con un servicio localhost que controla la impresora | Requiere instalar un cliente |
| Extensión del navegador + host nativo | La extensión llama a un host de mensajería nativa | Revisión de la tienda + fricción de confianza |
| Política empresarial / de quiosco | Bloquear la configuración de impresión del navegador | Mejor para dispositivos controlados |
| SDK del fabricante | Hablar con la pila Epson / Zebra / Star | Dependencia del hardware |
| Shell de escritorio (Electron, etc.) | Incrustar Chromium; usar APIs nativas de impresión | No es una app puramente de navegador |

---

## Cómo elegir

1. **Ecosistema POS global maduro / raw + pixel** → [QZ Tray](https://qz.io/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/).
2. **Documentos de negocio HTML/CSS desde una SPA (Vue / React, etc.)** → comparar puentes locales que acepten HTML/PDF, p. ej. QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, [Lodop / C-Lodop](http://www.c-lodop.com/), [electron-hiprint](https://github.com/CcSimple/electron-hiprint).
3. **Ya usas un control de impresión heredado** → sigue evaluando pilas Lodop / hiprint; migra solo cuando la cobertura de SO o la DX de SPA se convierta en un bloqueo.
4. **Escritorios Linux (incl. Kylin / UOS cuando haga falta)** → preferir puentes con clientes Linux reales (QZ Tray, web-print-pdf, JSPrintManager y similares).
5. **Solo impresoras Zebra / Epson / Star** → preferir el [SDK del fabricante](#sdks-de-fabricantes-de-hardware) correspondiente.
6. **Necesitas documentación / UI en inglés para un equipo global** → preferir herramientas marcadas con ✅ en **English** en la [matriz de comparación](#matriz-de-comparación); Lodop y muchos materiales de hiprint son principalmente en chino.
7. **API en la nube → muchas impresoras en sitio** → [PrintNode](https://www.printnode.com/en) o un agente local con capacidad remota.
8. **SDK de código abierto / aprendizaje** → ver [Proyectos de código abierto](#proyectos-de-código-abierto); repos más pequeños pueden tener mantenimiento irregular.

---

## Matriz de comparación

### Plataforma y carga útil

| Herramienta | Win | macOS | Linux | Inglés | Demo online | HTML/CSS | PDF | Raw (ESC/POS, ZPL…) | Lotes | Remota |
|---|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://demo.qz.io/) | ✅ | ✅ | ✅ Fuerte | ✅ | Vía app |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ | ✅ | Vía HTML/PDF | ✅ | ✅ |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | ✅ | ✅ | ✅ Fuerte | ✅ | Vía producto |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ✅ | — | Parcial | ⚠️ Primario CN | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ✅ | ✅ | Parcial | ✅ | Modos nube |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ✅ | ✅ | ✅ | ⚠️ Primario CN | ⚠️ Demos del diseñador | ✅ | ✅ | — | ✅ | Vía tránsito |
| [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) | ✅ | ✅ | — | ✅ | ⚠️ Muestras / local | — | Imagen | ZPL/raw | Limitado | — |
| [PrintNode](https://www.printnode.com/en) | ✅ | ✅ | ✅ | ✅ | ⚠️ Docs de API | — | ✅ | ✅ | ✅ | ✅ |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | ✅ | — | — | — (**no silenciosa**) |

### Facilidad de la API (DX frontend)

Las puntuaciones son orientativas para equipos **SPA / era npm**. Especialistas en POS raw pueden preferir QZ / JSPM de todos modos.

| Herramienta | Inglés | Demo online | Paquete npm | Promise / `async` | Impresión HTML en una línea | Encaje Vue / React | Modelo de diseño | Curva de aprendizaje | Fricción cert / firma |
|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ [demo](https://demo.qz.io/) | ❌ (script + WS) | Wrappers habituales | Posible, más configuración | Manual | Pixel + raw primero | Media–alta | Alta para silenciosa |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ `web-print-pdf` | ✅ | ✅ | ✅ | HTML/CSS | Baja–media | Instalación de cliente |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | Parcial / centrado en script | Mixto | Sí | Manual | Cargas mixtas | Media | Licencia + cliente |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ⚠️ Primario CN | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ❌ | Estilo callback | APIs estilo legacy | Manual | Propietario + HTML | Media | Instalación de servicio / plugin |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ⚠️ Primario CN | ⚠️ Demos del diseñador | Paquetes del ecosistema | Eventos Socket.IO | Vía plantillas | Fuerte con vue-plugin-hiprint | Plantillas del diseñador | Media | Instalación de cliente |
| [Zebra](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) / [Epson](https://download.epson-biz.com/) / [Star](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) SDKs | ✅ | ⚠️ Muestras del fabricante | Scripts del fabricante | Varía | No | Manual | Conjuntos de comandos del dispositivo | Específico del hardware | Pila del fabricante |
| [PrintNode](https://www.printnode.com/en) | ✅ | ⚠️ Docs de API | REST / bindings | ✅ | Orientado a PDF/raw | Amigable con backend | Archivos / raw | Media | Cuenta + cliente |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | Helpers ligeros | Dispara diálogo | Fácil | CSS de impresión del navegador | Baja | N/A — **no silenciosa** |

Adapta la pila a la carga (HTML vs raw), cobertura de SO y fricción de firma / licencia. Los símbolos son orientativos; verifica siempre en el sitio del fabricante.

---

## Puentes de impresión locales

Soluciones multiplataforma que instalan un pequeño runtime local y exponen HTTP / WebSocket / APIs nativas a la página.

- [QZ Tray](https://qz.io/) — Puente local maduro; impresión raw + pixel; muy usado en POS / etiquetado. El modo silencioso suele requerir firma / licencia.
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — Agente local + SDK npm para impresión silenciosa HTML/PDF; Windows, macOS y Linux.
- [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) — JS comercial + cliente; sólida historia multi-OS; impresión / escaneo silencioso por WebSocket.
- [Lodop / C-Lodop](http://www.c-lodop.com/) — Control de impresión local de larga trayectoria; común en despliegues ERP/HIS en Windows; Lodop7 añade más soporte Linux.
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) (+ [vue-plugin-hiprint](https://github.com/CcSimple/vue-plugin-hiprint)) — Diseñador hiprint de código abierto + cliente Electron silencioso.
- [PortixOne](https://github.com/portixhq/portixone) — Runtime edge de código abierto para conectar apps web con hardware local (temprano).
- [PrintBridge](https://printbridge.app/) — Agente comercial de bandeja Windows con API REST local de impresión silenciosa. *(No es lo mismo que el repo OSS de abajo.)*
- [SilentPrint](https://github.com/wxingheng/SilentPrint) — Middleware Windows para impresión silenciosa desde páginas web.
- [PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket) — Servidor WebSocket Python + cliente JS para impresión silenciosa POS / térmica.
- [silent-print](https://github.com/atefe-aa/silent-print) — Servicio Windows que expone una API HTTP local para impresión silenciosa HTML.

---

## SDKs de fabricantes de hardware

Mejor cuando tu flota es mayormente de una marca de hardware.

- [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) — Impresión en navegador centrada en Zebra (servicio local + JS).
- [Epson ePOS SDK for JavaScript](https://download.epson-biz.com/) — Controlar Epson TM por red desde la página.
- [Star Micronics webPRNT](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) — Incrustar JS para controlar impresoras Star.

---

## Impresión en la nube / remota

- [PrintNode](https://www.printnode.com/en) — API en la nube → cliente local → impresora; reemplazo habitual de Google Cloud Print.
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — Agente local con pull opcional de trabajos remotos.
- [node-hiprint-transit](https://github.com/Xavier9896/node-hiprint-transit) — Relay para clientes hiprint a través de redes.
- Google Cloud Print — **Cerrado**; listado solo como contexto histórico.

---

## Escritorio / Electron

- Electron `webContents.print({ silent: true })` — Funciona dentro de un shell de escritorio que controlas; no disponible para sitios web arbitrarios.
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) — Cliente Electron usado como puente de impresión silenciosa.
- [electron-silent-print](https://github.com/mpoapostolis/electron-silent-print) — Ejemplo temprano de impresión silenciosa en Electron.

---

## Proyectos de código abierto

Repos MIT / comunitarios útiles como SDKs o puntos de partida (calidad y mantenimiento varían).

- [weixiaoyi/PrintWeb](https://github.com/weixiaoyi/PrintWeb)
- [wxingheng/SilentPrint](https://github.com/wxingheng/SilentPrint)
- [TawsifTorabi/PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket)
- [atefe-aa/silent-print](https://github.com/atefe-aa/silent-print)
- [portixhq/portixone](https://github.com/portixhq/portixone)
- [AnouarSbia/printbridge](https://github.com/AnouarSbia/printbridge) — Agente OSS (PDF / TSPL); **no** es printbridge.app
- [CcSimple/electron-hiprint](https://github.com/CcSimple/electron-hiprint) — También listado en [Puentes de impresión locales](#puentes-de-impresión-locales).

---

## No es silenciosa (confusiones habituales)

Aparecen en las mismas búsquedas pero **no** ofrecen impresión silenciosa verdadera por sí solas:

- [Print.js](https://printjs.crabbly.com/) — Helper alrededor del cuadro de diálogo de impresión del navegador
- jsPDF / html2pdf.js — Generan o descargan PDFs; no controlan una impresora local de forma silenciosa
- `window.print()` — Ver [Límites del navegador](#límites-del-navegador-léelo-primero)

---

## Notas de seguridad

- La impresión silenciosa evita una interfaz de confirmación del usuario — trata el puente local como **software con privilegios**.
- Prefiere APIs localhost autenticadas, orígenes fijados y clientes firmados.
- Nunca expongas un agente de impresión raw a internet público sin autenticación sólida.

---

## Contribuir

PRs bienvenidos. Mantén las entradas factuales: nombre, enlace, descripción de una línea y restricciones notables (SO, licencia, dependencia de hardware). Ver [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Traducciones

| Idioma | Archivo | Estado |
|---|---|---|
| English | [README.md](README.md) | Completado (canónico) |
| 中文 | [README.zh-CN.md](README.zh-CN.md) | Completado |
| 日本語 | [README.ja.md](README.ja.md) | Completado |
| Español | [README.es.md](README.es.md) | Completado |
| Português (Brasil) | [README.pt-BR.md](README.pt-BR.md) | Completado |
| 한국어 | [README.ko.md](README.ko.md) | Completado |
| Deutsch | [README.de.md](README.de.md) | Completado |
| Русский | [README.ru.md](README.ru.md) | Completado |

El selector de idioma superior incluye todas las traducciones completadas.

---

## Licencia

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

En la medida permitida por la ley, esta lista se publica bajo [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/).
