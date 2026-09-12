# Chrome Local Network Access e 127.0.0.1

## Sintoma

Dev funciona. Produção falha com algo como:

```text
WebSocket connection to 'ws://127.0.0.1:…' failed
Failed to connect to print agent
ERR_CONNECTION_REFUSED / net::ERR_FAILED
```

O cliente de impressão local está rodando. Firewall parece ok. Só o **site publicado** não alcança o loopback.

Esse é um dos incidentes mais comuns de “impressão silenciosa quebrou após go-live” em qualquer ponte localhost.

## Causa

O **Local Network Access (LNA)** do Chromium restringe páginas públicas de conversar com a rede local / loopback do usuário. Agentes de impressão que escutam em `127.0.0.1` ficam exatamente nessa zona restrita.

Regras práticas que os times encontram no campo:

| Contexto | Resultado típico |
|---|---|
| App em `http://localhost` no dev | Costuma funcionar (contexto especial) |
| App em **HTTP** simples em produção | Muitas vezes **negado em silêncio** |
| App em **HTTPS** com certificado confiável | Pode pedir permissão de Local Network |
| Chrome mais novo + WebSocket para loopback | Mesmas regras LNA para `ws://127.0.0.1…` |
| Edge / outros Chromium | Políticas parecidas |

Então o agente “de pé” é necessário, mas não suficiente. A **origem do navegador** precisa estar autorizada a falar com o loopback.

## Fluxograma de decisão

```text
A página alcança ws://127.0.0.1 / http://127.0.0.1?
│
├─ Não, e o site é HTTP
│     → Coloque o site em HTTPS primeiro. Pare aqui até publicar.
│
├─ Não, e o site é HTTPS
│     → Verifique permissão / prompt de Local Network
│     → Confirme porta e processo do agente
│     → Teste na mesma máquina com um client WS mínimo
│
└─ Sim, mas a impressão ainda falha
      → Nome da impressora, driver, spooler, template — não é LNA
```

## O que fazer (checklist)

1. **Sirva o app web em HTTPS** com certificado que a estação confia (CA pública ou PKI interna). Certificados autoassinados que o usuário não confiou continuam falhando.
2. Na primeira impressão, observe o prompt de **Local Network** do navegador e permita para sua origem.
3. Confirme que o agente desktop escuta em `127.0.0.1` (e na porta que seu SDK espera).
4. Reproduza com DevTools → Network: a chamada WS/HTTP foi bloqueada, recusada ou resetada?
5. Para **frotas gerenciadas apenas**: flags / política enterprise podem relaxar checagens. Não é estratégia para usuários finais SaaS públicos.
6. Documente o passo de permissão no runbook de instalação; senão o helpdesk reinstala o agente para sempre.

## Como distinguir LNA de “agente parado”

| Checagem | Agente parado | LNA / problema de origem |
|---|---|---|
| `127.0.0.1:porta` de uma ferramenta local | Falha | Sucesso |
| Mesma máquina, site em HTTP | Pode falhar | Muitas vezes falha |
| Mesma máquina, site HTTPS + permissão | Funciona se agente up | Funciona |
| Perfil diferente no Chrome | Igual | Permissão pode faltar |

## Quem é afetado

Qualquer ponte localhost: QZ Tray, web-print-pdf, JSPrintManager, serviços locais estilo Lodop, agentes customizados. Não é bug de um único fabricante.

## Walkthrough mais detalhado

- English: [WebSocket to 127.0.0.1 failed after deploy](https://webprintpdf.com/en/docs/production-print-troubleshoot/)
- 中文: [上线后连接 127.0.0.1 失败](https://webprintpdf.com/docs/production-print-troubleshoot/)

## Relacionados

- [Como funciona a impressão silenciosa](how-silent-printing-works.pt-BR.md)
- [Impressão silenciosa em Vue / React](vue-react-silent-print.pt-BR.md)
- [Como escolher uma stack](choose-silent-print-stack.pt-BR.md)
