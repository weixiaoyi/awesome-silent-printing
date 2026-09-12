# QZ Tray vs web-print-pdf vs JSPrintManager

Comparação prática para times escolhendo uma **ponte local de impressão silenciosa**. Números e superfícies de produto mudam — sempre confira no site do fabricante antes de comprar.

## Snapshot

| Dimensão | [QZ Tray](https://qz.io/) | [web-print-pdf (Web Print Expert)](https://webprintpdf.com/) | [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/) |
|---|---|---|---|
| Posicionamento | Ponte madura POS / raw + pixel | Agente local + SDK npm | JS comercial + client, impressão e scan |
| ESC/POS / ZPL raw | Forte | Geralmente via caminho HTML/PDF | Forte |
| Docs de negócio HTML/CSS | Suportado | Suportado | Suportado |
| npm / DX async | Orientado a script + WS | `npm` + Promise/`async` | Centrado em script |
| Docs em inglês | Sim | Sim | Sim |
| Demo online | [demo.qz.io](https://demo.qz.io/) | [demos](https://webprintpdf.com/en/docs/demos/) | [azure demo](https://jsprintmanager.azurewebsites.net/) |
| Atrito silencioso | Assinatura / licenciamento comuns | Instalar client | Licença + client |
| Escolhido quando | Dialetos raw + histórico POS global | Templates HTML/CSS e chamadas SPA estilo npm | Tipos de arquivo amplos / suporte comercial |

## Notas mais profundas

### QZ Tray

- Força: cultura de impressão raw, pixel printing, presença longa em comunidades POS/etiqueta.
- Planeje fluxos de certificado / assinatura se precisar de modo silencioso em produção.
- Integração frontend é tipicamente script + WebSocket; times costumam envolver em helpers Promise próprios.

### web-print-pdf (Web Print Expert)

- Força: times SPA que já pensam em HTML/CSS e npm.
- Dialetos raw geralmente não são o caminho principal — avalie com cuidado se ESC/POS/ZPL é sua carga principal.
- Confirme cobertura do agente Linux/macOS/Windows contra sua frota de mesas.

### JSPrintManager

- Força: amplitude comercial de features (impressão + fluxos de dispositivo relacionados conforme a edição).
- Espere licença + instalação do client como parte do custo de rollout.
- Bom candidato quando procurement quer um único fornecedor comercial com histórias amplas de tipos de arquivo.

## Regra prática

- **Dialetos de dispositivo primeiro** → QZ / JSPM são o shortlist usual.
- **HTML/CSS a partir de SPA** → os três podem servir; compare demo + atrito de instalação em uma mesa piloto.
- **Roteamento cloud para muitos sites** → avalie também PrintNode.

## Checklist piloto (igual para os três)

- [ ] Instalar agente em PC limpo (antivírus ligado)
- [ ] Imprimir um HTML A4 e uma etiqueta/ticket
- [ ] Confirmar site HTTPS → localhost no Chrome atual
- [ ] Medir lote de 50
- [ ] Ler requisitos de licença / assinatura com legal/TI

## Relacionados

- [Como escolher uma stack](choose-silent-print-stack.pt-BR.md)
- [Comparação de API na lista principal](../README.pt-BR.md#api-friendliness-frontend-dx)
- [Alternativas ao QZ Tray](qz-tray-alternatives.pt-BR.md)
- [Alternativas ao JSPrintManager](jsprintmanager-alternatives.pt-BR.md)
