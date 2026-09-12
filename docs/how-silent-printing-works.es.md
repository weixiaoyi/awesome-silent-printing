# Cómo funciona la impresión silenciosa

Los navegadores están diseñados para **impedir** que una página web envíe trabajos en silencio a una impresora física. Por eso `window.print()` muestra un diálogo, y por eso «impresión silenciosa con JavaScript puro» suele ser un callejón sin salida.

Si estás construyendo recibos, etiquetas de envío, facturas o tickets de cocina desde una aplicación web, necesitas una arquitectura distinta — no una bandera del navegador.

## El patrón real

Casi toda configuración de impresión silenciosa en producción se ve así:

```text
Aplicación web (navegador)
    → HTTP / WebSocket / Native Messaging en localhost
    → agente local / servicio del fabricante / shell de escritorio
    → cola de impresión del SO o puerto raw del dispositivo
```

La página nunca es dueña de la impresora. Un **componente local de confianza** lo es. Ese componente se instala una vez por estación de trabajo (o se incluye en una imagen de quiosco), y luego la aplicación web habla con él como un servicio local.

### Por qué existe esto

| Preocupación | Respuesta del navegador | Respuesta de impresión silenciosa |
|---|---|---|
| Sitios maliciosos que imprimen spam | Bloquear acceso silencioso al dispositivo | El usuario instala un agente conocido |
| Elegir la impresora equivocada | Forzar un diálogo | Agente + API de impresora nombrada |
| ESC/POS / ZPL raw | No disponible desde la página | El agente o SDK del fabricante envía bytes |
| Trabajos por lotes / desatendidos | El diálogo rompe el flujo | Cola en el agente o servidor |

## Cinco arquitecturas

| Enfoque | Quién instala qué | Uso típico | Compromiso |
|---|---|---|---|
| Puente de impresión local | Agente de escritorio + SDK JS | ERP, WMS, POS, etiquetas | Mejor camino general para web silenciosa |
| Extensión + host nativo | Extensión del navegador + app host | Flotas controladas | Revisión de tienda + fricción de confianza |
| Política empresarial / quiosco | Imagen de navegador gestionada | Solo quioscos | No para usuarios SaaS públicos |
| SDK del fabricante de hardware | Servicio del fabricante o API de impresora en red | Flotas Zebra / Epson / Star | Dependencia del hardware |
| Shell de escritorio (Electron…) | Tu propia app de escritorio | Cuando controlas el cliente | No es una app puramente de navegador |

La mayoría de los hilos sobre «impresión silenciosa desde Chrome» en internet acaban en **puente de impresión local** o **SDK del fabricante**.

## Qué ocurre en un trabajo de impresión

Un trabajo típico de puente HTML/PDF:

1. La SPA construye HTML (o una URL / bytes de PDF).
2. El SDK abre `https://tusitio` → `ws://127.0.0.1:puerto` (o HTTP).
3. El agente acepta el trabajo y, opcionalmente, renderiza HTML en un Chromium embebido.
4. El agente envía a la cola del SO o abre un puerto TCP/USB raw.
5. El SDK devuelve éxito / fallo a la página.

Un trabajo POS raw típico omite el render HTML del paso 3 y envía ESC/POS o ZPL directamente.

## Modos de fallo que encontrarás en producción

| Síntoma | Causa probable | Dónde mirar |
|---|---|---|
| Funciona en portátil, falla tras el despliegue | Sitio HTTP bloqueado desde loopback | [Chrome Local Network Access](chrome-local-network-access.es.md) |
| Conexión OK, no imprime nada | Nombre de impresora incorrecto / cola offline | Consultar impresoras; revisar cola del SO |
| Diseño distinto a la pantalla | Motor diferente / fuentes faltantes | Fijar fuentes; probar en el SO destino |
| Primer trabajo lento, luego bien | Arranque en frío del agente / aviso de certificado | Preflight de conexión al iniciar sesión |
| Caídas aleatorias en PCs POS | Suspensión, antivirus, conflicto de puerto | Mantener agente como servicio; reservar puerto |

## Por qué importa localhost

Los puentes locales suelen escuchar en `127.0.0.1`. Los navegadores Chromium modernos también aplican reglas de **Local Network Access**: una página pública/de producción suele necesitar **HTTPS** antes de poder abrir `ws://127.0.0.1…`. El desarrollo en `http://localhost` es un contexto de seguridad distinto, por eso «funcionaba en mi máquina» es tan habitual.

## Qué no es la impresión silenciosa

- Generar un PDF para descargar (jsPDF, html2pdf…) — útil, pero no es imprimir
- Abrir el diálogo de impresión del sistema (Print.js, `window.print()`)
- Solo renderizado PDF en servidor (Puppeteer) sin camino a una impresora **local**
- «Silencioso» solo dentro de Electron mientras sigues usando `window.print()` en la build de navegador

## Línea base de seguridad

Trata el agente local como software privilegiado:

- Prefiere canales autenticados / firmados donde el fabricante lo soporte
- No expongas el puerto del agente a la LAN ni a internet
- Delimita impresoras y plantillas; evita «imprimir cualquier URL» desde entrada no confiable
- Registra job id → usuario/estación → impresora → resultado para auditorías

## Siguiente paso

- [Elegir un stack de impresión silenciosa](choose-silent-print-stack.es.md)
- [window.print vs impresión silenciosa](window-print-vs-silent-print.es.md)
- [Chrome Local Network Access y 127.0.0.1](chrome-local-network-access.es.md)
- Herramientas curadas: [Awesome Silent Printing](../README.es.md)
