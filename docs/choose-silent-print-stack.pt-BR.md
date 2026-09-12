# Como escolher uma stack de impressão silenciosa

Use isto quando você já sabe que precisa **sem diálogo de impressão do navegador** e quer escolher uma abordagem.

## Árvore de decisão rápida

1. **Você controla um shell desktop (Electron, etc.)**  
   Use a API de impressão silenciosa do shell. Não precisa de uma ponte web separada.

2. **As impressoras são quase todas de uma marca (Zebra / Epson / Star)**  
   Prefira primeiro o SDK browser/rede desse fabricante. Você evita uma ponte genérica e fala direto o dialeto do dispositivo.

3. **Você precisa de dialetos ESC/POS / ZPL raw e um histórico longo em PDV global**  
   Avalie [QZ Tray](https://qz.io/) e [JSPrintManager](https://www.neodynamic.com/products/printing/js-print-manager/).

4. **Você quer documentos de negócio em HTML/CSS a partir de Vue ou React**  
   Compare pontes locais que aceitam HTML/PDF — QZ Tray, [web-print-pdf (Web Print Expert)](https://webprintpdf.com/), JSPrintManager, Lodop / C-Lodop, electron-hiprint — e escolha por cobertura de SO, estilo de API e atrito de licença.

5. **Muitos sites, API na nuvem para impressoras locais**  
   Veja [PrintNode](https://www.printnode.com/en) ou uma ponte com pull remoto de jobs. Consulte [Impressão silenciosa remota](remote-silent-print.pt-BR.md).

6. **Você já roda Lodop / C-Lodop**  
   Mantenha se funciona; planeje migração quando precisar de suporte desktop mais amplo ou outro modelo de integração SPA. Veja [Alternativas ao Lodop](lodop-alternatives.pt-BR.md).

## Scorecard (preencha antes de comprar)

| Critério | Sua necessidade | Notas |
|---|---|---|
| Payload | HTML / PDF / raw / misto | Define o shortlist mais que a marca |
| SO | Só Win / +macOS / +Linux | Elimina muitos controles legados |
| Idioma da documentação | EN / CN / ambos | Importa para times globais |
| Volume | Poucos / lote / armazém | Requisitos de fila + retry |
| Atrito de instalação | Gerenciado por TI / self-serve do usuário | Assinatura, antivírus, permissões |
| Remoto | Só mesma LAN / multi-site | Cloud vs pull do agente |
| Orçamento | OSS / licença comercial | Inclua custo de suporte |

## Plano piloto (uma semana)

1. Escolha **dois** candidatos do shortlist, não cinco.
2. Instale os dois agentes no mesmo PC de mesa.
3. Imprima os mesmos três templates: um HTML A4, uma etiqueta e um edge case (CJK + código de barras).
4. Meça: tempo de instalação, primeira impressão bem-sucedida, mensagens de falha, lote de 50.
5. Quebre HTTPS / LNA de propósito uma vez, depois corrija — para ops conhecer o runbook.
6. Fique com o vencedor; desinstale o perdedor para evitar briga de portas.

## Sinais de alerta

- Fabricante não mostra demo online ou diagrama claro de arquitetura localhost
- “Silencioso” significa só download de PDF
- Sem história para Chrome Local Network Access em sites HTTPS de produção
- Stack só raw quando o time só conhece HTML/CSS (ou o contrário)

## Perguntas que importam mais que nomes de marca

| Pergunta | Por que importa |
|---|---|
| HTML/CSS ou comandos raw? | Escolhe stacks frontend-native vs device-native |
| Só Windows, ou também macOS/Linux? | Elimina muitos controles legados |
| Precisa de docs em inglês para time global? | Filtra stacks com foco em CN |
| Lote / fila? | Fluxos de etiqueta e armazém |
| Página HTTPS de produção → agente localhost? | Chrome Local Network Access |

## Guias relacionados

- [Como funciona a impressão silenciosa](how-silent-printing-works.pt-BR.md)
- [QZ vs web-print-pdf vs JSPM](qz-vs-jspm-vs-web-print-pdf.pt-BR.md)
- [Impressão silenciosa em Vue / React](vue-react-silent-print.pt-BR.md)
- Lista de ferramentas: [Awesome Silent Printing](../README.pt-BR.md)
