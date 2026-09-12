# Como funciona a impressão silenciosa

Os navegadores foram feitos para **impedir** que uma página envie trabalhos em silêncio para uma impressora física. Por isso o `window.print()` abre um diálogo, e “impressão silenciosa com JavaScript puro” costuma ser um beco sem saída.

Se você está construindo cupons, etiquetas de envio, faturas ou tickets de cozinha a partir de um app web, precisa de uma arquitetura diferente — não de uma flag do navegador.

## O padrão real

Quase toda configuração de impressão silenciosa em produção se parece com isto:

```text
App web (navegador)
    → HTTP / WebSocket / Native Messaging em localhost
    → agente local / serviço do fabricante / shell desktop
    → spooler do SO ou porta raw do dispositivo
```

A página nunca “possui” a impressora. Um **componente local confiável** possui. Esse componente é instalado uma vez por estação (ou já vem na imagem do quiosque), e o app web conversa com ele como um serviço local.

### Por que isso existe

| Preocupação | Resposta do navegador | Resposta da impressão silenciosa |
|---|---|---|
| Sites maliciosos imprimindo spam | Bloquear acesso silencioso ao dispositivo | Usuário instala um agente conhecido |
| Escolher a impressora errada | Forçar um diálogo | Agente + API de impressora nomeada |
| ESC/POS / ZPL raw | Indisponível na página | Agente ou SDK do fabricante envia bytes |
| Jobs em lote / sem atendimento | Diálogo quebra o fluxo | Fila no agente ou no servidor |

## Cinco arquiteturas

| Abordagem | Quem instala o quê | Uso típico | Trade-off |
|---|---|---|---|
| Ponte de impressão local | Agente desktop + SDK JS | ERP, WMS, PDV, etiquetas | Melhor caminho web geral para impressão silenciosa |
| Extensão + host nativo | Extensão do navegador + app host | Frotas controladas | Revisão da loja + atrito de confiança |
| Política enterprise / quiosque | Imagem gerenciada do navegador | Só quiosques | Não serve para usuários SaaS públicos |
| SDK do fabricante de hardware | Serviço do fabricante ou API de impressora em rede | Frotas Zebra / Epson / Star | Lock-in de hardware |
| Shell desktop (Electron…) | Seu próprio app desktop | Quando você controla o cliente | Não é um app puramente no navegador |

A maioria das threads sobre “impressão silenciosa no Chrome” acaba em **ponte de impressão local** ou **SDK do fabricante**.

## O que acontece em um job de impressão

Um job típico de ponte HTML/PDF:

1. A SPA monta HTML (ou URL / bytes de PDF).
2. O SDK abre `https://seusite` → `ws://127.0.0.1:porta` (ou HTTP).
3. O agente aceita o job, opcionalmente renderiza HTML em um Chromium embutido.
4. O agente envia ao spooler do SO ou abre uma porta TCP/USB raw.
5. O SDK devolve sucesso / falha para a página.

Um job POS raw típico pula a renderização HTML do passo 3 e envia ESC/POS ou ZPL diretamente.

## Modos de falha que você verá em produção

| Sintoma | Causa provável | Onde olhar |
|---|---|---|
| Funciona no notebook, falha após deploy | Site HTTP bloqueado do loopback | [Chrome Local Network Access](chrome-local-network-access.pt-BR.md) |
| Conecta OK, nada imprime | Nome de impressora errado / fila offline | Consultar impressoras; checar spooler do SO |
| Layout diferente da tela | Motor diferente / fontes faltando | Fixar fontes; testar no SO alvo |
| Primeiro job lento, depois ok | Cold start do agente / prompt de certificado | Preflight de conexão no login |
| Quedas aleatórias em PCs de PDV | Sleep, antivírus, conflito de porta | Manter agente como serviço; reservar porta |

## Por que localhost importa

Pontes locais costumam escutar em `127.0.0.1`. Navegadores Chromium modernos também aplicam regras de **Local Network Access**: uma página pública/de produção muitas vezes precisa de **HTTPS** antes de abrir `ws://127.0.0.1…`. Dev em `http://localhost` é outro contexto de segurança — daí o clássico “funcionou na minha máquina”.

## O que impressão silenciosa não é

- Gerar PDF para download (jsPDF, html2pdf…) — útil, mas não é impressão
- Abrir o diálogo de impressão do sistema (Print.js, `window.print()`)
- Renderização server-side de PDF sozinha (Puppeteer) sem caminho até uma impressora **local**
- “Silencioso” só dentro do Electron enquanto o build de navegador ainda usa `window.print()`

## Linha de base de segurança

Trate o agente local como software privilegiado:

- Prefira canais autenticados / assinados quando o fabricante suportar
- Não exponha a porta do agente na LAN ou na internet
- Limite impressoras e templates; evite “imprimir qualquer URL” a partir de input não confiável
- Registre job id → usuário/estação → impressora → resultado para auditoria

## Próximos passos

- [Como escolher uma stack de impressão silenciosa](choose-silent-print-stack.pt-BR.md)
- [window.print vs impressão silenciosa](window-print-vs-silent-print.pt-BR.md)
- [Chrome Local Network Access e 127.0.0.1](chrome-local-network-access.pt-BR.md)
- Ferramentas curadas: [Awesome Silent Printing](../README.pt-BR.md)
