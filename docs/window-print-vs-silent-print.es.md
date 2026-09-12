# window.print vs impresión silenciosa

Mucha gente pregunta: ¿puedo llamar a `window.print()` sin diálogo?  
Respuesta breve: **no en una página web normal del navegador.**

Los navegadores tratan la impresión como una acción privilegiada del usuario. La UI de impresión es la válvula de seguridad. Las bibliotecas que envuelven `window.print()` (por ejemplo Print.js o react-to-print) siguen terminando en esa UI.

## Lado a lado

| | `window.print()` / Print.js | Puente de impresión silenciosa |
|---|---|---|
| Diálogo | Sí | No |
| Elegir impresora desde JS | Limitado / no | Sí (vía agente local) |
| Trabajos por lotes | Pobre | Diseñado para colas |
| Impresora / bandeja / papel nombrados | Lo elige el usuario | API del agente |
| Instalar algo | No | Normalmente sí |
| Modelo de seguridad | Controlado por el navegador | Software local privilegiado |
| Funciona en clientes SaaS no gestionados | Sí (con diálogo) | Requiere instalar agente |

## Mitos que desperdician sprints

| Mito | Realidad |
|---|---|
| «Debe haber una bandera de Chrome para usuarios SaaS» | Banderas/políticas son para dispositivos gestionados, no visitantes públicos |
| «Puppeteer en el servidor es impresión silenciosa» | Genera PDFs/imágenes; no controla la impresora USB/red del usuario |
| «Descargar PDF es casi lo mismo» | El usuario sigue imprimiendo a mano; sin lote / impresora nombrada |
| «Las APIs silenciosas de Electron funcionan en la build de navegador» | Esas APIs existen solo dentro del shell de escritorio que distribuyes |

## Cuándo basta con `window.print()`

- Impresión ocasional dirigida por el usuario (extractos que el usuario espera confirmar)
- Flujos legales / médicos donde un confirm explícito es deseable
- Bajo volumen, sin automatización de quiosco / almacén
- Te niegas a distribuir una instalación local

## Cuándo necesitas impresión silenciosa

- Etiquetas de envío, recibos, tickets de cocina
- Trabajos desatendidos o de alta frecuencia
- Flujos SPA donde un diálogo rompe la UX (escanear → imprimir → siguiente)
- Escritorios multi-impresora (etiqueta vs A4 vs recibo) elegidos en software

## Camino de migración

1. **Inventaria** lo que imprimes hoy: capturas HTML, CSS `@media print`, archivos PDF o raw.
2. **Mantén plantillas HTML/CSS** cuando el puente destino pueda renderizarlas; reescribe solo lo que deba ser ZPL/ESC/POS.
3. **Elige un agente local** (o SDK del fabricante / Electron) con [Elegir un stack de impresión silenciosa](choose-silent-print-stack.es.md).
4. Sustituye llamadas a `window.print()` por una llamada al SDK (HTML/PDF/raw vía localhost).
5. Añade UX de **agente no instalado**: detecta conexión, muestra enlace de instalación, bloquea el botón «Imprimir» hasta que esté listo.
6. Despliega el sitio en **HTTPS** y verifica [Local Network Access a `127.0.0.1`](chrome-local-network-access.es.md) en una estación limpia.
7. Pilota un escritorio una semana con logging antes del despliegue general.

### Cambio mínimo de forma de código

Antes:

```js
window.print();
```

Después (pseudocódigo — las APIs varían según el fabricante):

```js
await printAgent.printHtml(document.getElementById('label').outerHTML, {
  printer: selectedPrinter,
});
```

## Pruebas de aceptación antes de darlo por hecho

- [ ] El diálogo nunca aparece en el camino feliz
- [ ] La impresora correcta se selecciona sin clics del usuario
- [ ] Un trabajo malo no congela todo el lote
- [ ] Un perfil de navegador nuevo pasa HTTPS + permiso LNA una vez
- [ ] Agente offline muestra un error recuperable, no un cuelgue

## Relacionado

- [Cómo funciona la impresión silenciosa](how-silent-printing-works.es.md)
- [Elegir un stack](choose-silent-print-stack.es.md)
- [Chrome Local Network Access](chrome-local-network-access.es.md)
- [Impresión silenciosa en Vue / React](vue-react-silent-print.es.md)
