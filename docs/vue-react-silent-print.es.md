# Impresión silenciosa en Vue / React

## Objetivo

Llamar a impresión silenciosa desde una SPA sin abrir `window.print()`.

## Integración típica

1. El usuario instala un agente de impresión local una vez.
2. El frontend depende de un SDK npm o JS del fabricante (QZ, web-print-pdf, JSPM, etc.).
3. La página envía HTML, PDF o payload raw a `127.0.0.1`.
4. El agente renderiza / reenvía a la impresora del SO.

Forma de la llamada (las APIs varían según el fabricante):

```js
// Pseudocódigo — consulta la documentación del SDK de tu puente
await printAgent.printHtml(
  '<div class="label">Order #1001</div>',
  { printer: 'LabelPrinter' }
);
```

### Límite de módulo sugerido

Mantén la impresión detrás de un servicio pequeño para que los componentes Vue/React sigan siendo simples:

```js
// printService.js
export async function printLabel(html, printer) {
  await ensureAgent();
  return printAgent.printHtml(html, { printer });
}
```

- Llama a `ensureAgent()` al arrancar la app o antes de la primera pantalla de impresión.
- Muestra instrucciones de instalación cuando falte el agente.
- No llames a imprimir directamente desde diez componentes con diez objetos de opciones distintos.

## Trampas habituales en SPA

| Trampa | Solución |
|---|---|
| Llamar a imprimir antes de que el agente esté arriba | Preflight de conexión / mostrar guía de instalación |
| Nombres de impresora hardcodeados | Consultar lista de impresoras; persistir por estación |
| Origen HTTP en producción | Pasar a HTTPS para Local Network Access |
| Estilos distintos a la pantalla | Prefiere agentes HTML→impresión basados en Chromium; fija fuentes |
| Imprimir un árbol Vue/React vivo lleno de UI de la app | Renderiza una raíz de impresión dedicada / plantilla fuera de pantalla |
| Imágenes base64 enormes inline | Prefiere URLs que el agente pueda obtener, o comprime |
| Ignorar rechazos de Promise | Mapea errores a toast + reintento; registra job ids |

## Notas para Vue

- Pon plantillas en un SFC dedicado solo para imprimir (`LabelTicket.vue`), no en el layout completo de la página.
- Prefiere `ref` + `innerHTML` / `outerHTML` de una raíz de impresión montada, o construye cadenas HTML desde datos.
- Evita imprimir mientras un `<Transition>` o lista virtual está a mitad de actualización.

## Notas para React

- Misma idea: un componente `PrintTicket` renderizado en un contenedor oculto, luego serializa HTML.
- Cuidado con portales y renderizado concurrente — haz snapshot cuando los datos estén estables.
- No confíes en `window.print()` dentro de `useEffect` como camino «temporal» silencioso; entrena el hábito equivocado.

## Ciclo de vida de la conexión

```text
Inicio de app
  → ping al agente
  → si caído: banner + enlace de instalación
  → si arriba: cachear lista de impresoras
Usuario pulsa Imprimir
  → re-ping (barato)
  → enviar trabajo
  → mostrar resultado / código de error
```

## Relacionado

- [Impresión silenciosa con HTML/CSS](html-css-silent-print.es.md)
- [Chrome Local Network Access](chrome-local-network-access.es.md)
- [Elegir un stack](choose-silent-print-stack.es.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.es.md)
