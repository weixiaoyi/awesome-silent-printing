# Alternativas a Lodop

Lodop / C-Lodop sigue siendo habitual en muchos sistemas de negocio Windows. Los equipos suelen buscar alternativas cuando necesitan:

- Integración SPA moderna
- Soporte de SO de escritorio más amplio (escritorios macOS / Linux)
- Docs en inglés para equipos mixtos
- Comportamiento HTTPS + localhost más claro bajo las reglas actuales de Chromium

## Direcciones de reemplazo

| Necesidad | Candidatos |
|---|---|
| Documentos de negocio HTML/CSS | QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, hiprint + electron-hiprint |
| POS / etiquetas raw | [QZ Tray](https://qz.io/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| Nube hacia muchas impresoras | [PrintNode](https://www.printnode.com/en) |
| Permanecer en Lodop | Lodop7 / C-Lodop si el stack sigue encajando |

## Por qué se estancan las migraciones

- Las plantillas mezclan comandos propietarios de Lodop y fragmentos HTML
- Nombres de impresora y bandejas de papel están codificados en scripts antiguos
- Hospitales / ERPs temen cambiar un camino de impresión que funciona en temporada alta

## Consejos de migración

1. Inventaria plantillas: HTML vs comandos propietarios. Cuenta cuántas son «solo HTML ya».
2. Reconstruye documentos críticos como HTML/CSS cuando sea posible; deja lo raw exótico para una segunda ola.
3. Ejecuta agentes viejos y nuevos en paralelo en un escritorio piloto (puertos distintos).
4. Arregla HTTPS / Local Network Access antes del despliegue nacional.
5. Forma al helpdesk en el nuevo síntoma «agente no en marcha» — sustituye errores de la era ActiveX.

## Relacionado

- [Elegir un stack](choose-silent-print-stack.es.md)
- [window.print vs impresión silenciosa](window-print-vs-silent-print.es.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.es.md)
