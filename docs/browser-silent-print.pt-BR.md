# Impressão silenciosa no navegador

**Impressão silenciosa no navegador** significa que uma página web envia um trabalho para a impressora local **sem** abrir a caixa de diálogo de impressão do navegador.

Este hub cobre os caminhos mais comuns na prática:

- impressão silenciosa a partir de um app web
- imprimir sem diálogo no Chrome
- impressão silenciosa em Vue / React
- alternativas ao `window.print()`

## Resposta rápida

Um site comum não consegue, sozinho, acionar silenciosamente uma impressora qualquer. Você precisa de um **agente local**, **SDK do fabricante**, **política de quiosque** ou **shell desktop**.

Se alguém disser que dá para “imprimir silenciosamente com JavaScript puro no Chrome em qualquer impressora”, pergunte qual componente local foi instalado. Esse componente é o caminho real até a impressora.

## Modelo mental em um minuto

```text
Página (HTTPS)
  → ponte localhost
  → spooler do SO ou porta raw
  → impressora física
```

Detalhes: [Como funciona a impressão silenciosa](how-silent-printing-works.pt-BR.md).

## Escolha a próxima página

| Sua situação | Leia |
|---|---|
| Precisa da arquitetura | [Como funciona a impressão silenciosa](how-silent-printing-works.pt-BR.md) |
| Precisa escolher uma stack | [Como escolher uma stack de impressão silenciosa](choose-silent-print-stack.pt-BR.md) |
| Vindo do `window.print` | [window.print vs impressão silenciosa](window-print-vs-silent-print.pt-BR.md) |
| Produção não alcança `127.0.0.1` | [Chrome Local Network Access](chrome-local-network-access.pt-BR.md) |
| Integração em SPA | [Impressão silenciosa em Vue / React](vue-react-silent-print.pt-BR.md) |
| Templates HTML | [Impressão silenciosa com HTML/CSS](html-css-silent-print.pt-BR.md) |
| Etiquetas / volume de armazém | [Impressão em lote e etiquetas](batch-label-printing.pt-BR.md) |
| WMS envia jobs para mesas | [Impressão silenciosa remota](remote-silent-print.pt-BR.md) |
| Cupom / cozinha | [Impressão silenciosa em térmica](thermal-receipt-silent-print.pt-BR.md) |
| Comparando pontes principais | [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.pt-BR.md) |

## Busca comum → guia

| As pessoas procuram | Comece aqui |
|---|---|
| browser silent print / webpage silent print | Esta página |
| window.print without dialog | [window.print vs impressão silenciosa](window-print-vs-silent-print.pt-BR.md) |
| Chrome websocket 127.0.0.1 failed | [Chrome LNA](chrome-local-network-access.pt-BR.md) |
| Alternativa a Lodop / hiprint / QZ | [Lodop](lodop-alternatives.pt-BR.md) · [hiprint](hiprint-alternatives.pt-BR.md) · [QZ](qz-tray-alternatives.pt-BR.md) · [JSPM](jsprintmanager-alternatives.pt-BR.md) |

## Lista de ferramentas

Veja a lista curada: [Awesome Silent Printing](../README.pt-BR.md).
