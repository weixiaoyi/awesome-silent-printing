# window.print vs impressão silenciosa

Muita gente pergunta: dá para chamar `window.print()` sem diálogo?  
Resposta curta: **não, em uma página web comum.**

Os navegadores tratam impressão como ação privilegiada do usuário. A UI de impressão é a válvula de segurança. Bibliotecas que envolvem `window.print()` (por exemplo Print.js ou react-to-print) ainda terminam nessa UI.

## Lado a lado

| | `window.print()` / Print.js | Ponte de impressão silenciosa |
|---|---|---|
| Diálogo | Sim | Não |
| Escolher impressora via JS | Limitado / não | Sim (via agente local) |
| Jobs em lote | Fraco | Pensado para filas |
| Impressora / bandeja / papel nomeados | Usuário decide | API do agente |
| Instalar algo | Não | Geralmente sim |
| Modelo de segurança | Controlado pelo navegador | Software local privilegiado |
| Funciona em clientes SaaS não gerenciados | Sim (com diálogo) | Precisa instalar agente |

## Mitos que desperdiçam sprints

| Mito | Realidade |
|---|---|
| “Deve ter uma flag do Chrome para usuários SaaS” | Flags/políticas são para dispositivos gerenciados, não visitantes públicos |
| “Puppeteer no servidor é impressão silenciosa” | Gera PDFs/imagens; não aciona a impressora USB/rede do usuário |
| “Download de PDF já resolve” | Usuário ainda imprime manualmente; sem lote / impressora nomeada |
| “APIs silenciosas do Electron funcionam no build de navegador” | Essas APIs existem só dentro do shell desktop que você entrega |

## Quando `window.print()` basta

- Impressão ocasional guiada pelo usuário (extratos que ele espera confirmar)
- Fluxos legais / médicos onde confirmar explicitamente é desejável
- Baixo volume, sem automação de quiosque / armazém
- Você não quer entregar instalação local

## Quando você precisa de impressão silenciosa

- Etiquetas de envio, cupons, tickets de cozinha
- Jobs sem atendimento ou de alta frequência
- Fluxos SPA onde o diálogo quebra a UX (escanear → imprimir → próximo)
- Mesas com várias impressoras (etiqueta vs A4 vs cupom) escolhidas no software

## Caminho de migração

1. **Inventarie** o que imprime hoje: screenshots HTML, CSS `@media print`, arquivos PDF ou raw.
2. **Mantenha templates HTML/CSS** quando a ponte alvo renderizar; reescreva só o que precisa virar ZPL/ESC/POS.
3. **Escolha um agente local** (ou SDK do fabricante / Electron) com [Como escolher uma stack de impressão silenciosa](choose-silent-print-stack.pt-BR.md).
4. Substitua chamadas de `window.print()` por uma chamada de SDK (HTML/PDF/raw via localhost).
5. Adicione UX de **agente não instalado**: detecte conexão, mostre link de instalação, bloqueie o botão “Imprimir” até estar pronto.
6. Publique o site em **HTTPS** e verifique [Local Network Access para `127.0.0.1`](chrome-local-network-access.pt-BR.md) em uma estação limpa.
7. Pilote uma mesa por uma semana com logging antes do rollout.

### Mudança mínima de forma de código

Antes:

```js
window.print();
```

Depois (pseudocódigo — APIs variam por fabricante):

```js
await printAgent.printHtml(document.getElementById('label').outerHTML, {
  printer: selectedPrinter,
});
```

## Testes de aceitação antes de dar por encerrado

- [ ] Diálogo nunca aparece no caminho feliz
- [ ] Impressora correta é selecionada sem cliques do usuário
- [ ] Um job ruim não trava o lote inteiro
- [ ] Perfil novo do navegador passa por HTTPS + permissão LNA uma vez
- [ ] Agente offline mostra erro recuperável, não trava

## Relacionados

- [Como funciona a impressão silenciosa](how-silent-printing-works.pt-BR.md)
- [Como escolher uma stack](choose-silent-print-stack.pt-BR.md)
- [Chrome Local Network Access](chrome-local-network-access.pt-BR.md)
- [Impressão silenciosa em Vue / React](vue-react-silent-print.pt-BR.md)
