# Impresión por lotes y etiquetas desde la web

## Problema

Almacenes y mostradores de ecommerce suelen necesitar **decenas o cientos** de etiquetas sin que un humano haga clic en diálogos de impresión. Un diálogo por etiqueta no es un diseño operativo — es una incidencia.

## Qué necesitas

1. Un camino silencioso hacia una impresora **nombrada**
2. Una cola de trabajos / API por lotes (un bucle en el cliente no basta a escala)
3. Renderizado estable de plantillas (HTML o ZPL)
4. Reintento + logging cuando un trabajo falla a mitad de lote
5. Back-pressure cuando la impresora es más lenta que quien envía

## Patrones

| Patrón | Notas | Mejor cuando |
|---|---|---|
| Navegador → API por lotes del agente local | Latencia mínima en el PC de escritorio | El operador trabaja dentro de la SPA |
| Push del servidor → pull del agente en escritorio | Mejor para muchas estaciones de empaquetado | El navegador no debe permanecer abierto |
| Raw del fabricante (ZPL) | Excelente para impresoras solo de etiquetas | Flota ya estandarizada en ZPL |

## Flujo por lotes recomendado

```text
Seleccionar pedidos
  → renderizar o obtener N plantillas
  → enviar como lote (o lotes fragmentados de 20–50)
  → mostrar estado por trabajo (en cola / imprimiendo / hecho / fallido)
  → reintentar solo ids fallidos
```

### Fragmentación

Enviar 500 trabajos en un Promise.all suele perjudicar más que ayudar. Prefiere fragmentos:

- 20–50 trabajos por fragmento para agentes que renderizan HTML
- Fragmentos más grandes pueden ir bien para cadenas ZPL pequeñas
- Espera la finalización del fragmento (o límite de concurrencia 2–3) antes del siguiente

## Taxonomía de fallos

| Fallo | Acción del operador | Acción del sistema |
|---|---|---|
| Agente offline | Instalar / iniciar agente | Pausar cola; banner |
| Impresora offline / sin papel | Arreglar hardware | Marcar trabajos como reintentables |
| Plantilla / código de barras malo | Corregir datos | Fallar ese trabajo; continuar otros |
| LNA / HTTPS | IT corrige origen | Ver [Chrome LNA](chrome-local-network-access.es.md) |

## Checklist

- [ ] Selección de impresora por tamaño de papel / estación
- [ ] Envío por lotes + estado por trabajo
- [ ] Página HTTPS puede llegar a localhost (LNA)
- [ ] Pruebas de regresión de plantilla para código de barras/QR
- [ ] Job ids idempotentes (reenvío seguro)
- [ ] Ops puede exportar ids de trabajos fallidos

## Relacionado

- [Impresión silenciosa remota](remote-silent-print.es.md)
- [Impresión silenciosa con HTML/CSS](html-css-silent-print.es.md)
- [Impresión silenciosa de recibos térmicos](thermal-receipt-silent-print.es.md)
- [Elegir un stack](choose-silent-print-stack.es.md)
