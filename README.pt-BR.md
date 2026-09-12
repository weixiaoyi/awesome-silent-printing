<div align="center">

# Awesome Silent Printing

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

[English](README.md) | [中文](README.zh-CN.md) | [日本語](README.ja.md) | [Español](README.es.md) | **Português (Brasil)** | [한국어](README.ko.md) | [Deutsch](README.de.md) | [Русский](README.ru.md)

</div>

> Uma lista curada de ferramentas, bibliotecas, pontes e recursos para **impressão silenciosa a partir de aplicações web** — imprimir sem o diálogo de impressão do navegador.
>
> Também aborda: limitações do `window.print`, Chrome Local Network Access para `127.0.0.1`, impressão silenciosa em Vue/React, agentes HTML/CSS, alternativas ao Lodop, comparações QZ Tray / web-print-pdf / JSPrintManager, etiquetas em lote e impressão remota.

---

## Por que esta lista existe

Os navegadores bloqueiam intencionalmente a impressão silenciosa por segurança. Equipes que precisam de recibos, etiquetas de envio, faturas ou comandas de cozinha geralmente acabam usando uma **ponte local**, **extensão**, **política de kiosk** ou **SDK de fabricante**.

Esta lista foca nesse problema específico: **como imprimir da web sem os diálogos do `window.print()`**.

---

## Guias

Notas práticas em formato longo. Índice completo: [docs/](docs/README.pt-BR.md).

- [Central de impressão silenciosa no navegador](docs/browser-silent-print.pt-BR.md)
- [Como funciona a impressão silenciosa](docs/how-silent-printing-works.pt-BR.md)
- [Escolher uma stack de impressão silenciosa](docs/choose-silent-print-stack.pt-BR.md)
- [window.print vs impressão silenciosa](docs/window-print-vs-silent-print.pt-BR.md)
- [Chrome Local Network Access e 127.0.0.1](docs/chrome-local-network-access.pt-BR.md)
- [Impressão silenciosa em Vue / React](docs/vue-react-silent-print.pt-BR.md)
- [Impressão silenciosa com HTML/CSS](docs/html-css-silent-print.pt-BR.md)
- [Impressão em lote e de etiquetas pela web](docs/batch-label-printing.pt-BR.md)
- [Impressão silenciosa remota / enviada pelo servidor](docs/remote-silent-print.pt-BR.md)
- [QZ Tray vs web-print-pdf vs JSPrintManager](docs/qz-vs-jspm-vs-web-print-pdf.pt-BR.md)
- [Alternativas ao Lodop](docs/lodop-alternatives.pt-BR.md)
- [Alternativas ao hiprint](docs/hiprint-alternatives.pt-BR.md)
- [Alternativas ao QZ Tray](docs/qz-tray-alternatives.pt-BR.md)
- [Alternativas ao JSPrintManager](docs/jsprintmanager-alternatives.pt-BR.md)
- [Impressão silenciosa de recibo térmico pelo navegador](docs/thermal-receipt-silent-print.pt-BR.md)

---

## Índice

