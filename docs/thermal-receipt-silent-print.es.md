# Impresión silenciosa de recibos térmicos desde el navegador

**Imprimir recibos térmicos desde una página web** suele necesitar impresión silenciosa: cajeros y estaciones de cocina no pueden hacer clic en un diálogo en cada ticket.

## Qué funciona

1. **Puente de impresión local** — HTML/imagen/PDF para tickets simples, o ESC/POS raw para control total del dispositivo
2. **SDK del fabricante** para impresoras en red clase Epson / Star (la página habla con la impresora o servicio del fabricante)
3. **Shell de escritorio** con impresión silenciosa si distribuyes una app POS Electron

## Qué no funciona

- `window.print()` como camino silencioso de producción
- Bibliotecas de solo descarga PDF sin agente de impresión local
- Asumir que toda «página CSS de 80 mm» cortará y abrirá el cajón sin comandos raw

## Elecciones de arquitectura

| Necesidad | Inclínate hacia |
|---|---|
| Logo + diseño HTML variable, bajo volumen | HTML vía agente local |
| Cocina alto volumen, cortador, cajón, zumbador | ESC/POS raw en QZ / JSPM / SDK del fabricante |
| Solo Epson/Star en LAN | SDK estilo ePOS / webPRNT del fabricante |
| Ya distribuyes una app POS de escritorio | Impresión silenciosa Electron |

## Consejos de plantilla

- Prefiere diseños de ancho fijo (p. ej. **58 mm / 80 mm**)
- Mantén códigos de barras de alto contraste; prueba con el escáner que uses de verdad
- Evita grids CSS pesados; motores térmicos y drivers no perdonan
- Prueba comandos de cortador / cajón **solo** en stacks con soporte raw
- Codifica páginas de código correctamente para CJK / texto acentuado en ESC/POS
- Imprime un ticket de calibración tras actualizaciones de driver o agente

## Mostrador vs cocina

| | Mostrador | Cocina |
|---|---|---|
| Tolerancia a latencia | Baja | Muy baja |
| Payload típico | HTML o ESC/POS | A menudo ESC/POS |
| UX de fallo | Mostrar reintento al cajero | Reintento automático + alerta fuerte |
| Multi-impresora | Recibo + etiqueta | Enrutar por estación / ítem |

## Relacionado

- [Impresión por lotes y etiquetas](batch-label-printing.es.md)
- [Impresión silenciosa con HTML/CSS](html-css-silent-print.es.md)
- [Elegir un stack de impresión silenciosa](choose-silent-print-stack.es.md)
- Lista de herramientas: [Awesome Silent Printing](../README.es.md)
