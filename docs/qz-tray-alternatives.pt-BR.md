# Alternativas ao QZ Tray

Buscar uma **alternativa ao QZ Tray** costuma significar que você quer impressão silenciosa a partir do navegador, mas com outro trade-off em estilo de API, preço, assinatura ou fluxo HTML/CSS.

## Quando ficar no QZ Tray

- ESC/POS / ZPL raw é a carga principal
- Você já investiu em assinatura e certificados QZ
- Precisa de histórico longo em PDV global
- Seu time já envolve chamadas WebSocket QZ em produção

## Quando avaliar alternativas

| Necessidade | Veja |
|---|---|
| Client JS comercial com tipos de arquivo amplos | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| HTML/CSS estilo npm a partir de SPA | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) e outras pontes HTML-capable |
| API cloud para muitas impressoras | [PrintNode](https://www.printnode.com/en) |
| Templates liderados por designer | hiprint + electron-hiprint |
| Marca única de hardware | SDKs Zebra / Epson / Star |

## Notas de migração

1. Inventarie quais jobs são raw vs HTML/PDF — jobs raw são a reescrita cara.
2. Re-teste suposições de assinatura / licença; não assuma que o próximo fabricante é “silencioso sem assinar”.
3. Mantenha uma mesa QZ ativa enquanto pilota a alternativa.
4. Re-valide Chrome Local Network Access na nova porta do agente.

## Compare diretamente

- [QZ Tray vs web-print-pdf vs JSPrintManager](qz-vs-jspm-vs-web-print-pdf.pt-BR.md)
- [Como escolher uma stack de impressão silenciosa](choose-silent-print-stack.pt-BR.md)
