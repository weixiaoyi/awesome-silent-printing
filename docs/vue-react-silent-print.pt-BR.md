# Impressão silenciosa em Vue / React

## Objetivo

Chamar impressão silenciosa a partir de uma SPA sem abrir `window.print()`.

## Integração típica

1. O usuário instala um agente de impressão local uma vez.
2. O frontend depende de um SDK npm ou JS do fabricante (QZ, web-print-pdf, JSPM, etc.).
3. A página envia HTML, PDF ou payload raw para `127.0.0.1`.
4. O agente renderiza / encaminha para a impressora do SO.

Formato da chamada (APIs variam por fabricante):

```js
// Pseudocódigo — consulte a documentação do SDK da sua ponte
await printAgent.printHtml(
  '<div class="label">Pedido #1001</div>',
  { printer: 'LabelPrinter' }
);
```

### Limite de módulo sugerido

Mantenha impressão atrás de um serviço pequeno para componentes Vue/React ficarem simples:

```js
// printService.js
export async function printLabel(html, printer) {
  await ensureAgent();
  return printAgent.printHtml(html, { printer });
}
```

- Chame `ensureAgent()` na inicialização do app ou antes da primeira tela de impressão.
- Mostre instruções de instalação quando o agente estiver ausente.
- Nunca chame impressão diretamente de dez componentes com dez objetos de opções diferentes.

## Armadilhas em SPA

| Armadilha | Correção |
|---|---|
| Chamar impressão antes do agente estar up | Preflight de conexão / orientação de instalação |
| Nomes de impressora hard-coded | Consultar lista; persistir por estação |
| Origem HTTP em produção | Migrar para HTTPS para Local Network Access |
| Estilo diferente da tela | Prefira agentes HTML→print baseados em Chromium; fixe fontes |
| Imprimir a árvore Vue/React viva com chrome de UI | Renderize um root de impressão dedicado / template offscreen |
| Imagens base64 enormes inline | Prefira URLs que o agente busca, ou comprima |
| Ignorar rejeições de Promise | Mapeie erros para toast + retry; registre job ids |

## Notas para Vue

- Coloque templates em um SFC dedicado só para impressão (`LabelTicket.vue`), não no layout completo da página.
- Prefira `ref` + `innerHTML` / `outerHTML` de um root de impressão montado, ou monte strings HTML a partir dos dados.
- Evite imprimir enquanto um `<Transition>` ou lista virtual está atualizando.

## Notas para React

- Mesma ideia: componente `PrintTicket` renderizado em container oculto, depois serialize HTML.
- Cuidado com portals e renderização concorrente — faça snapshot quando os dados estiverem estáveis.
- Não confie em `window.print()` dentro de `useEffect` como caminho “temporário” silencioso; isso cria o hábito errado.

## Ciclo de vida da conexão

```text
Início do app
  → ping no agente
  → se down: banner + link de instalação
  → se up: cache da lista de impressoras
Usuário clica Imprimir
  → re-ping (barato)
  → envia job
  → mostra resultado / código de erro
```

## Relacionados

- [Impressão silenciosa com HTML/CSS](html-css-silent-print.pt-BR.md)
- [Chrome Local Network Access](chrome-local-network-access.pt-BR.md)
- [Como escolher uma stack](choose-silent-print-stack.pt-BR.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.pt-BR.md)
