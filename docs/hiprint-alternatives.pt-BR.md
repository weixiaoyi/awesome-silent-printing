# Alternativas ao hiprint

As pessoas buscam **alternativas ao hiprint** quando precisam de:

- Impressão silenciosa sem depender só do ecossistema designer do hiprint
- Clients desktop multi-SO mais robustos
- Outro estilo de integração SPA (npm / Promise, etc.)
- Documentação em inglês para times mistos

## Direções comuns

| Necessidade | Opções |
|---|---|
| Manter designer + client open-source | [vue-plugin-hiprint](https://github.com/CcSimple/vue-plugin-hiprint) + [electron-hiprint](https://github.com/CcSimple/electron-hiprint) |
| HTML/CSS a partir de SPA sem o designer | QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager |
| PDV / ZPL raw primeiro | [QZ Tray](https://qz.io/), [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
| Cloud para impressoras em muitos sites | [PrintNode](https://www.printnode.com/en) |

## Fique no hiprint quando

- Designers já possuem centenas de templates no editor visual
- electron-hiprint (ou seu fork) está estável nas suas mesas
- Docs em chinês servem para o time

## Saia (ou hibridize) quando

- Você quer chamadas de impressão que pareçam um SDK frontend normal em páginas Vue/React
- Precisa de onboarding em inglês para mesas no exterior
- Impressão cross-network precisa de história cloud gerenciada mais clara

## Híbrido prático

Mantenha hiprint para design de template, exporte para HTML/PDF/imagem, depois imprima via ponte silenciosa genérica. Dá mais trabalho no início, mas evita reescrever todo template quando o client muda.

## Relacionados

- [Como escolher uma stack de impressão silenciosa](choose-silent-print-stack.pt-BR.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.pt-BR.md)
- [Alternativas ao Lodop](lodop-alternatives.pt-BR.md)