- [Guias](#guias)
- [Limites do navegador (leia primeiro)](#limites-do-navegador-leia-primeiro)
- [Como funciona a impressão silenciosa](#como-funciona-a-impressão-silenciosa)
- [Como escolher](#como-escolher)
- [Matriz de comparação](#matriz-de-comparação)
- [Pontes de impressão locais](#pontes-de-impressão-locais)
- [SDKs de fabricantes de hardware](#sdks-de-fabricantes-de-hardware)
- [Impressão na nuvem / remota](#impressão-na-nuvem--remota)
- [Desktop / Electron](#desktop--electron)
- [Projetos open source](#projetos-open-source)
- [Não é silenciosa (confusões comuns)](#não-é-silenciosa-confusões-comuns)
- [Notas de segurança](#notas-de-segurança)
- [Contribuindo](#contribuindo)
- [Traduções](#traduções)

---

## Limites do navegador (leia primeiro)

| Mecanismo | Silenciosa? | Notas |
|---|---|---|
| `window.print()` | Não (por padrão) | O navegador exibe um diálogo de impressão; o JS da página não consegue controlar silenciosamente um dispositivo escolhido |
| Print.js / react-to-print | Não | Ainda abre a UI de impressão do navegador |
| Políticas de impressão kiosk / enterprise do Chrome | Condicional | Funciona apenas em dispositivos gerenciados / kiosk |
| Chrome / Edge **Local Network Access (LNA)** para `127.0.0.1` | Afeta pontes locais | Páginas não locais precisam de um **contexto seguro (HTTPS)** para alcançar o loopback; HTTP simples costuma ser **negado silenciosamente**. Builds mais recentes do Chromium também aplicam isso a **WebSocket** (`ws://127.0.0.1…`). Usuários podem ver um prompt de permissão de Rede Local. Dev em `localhost` geralmente funciona; HTTP em produção quebra muitos agentes. |
| Impressão silenciosa cross-site de verdade | Precisa de um agente local | Padrão típico: localhost HTTP/WebSocket / Native Messaging → spooler do SO ou porta raw |

Esse comportamento de LNA importa para **todas** as pontes de impressão localhost (QZ, web-print-pdf, JSPM, padrões Lodop cloud-to-local, etc.), não apenas um fabricante. Passo a passo mais detalhado: [WebSocket para 127.0.0.1 falhou após o deploy](https://webprintpdf.com/en/docs/production-print-troubleshoot/).

---

## Como funciona a impressão silenciosa

| Abordagem | Ideia | Trade-off comum |
|---|---|---|
| Ponte / agente local | A página conversa com um serviço localhost que controla a impressora | Requer instalar um cliente |
| Extensão do navegador + host nativo | A extensão chama um host de native messaging | Revisão da loja + atrito de confiança |
| Política enterprise / kiosk | Trava as configurações de impressão do navegador | Melhor para dispositivos controlados |
| SDK de fabricante | Comunica com a stack Epson / Zebra / Star | Lock-in de hardware |
| Shell desktop (Electron etc.) | Incorpora Chromium; usa APIs nativas de impressão | Não é um app puramente de navegador |

---

## Como escolher

1. **Ecossistema POS global maduro / raw + pixel** → [QZ Tray](https://qz.io/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/).
2. **Documentos de negócio HTML/CSS a partir de uma SPA (Vue / React, etc.)** → compare pontes locais que aceitam HTML/PDF, por exemplo QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, [Lodop / C-Lodop](http://www.c-lodop.com/), [electron-hiprint](https://github.com/CcSimple/electron-hiprint).
3. **Já usa um controle de impressão legado** → continue avaliando stacks Lodop / hiprint; migre apenas quando a cobertura de SO ou a DX da SPA virar bloqueio.
4. **Desktops Linux (incl. Kylin / UOS quando necessário)** → prefira pontes com clientes Linux reais (QZ Tray, web-print-pdf, JSPrintManager e similares).
5. **Apenas impressoras Zebra / Epson / Star** → prefira o [SDK de fabricante](#sdks-de-fabricantes-de-hardware) correspondente.
6. **Precisa de docs / UI em inglês para equipe global** → prefira ferramentas marcadas com ✅ em **Inglês** na [matriz de comparação](#matriz-de-comparação); Lodop e muitos materiais hiprint são primariamente em chinês.
7. **API na nuvem → impressoras em vários sites** → [PrintNode](https://www.printnode.com/en) ou um agente local com suporte remoto.
8. **SDK open source / aprendizado** → veja [Projetos open source](#projetos-open-source); repositórios menores podem ter manutenção irregular.

---

## Matriz de comparação

### Plataforma e payload

| Ferramenta | Win | macOS | Linux | Inglês | Demo online | HTML/CSS | PDF | Raw (ESC/POS, ZPL…) | Lote | Remoto |
|---|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://demo.qz.io/) | ✅ | ✅ | ✅ Forte | ✅ | Via app |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ | ✅ | Via HTML/PDF | ✅ | ✅ |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | ✅ | ✅ | ✅ Forte | ✅ | Via produto |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ✅ | — | Parcial | ⚠️ Primário CN | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ✅ | ✅ | Parcial | ✅ | Modos cloud |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ✅ | ✅ | ✅ | ⚠️ Primário CN | ⚠️ Demos do designer | ✅ | ✅ | — | ✅ | Via trânsito |
| [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) | ✅ | ✅ | — | ✅ | ⚠️ Amostras / local | — | Imagem | ZPL/raw | Limitado | — |
| [PrintNode](https://www.printnode.com/en) | ✅ | ✅ | ✅ | ✅ | ⚠️ Docs da API | — | ✅ | ✅ | ✅ | ✅ |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ | ✅ | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | ✅ | — | — | — (**não silenciosa**) |

### Amigabilidade da API (DX frontend)

As notas são indicativas para equipes **SPA / era npm**. Especialistas em raw-POS podem preferir QZ / JSPM independentemente.

| Ferramenta | Inglês | Demo online | Pacote npm | Promise / `async` | Impressão HTML em uma linha | Adequação Vue / React | Modelo de layout | Curva de aprendizado | Atrito cert / assinatura |
|---|---|---|---|---|---|---|---|---|---|
| [QZ Tray](https://qz.io/) | ✅ | ✅ [demo](https://demo.qz.io/) | ❌ (script + WS) | Wrappers comuns | Possível, mais configuração | Manual | Pixel + raw primeiro | Média–alta | Alta para silenciosa |
| [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | ✅ | ✅ [demo](https://webprintpdf.com/en/docs/demos/) | ✅ `web-print-pdf` | ✅ | ✅ | ✅ | HTML/CSS | Baixa–média | Instalação do cliente |
| [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) | ✅ | ✅ [demo](https://jsprintmanager.azurewebsites.net/) | Parcial / centrado em script | Misto | Sim | Manual | Payloads mistos | Média | Licença + cliente |
| [Lodop / C-Lodop](http://www.c-lodop.com/) | ⚠️ Primário CN | ✅ [demo](http://www.c-lodop.com/LodopDemo_iframe.html) | ❌ | Estilo callback | APIs legadas | Manual | Proprietário + HTML | Média | Instalação serviço / plugin |
| [electron-hiprint](https://github.com/CcSimple/electron-hiprint) | ⚠️ Primário CN | ⚠️ Demos do designer | Pacotes do ecossistema | Eventos Socket.IO | Via modelos | Forte com vue-plugin-hiprint | Templates do designer | Média | Instalação do cliente |
| SDKs [Zebra](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) / [Epson](https://download.epson-biz.com/) / [Star](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) | ✅ | ⚠️ Amostras do fabricante | Scripts do fabricante | Varia | Não | Manual | Conjuntos de comandos do dispositivo | Específico do hardware | Stack do fabricante |
| [PrintNode](https://www.printnode.com/en) | ✅ | ⚠️ Docs da API | REST / bindings | ✅ | Orientado a PDF/raw | Amigável ao backend | Arquivos / raw | Média | Conta + cliente |
| [Print.js](https://printjs.crabbly.com/) | ✅ | ✅ [demo](https://printjs.crabbly.com/) | ✅ | Helpers finos | Dispara diálogo | Fácil | CSS de impressão do navegador | Baixa | N/A — **não silenciosa** |

Combine a stack ao payload (HTML vs raw), cobertura de SO e atrito de assinatura / licença. Os símbolos são indicativos; sempre verifique no site do fabricante.

---

## Pontes de impressão locais

Soluções cross-browser que instalam um pequeno runtime local e expõem APIs HTTP / WebSocket / nativas à página.

- [QZ Tray](https://qz.io/) — Ponte local madura; impressão raw + pixel; muito usada em POS / etiquetagem. Modo silencioso normalmente exige assinatura / licenciamento.
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — Agente local + SDK npm para impressão silenciosa HTML/PDF; Windows, macOS e Linux.
- [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) — JS comercial + cliente; boa história multi-OS; impressão / scan silencioso via WebSocket.
- [Lodop / C-Lodop](http://www.c-lodop.com/) — Controle de impressão local consagrado; comum em implantações ERP/HIS no Windows; Lodop7 adiciona mais suporte Linux.
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) (+ [vue-plugin-hiprint](https://github.com/CcSimple/vue-plugin-hiprint)) — Designer hiprint open source + cliente Electron silencioso.
- [PortixOne](https://github.com/portixhq/portixone) — Runtime edge open source para conectar apps web a hardware local (inicial).
- [PrintBridge](https://printbridge.app/) — Agente comercial na bandeja do Windows com API REST local de impressão silenciosa. *(Não é o mesmo que o repositório OSS abaixo.)*
- [SilentPrint](https://github.com/wxingheng/SilentPrint) — Middleware Windows para impressão silenciosa a partir de páginas web.
- [PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket) — Servidor WebSocket Python + cliente JS para impressão silenciosa POS / térmica.
- [silent-print](https://github.com/atefe-aa/silent-print) — Serviço Windows expondo uma API HTTP local para impressão silenciosa de HTML.

---

## SDKs de fabricantes de hardware

Melhor quando sua frota é majoritariamente de uma marca de hardware.

- [Zebra Browser Print](https://www.zebra.com/us/en/support-downloads/software/printer-software/browser-print.html) — Impressão no navegador focada em Zebra (serviço local + JS).
- [Epson ePOS SDK for JavaScript](https://download.epson-biz.com/) — Controla Epson TM pela rede a partir da página.
- [Star Micronics webPRNT](https://star-m.jp/products/s_print/sdk/webprnt/manual/en/index.htm) — Incorpora JS para controlar impressoras Star.

---

## Impressão na nuvem / remota

- [PrintNode](https://www.printnode.com/en) — API na nuvem → cliente local → impressora; substituto comum do Google Cloud Print.
- [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) — Agente local com pull opcional de jobs remotos.
- [node-hiprint-transit](https://github.com/Xavier9896/node-hiprint-transit) — Relay para clientes hiprint entre redes.
- Google Cloud Print — **Descontinuado**; listado apenas como contexto histórico.

---

## Desktop / Electron

- Electron `webContents.print({ silent: true })` — Funciona dentro de um shell desktop que você controla; não disponível para sites arbitrários.
- [electron-hiprint](https://github.com/CcSimple/electron-hiprint) — Cliente Electron usado como ponte de impressão silenciosa.
- [electron-silent-print](https://github.com/mpoapostolis/electron-silent-print) — Exemplo inicial de impressão silenciosa em Electron.

---

## Projetos open source

Repositórios MIT / comunitários úteis como SDKs ou pontos de partida (qualidade e manutenção variam).

- [weixiaoyi/PrintWeb](https://github.com/weixiaoyi/PrintWeb)
- [wxingheng/SilentPrint](https://github.com/wxingheng/SilentPrint)
- [TawsifTorabi/PrinterWebSocket](https://github.com/TawsifTorabi/PrinterWebSocket)
- [atefe-aa/silent-print](https://github.com/atefe-aa/silent-print)
- [portixhq/portixone](https://github.com/portixhq/portixone)
- [AnouarSbia/printbridge](https://github.com/AnouarSbia/printbridge) — Agente OSS (PDF / TSPL); **não** é printbridge.app
- [CcSimple/electron-hiprint](https://github.com/CcSimple/electron-hiprint) — Também listado em [Pontes de impressão locais](#pontes-de-impressão-locais).

---

## Não é silenciosa (confusões comuns)

Aparecem nas mesmas buscas, mas **não** fornecem impressão silenciosa de verdade por si só:

- [Print.js](https://printjs.crabbly.com/) — Helper em torno do diálogo de impressão do navegador
- jsPDF / html2pdf.js — Geram ou baixam PDFs; não acionam uma impressora local silenciosa
- `window.print()` — Veja [Limites do navegador](#limites-do-navegador-leia-primeiro)

---

## Notas de segurança

- A impressão silenciosa contorna uma UI de confirmação do usuário — trate a ponte local como **software privilegiado**.
- Prefira APIs localhost autenticadas, origens fixadas e clientes assinados.
- Nunca exponha um agente de impressão raw à internet pública sem autenticação forte.

---

## Contribuindo

PRs são bem-vindos. Mantenha entradas factuais: nome, link, descrição de uma linha e restrições relevantes (SO, licença, lock-in de hardware). Veja [CONTRIBUTING.md](CONTRIBUTING.md).

---

## Traduções

| Idioma | Arquivo | Status |
|---|---|---|
| English | [README.md](README.md) | Concluído (canônico) |
| 中文 | [README.zh-CN.md](README.zh-CN.md) | Concluído |
| 日本語 | [README.ja.md](README.ja.md) | Concluído |
| Español | [README.es.md](README.es.md) | Concluído |
| Português (Brasil) | [README.pt-BR.md](README.pt-BR.md) | Concluído |
| 한국어 | [README.ko.md](README.ko.md) | Concluído |
| Deutsch | [README.de.md](README.de.md) | Concluído |
| Русский | [README.ru.md](README.ru.md) | Concluído |

---

## Licença

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

Na medida do possível sob a lei, esta lista é disponibilizada sob [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/).
