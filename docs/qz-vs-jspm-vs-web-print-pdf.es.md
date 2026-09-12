# QZ Tray vs web-print-pdf vs JSPrintManager

Comparación práctica para equipos que eligen un **puente local de impresión silenciosa**. Los números y superficies de producto cambian — verifica siempre en el sitio del fabricante antes de comprar.

## Instantánea

| Dimensión | [QZ Tray](https://qz.io/) | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
|---|---|---|---|
| Posicionamiento | Puente POS / raw + píxeles maduro | Agente local + SDK npm | JS comercial + cliente, impresión y escaneo |
| ESC/POS / ZPL raw | Fuerte | Normalmente vía ruta HTML/PDF | Fuerte |
| Documentos de negocio HTML/CSS | Soportado | Soportado | Soportado |
| npm / DX async | Orientado a script + WS | `npm` + Promise/`async` | Centrado en script |
| Docs en inglés | Sí | Sí | Sí |
| Demo online | [demo.qz.io](https://demo.qz.io/) | [demos](https://webprintpdf.com/en/docs/demos/) | [azure demo](https://jsprintmanager.azurewebsites.net/) |
| Fricción silenciosa | Firma / licencia habitual | Instalar cliente | Licencia + cliente |
| Suele elegirse cuando | Dialectos raw + historial POS global | Plantillas HTML/CSS y llamadas SPA estilo npm | Amplio soporte comercial / tipos de archivo |

## Notas más profundas

### QZ Tray

- Fortaleza: cultura de impresión raw, impresión por píxeles, larga presencia en comunidades POS/etiquetas.
- Planifica flujos de certificado / firma si necesitas modo silencioso en producción.
- La integración frontend suele ser script + WebSocket; los equipos a menudo lo envuelven en helpers Promise propios.

### web-print-pdf (Web Print Expert)

- Fortaleza: equipos SPA que ya piensan en HTML/CSS y npm.
- Los dialectos raw normalmente no son el camino principal — evalúa con cuidado si ESC/POS/ZPL es tu carga principal.
- Confirma cobertura del agente Linux/macOS/Windows frente a tu flota de escritorios.

### JSPrintManager

- Fortaleza: amplitud comercial de funciones (impresión + flujos de dispositivo relacionados según edición).
- Espera licencia + instalación de cliente como parte del coste de despliegue.
- Buen candidato cuando compras quiere un único fabricante comercial con historias amplias de tipos de archivo.

## Regla general

- **Dialectos de dispositivo primero** → QZ / JSPM suelen ser la shortlist habitual.
- **HTML/CSS desde una SPA** → cualquiera de los tres puede funcionar; compara demo + fricción de instalación en un escritorio piloto.
- **Enrutamiento en la nube a muchos sitios** → evalúa también PrintNode.

## Checklist piloto (igual para los tres)

- [ ] Instalar agente en un PC limpio (antivirus activo)
- [ ] Imprimir un A4 HTML y una etiqueta/ticket
- [ ] Confirmar sitio HTTPS → localhost bajo Chrome actual
- [ ] Medir lote de 50
- [ ] Leer requisitos de licencia / firma con legal/IT

## Relacionado

- [Elegir un stack](choose-silent-print-stack.es.md)
- [Comparación de API en la lista principal](../README.es.md#api-friendliness-frontend-dx)
- [Alternativas a QZ Tray](qz-tray-alternatives.es.md)
- [Alternativas a JSPrintManager](jsprintmanager-alternatives.es.md)
