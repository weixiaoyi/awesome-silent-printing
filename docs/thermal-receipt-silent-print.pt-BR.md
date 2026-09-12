# Impressão silenciosa de cupom térmico a partir do navegador

**Impressão de cupom térmico a partir de uma página web** costuma precisar de impressão silenciosa: caixas e cozinha não podem clicar em diálogo a cada ticket.

## O que funciona

1. **Ponte de impressão local** — HTML/imagem/PDF para tickets simples, ou ESC/POS raw para controle total do dispositivo
2. **SDK do fabricante** para impressoras Epson / Star em rede (página fala com impressora ou serviço do fabricante)
3. **Shell desktop** com impressão silenciosa se você entrega um app POS Electron

## O que não funciona

- `window.print()` como caminho silencioso de produção
- Bibliotecas puras de download de PDF sem agente de impressão local
- Assumir que toda “página CSS 80mm” vai cortar e abrir gaveta sem comandos raw

## Escolhas de arquitetura

| Necessidade | Incline para |
|---|---|
| Logo + layout HTML variável, baixo volume | HTML via agente local |
| Cozinha alto volume, cortador, gaveta, buzzer | ESC/POS raw em QZ / JSPM / SDK do fabricante |
| Só Epson/Star na LAN | SDK estilo ePOS / webPRNT do fabricante |
| Você já entrega app POS desktop | Impressão silenciosa Electron |

## Dicas de template

- Prefira layouts de largura fixa (ex.: **58mm / 80mm**)
- Mantenha códigos de barras em alto contraste; teste com o leitor que você usa de verdade
- Evite grids CSS pesados; motores térmicos e drivers não perdoam
- Teste comandos de cortador / gaveta **somente** em stacks raw-capable
- Codifique code pages corretamente para CJK / texto acentuado em ESC/POS
- Imprima ticket de calibração após atualizar driver ou agente

## Balcão vs cozinha

| | Balcão | Cozinha |
|---|---|---|
| Tolerância a latência | Baixa | Muito baixa |
| Payload típico | HTML ou ESC/POS | Muitas vezes ESC/POS |
| UX de falha | Mostrar retry ao caixa | Auto-retry + alerta alto |
| Multi-impressora | Cupom + etiqueta | Rotear por estação / item |

## Relacionados

- [Impressão em lote e etiquetas](batch-label-printing.pt-BR.md)
- [Impressão silenciosa com HTML/CSS](html-css-silent-print.pt-BR.md)
- [Como escolher uma stack de impressão silenciosa](choose-silent-print-stack.pt-BR.md)
- Lista de ferramentas: [Awesome Silent Printing](../README.pt-BR.md)
