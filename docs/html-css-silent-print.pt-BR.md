# Impressão silenciosa com HTML/CSS

## Por que os times querem isso

A maioria das UIs de negócio já é HTML/CSS. Se a impressão silenciosa reutilizar os mesmos templates, a velocidade do frontend continua alta e você evita manter um segundo skill set ZPL/ESC/POS para cada fatura.

## Dois modelos de layout

| Modelo | Prós | Contras |
|---|---|---|
| HTML/CSS via Chromium/agente local | Familiar para times web; boa fidelidade | Precisa de agente local |
| ESC/POS / ZPL raw | Controle preciso do dispositivo | Skill set diferente; específico de hardware |

Muitos produtos misturam os dois: docs A4 em HTML, tickets térmicos em raw.

## O que “print CSS” significa aqui

`@media print` do navegador sozinho **não** é caminho silencioso — ainda passa por `window.print()`. Com agente local você tipicamente:

1. Monta uma string HTML autocontida (ou URL).
2. Inclui o CSS que o agente precisa (inline, URL absoluta linkada ou bundled).
3. Passa tamanho de papel / margens / nome da impressora nas opções do SDK.
4. Deixa o motor do agente (muitas vezes Chromium) paginar e enviar ao spooler.

### Checklist de template

- [ ] Tamanho de página explícito (A4, 100×150 mm, rolo 80 mm, …)
- [ ] Margens que batem com o material físico
- [ ] Fontes instaladas na estação ou embutidas
- [ ] Código de barras/QR como SVG ou imagem em alta resolução (teste a leitura)
- [ ] Tabelas que não cortam na última linha
- [ ] Sem dependência de layouts só de viewport (`100vh` prende)

## Dicas práticas

- Desenhe uma folha de estilo **só para impressão**; não reutilize cegamente o CSS completo do app.
- Prefira unidades `mm` / `in` para etiquetas; só `px` deriva entre DPIs.
- Teste fontes chinesas / CJK no Windows **e** em mesas Linux se suportar ambos.
- Faça preview de um job antes de habilitar lote ao migrar templates.
- Mantenha imagens pequenas; PNGs gigantes matam throughput de lote.
- Para cortadores térmicos / gavetas exatos, você ainda pode precisar de comandos raw em ponte raw-capable.

## Bom encaixe

Faturas, extratos, listas de separação, relatórios A4, muitos layouts de etiqueta renderizados como HTML.

## Encaixe fraco (considere raw / SDK do fabricante)

- Impressoras de cozinha ultra-rápidas que esperam só ESC/POS
- Frotas Zebra padronizadas em templates ZPL no firmware da impressora
- Dispositivos sem caminho útil de driver Windows/macOS/Linux exceto raw do fabricante

## Relacionados

- [Impressão silenciosa em Vue / React](vue-react-silent-print.pt-BR.md)
- [Impressão em lote e etiquetas](batch-label-printing.pt-BR.md)
- [Impressão silenciosa em térmica](thermal-receipt-silent-print.pt-BR.md)
- [Como escolher uma stack](choose-silent-print-stack.pt-BR.md)
