# Impresión silenciosa con HTML/CSS

## Por qué los equipos lo quieren

La mayoría de UIs de negocio ya son HTML/CSS. Si la impresión silenciosa puede reutilizar las mismas plantillas, la velocidad del frontend se mantiene alta y evitas mantener un segundo skill set ZPL/ESC/POS para cada factura.

## Dos modelos de diseño

| Modelo | Ventajas | Desventajas |
|---|---|---|
| HTML/CSS vía Chromium/agente local | Familiar para equipos web; buena fidelidad | Necesita un agente local |
| ESC/POS / ZPL raw | Control preciso del dispositivo | Skill set distinto; específico del hardware |

Muchos productos mezclan ambos: documentos A4 en HTML, tickets térmicos en raw.

## Qué significa «print CSS» aquí

Solo `@media print` del navegador **no** es un camino silencioso — sigue pasando por `window.print()`. Con un agente local normalmente:

1. Construyes una cadena HTML autocontenida (o URL).
2. Incluyes el CSS que el agente necesita (inline, URL absoluta enlazada o empaquetado).
3. Pasas tamaño de papel / márgenes / nombre de impresora en opciones del SDK.
4. Dejas que el motor del agente (a menudo Chromium) pagine y envíe a la cola.

### Checklist de plantilla

- [ ] Tamaño de página explícito (A4, 100×150 mm, rollo 80 mm, …)
- [ ] Márgenes que coincidan con el soporte físico
- [ ] Fuentes instaladas en la estación o embebidas
- [ ] Código de barras/QR como SVG o imagen de alta resolución (probar escaneo)
- [ ] Tablas que no recorten la última fila
- [ ] Sin dependencia de layouts solo de viewport (`100vh` es trampa)

## Consejos prácticos

- Diseña una hoja de estilos **solo para impresión**; no reutilices a ciegas todo el CSS de la app.
- Prefiere unidades `mm` / `in` para etiquetas; solo `px` deriva entre DPI.
- Prueba fuentes chinas / CJK en Windows **y** escritorios Linux si soportas ambos.
- Previsualiza un trabajo antes de activar lotes al migrar plantillas.
- Mantén imágenes pequeñas; PNG gigantes matan el throughput por lotes.
- Para cortadores térmicos / cajones exactos, puede que aún necesites comandos raw en un puente con soporte raw.

## Buen encaje

Facturas, extractos, albaranes, informes A4, muchos diseños de etiqueta renderizados como HTML.

## Mal encaje (considera raw / SDK del fabricante)

- Impresoras de cocina ultra rápidas que esperan solo ESC/POS
- Flotas Zebra estandarizadas en plantillas ZPL en firmware de impresora
- Dispositivos sin un driver Windows/macOS/Linux usable excepto raw del fabricante

## Relacionado

- [Impresión silenciosa en Vue / React](vue-react-silent-print.es.md)
- [Impresión por lotes y etiquetas](batch-label-printing.es.md)
- [Impresión silenciosa de recibos térmicos](thermal-receipt-silent-print.es.md)
- [Elegir un stack](choose-silent-print-stack.es.md)
