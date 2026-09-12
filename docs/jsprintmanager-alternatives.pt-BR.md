# Alternativas ao JSPrintManager

Uma **alternativa ao JSPrintManager** muitas vezes é necessária por preço, estilo de API ou outro fluxo HTML/CSS mantendo impressão silenciosa.

## Fique se

- Você depende do amplo conjunto de features de arquivo/impressão/scan do JSPM
- Suporte comercial e cobertura multi-SO do client importam mais
- Procurement já padronizou licenciamento Neodynamic

## Alternativas por necessidade

| Necessidade | Opções |
|---|---|
| Ecossistema POS raw-heavy | [QZ Tray](https://qz.io/) |
| HTML/CSS estilo npm a partir de SPA | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) e outras pontes HTML-capable |
| Roteamento cloud de impressão | [PrintNode](https://www.printnode.com/en) |
| Designer open + client Electron | hiprint + electron-hiprint |
| Marca única de hardware | SDKs Zebra / Epson / Star |

## Notas de migração

1. Liste quais APIs JSPM você realmente chama (impressão vs scan vs tipos de arquivo).
2. Mapeie cada uma para a API mais próxima do candidato — espere código de cola.
3. Re-orçamente licença + suporte; “SDK mais barato” pode perder em horas de helpdesk.
4. Pilote em uma estação com antivírus ligado; agentes comerciais costumam disparar alertas uma vez.

## Relacionados

- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.pt-BR.md)
- [Como escolher uma stack de impressão silenciosa](choose-silent-print-stack.pt-BR.md)
- [Impressão silenciosa em Vue / React](vue-react-silent-print.pt-BR.md)
