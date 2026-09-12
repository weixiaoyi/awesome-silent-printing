# Alternativas ao Lodop

Lodop / C-Lodop ainda é comum em muitos sistemas de negócio Windows. Times costumam buscar alternativas quando precisam de:

- Integração SPA moderna
- Suporte desktop mais amplo (mesas macOS / Linux)
- Docs em inglês para times mistos
- Comportamento mais claro de HTTPS + localhost nas regras Chromium atuais

## Direções de substituição

| Necessidade | Candidatos |
|---|---|
| Docs de negócio HTML/CSS | QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, hiprint + electron-hiprint |
| PDV / etiquetas raw | [QZ Tray](https://qz.io/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| Cloud para muitas impressoras | [PrintNode](https://www.printnode.com/en) |
| Ficar no Lodop | Lodop7 / C-Lodop se a stack ainda encaixa |

## Por que migrações travam

- Templates misturam comandos proprietários Lodop e fragmentos HTML
- Nomes de impressora e bandejas de papel estão codificados em scripts antigos
- Hospitais / ERPs temem mudar um caminho de impressão que funciona na alta temporada

## Dicas de migração

1. Inventarie templates: HTML vs comandos proprietários. Conte quantos já são “só HTML”.
2. Reconstrua docs críticos como HTML/CSS quando possível; deixe raw exótico para uma segunda onda.
3. Rode agentes antigo e novo em paralelo em uma mesa piloto (portas diferentes).
4. Corrija HTTPS / Local Network Access antes do rollout nacional.
5. Treine helpdesk no novo sintoma “agente não está rodando” — substitui erros da era ActiveX.

## Relacionados

- [Como escolher uma stack](choose-silent-print-stack.pt-BR.md)
- [window.print vs impressão silenciosa](window-print-vs-silent-print.pt-BR.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.pt-BR.md)
