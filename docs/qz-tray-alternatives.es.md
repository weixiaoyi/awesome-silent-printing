# Alternativas a QZ Tray

Buscar una **alternativa a QZ Tray** suele significar que quieres impresión silenciosa desde el navegador, pero con otro compromiso en estilo de API, precio, firma o flujo HTML/CSS.

## Cuándo permanecer en QZ Tray

- ESC/POS / ZPL raw es la carga principal
- Ya invertiste en firma y certificados de QZ
- Necesitas un historial POS global largo
- Tu equipo ya envuelve llamadas WebSocket de QZ en producción

## Cuándo evaluar alternativas

| Necesidad | Mira |
|---|---|
| Cliente JS comercial con amplio soporte de tipos de archivo | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| HTML/CSS estilo npm desde una SPA | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), y otros puentes con soporte HTML |
| API en la nube hacia muchas impresoras | [PrintNode](https://www.printnode.com/en) |
| Plantillas guiadas por diseñador | hiprint + electron-hiprint |
| Una sola marca de hardware | SDKs de Zebra / Epson / Star |

## Notas de migración

1. Inventaria qué trabajos son raw vs HTML/PDF — los raw son la reescritura costosa.
2. Vuelve a probar supuestos de firma / licencia; no asumas que el siguiente fabricante es «silencioso sin firma».
3. Mantén un escritorio QZ vivo mientras pilotas la alternativa.
4. Revalida Chrome Local Network Access en el puerto del nuevo agente.

## Comparar directamente

- [QZ Tray vs web-print-pdf vs JSPrintManager](qz-vs-jspm-vs-web-print-pdf.es.md)
- [Elegir un stack de impresión silenciosa](choose-silent-print-stack.es.md)
