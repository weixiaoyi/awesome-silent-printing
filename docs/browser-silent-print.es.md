# Impresión silenciosa en el navegador

**Impresión silenciosa en el navegador** significa que una página web envía un trabajo a una impresora local **sin** el cuadro de diálogo de impresión del navegador.

Este hub cubre los caminos habituales que la gente recorre en la práctica:

- impresión silenciosa desde una aplicación web
- imprimir sin diálogo en Chrome
- impresión silenciosa en Vue / React
- alternativas a `window.print()`

## Respuesta breve

Un sitio web normal no puede, por sí solo, controlar en silencio una impresora arbitraria. Necesitas un **agente local**, un **SDK del fabricante**, una **política de quiosco** o un **shell de escritorio**.

Si alguien dice «impresión silenciosa con JavaScript puro en Chrome para cualquier impresora», pregúntale qué componente local instala. Ese componente es el verdadero camino hacia la impresora.

## Modelo mental en un minuto

```text
Página (HTTPS)
  → puente localhost
  → cola del SO o puerto raw
  → impresora física
```

Detalles: [Cómo funciona la impresión silenciosa](how-silent-printing-works.es.md).

## Elige tu siguiente página

| Tu situación | Lee |
|---|---|
| Necesitas la arquitectura | [Cómo funciona la impresión silenciosa](how-silent-printing-works.es.md) |
| Necesitas elegir un stack | [Elegir un stack de impresión silenciosa](choose-silent-print-stack.es.md) |
| Vienes de `window.print` | [window.print vs impresión silenciosa](window-print-vs-silent-print.es.md) |
| Producción no puede llegar a `127.0.0.1` | [Chrome Local Network Access](chrome-local-network-access.es.md) |
| Integración en SPA | [Impresión silenciosa en Vue / React](vue-react-silent-print.es.md) |
| Plantillas HTML | [Impresión silenciosa con HTML/CSS](html-css-silent-print.es.md) |
| Etiquetas / volumen de almacén | [Impresión por lotes y etiquetas](batch-label-printing.es.md) |
| WMS envía trabajos a puestos | [Impresión silenciosa remota](remote-silent-print.es.md) |
| Recibos / tickets de cocina | [Impresión silenciosa de recibos térmicos](thermal-receipt-silent-print.es.md) |
| Comparar puentes principales | [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.es.md) |

## Lista de herramientas

Consulta la lista curada: [Awesome Silent Printing](../README.es.md).
