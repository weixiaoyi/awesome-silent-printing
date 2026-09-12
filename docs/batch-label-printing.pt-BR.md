# Impressão em lote e etiquetas a partir da web

## Problema

Armazéns e mesas de ecommerce muitas vezes precisam de **dezenas ou centenas** de etiquetas sem um humano clicando em diálogos de impressão. Um diálogo por etiqueta não é design operacional — é incidente.

## O que você precisa

1. Um caminho silencioso para uma impressora **nomeada**
2. Uma fila / API de lote (loop no client-side não basta em escala)
3. Renderização estável de template (HTML ou ZPL)
4. Retry + logging quando um job falha no meio do lote
5. Back-pressure quando a impressora é mais lenta que quem envia

## Padrões

| Padrão | Notas | Melhor quando |
|---|---|---|
| Navegador → API de lote do agente local | Menor latência no PC da mesa | Operador trabalha dentro da SPA |
| Push do servidor → pull do agente na mesa | Melhor para muitas estações de packing | Navegador não precisa ficar aberto |
| Raw do fabricante (ZPL) | Excelente para impressoras só de etiqueta | Frota já padronizada em ZPL |

## Fluxo de lote recomendado

```text
Selecionar pedidos
  → renderizar ou buscar N templates
  → enviar como lote (ou lotes em chunks de 20–50)
  → mostrar status por job (na fila / imprimindo / ok / falhou)
  → retry só nos ids que falharam
```

### Chunking

Enviar 500 jobs em um Promise.all costuma piorar mais do que ajudar. Prefira chunks:

- 20–50 jobs por chunk para agentes que renderizam HTML
- Chunks maiores podem servir para strings ZPL pequenas
- Aguarde conclusão do chunk (ou limite de concorrência 2–3) antes do próximo

## Taxonomia de falhas

| Falha | Ação do operador | Ação do sistema |
|---|---|---|
| Agente offline | Instalar / iniciar agente | Pausar fila; banner |
| Impressora offline / sem papel | Corrigir hardware | Marcar jobs como retryable |
| Template / código de barras ruim | Corrigir dados | Falhar aquele job; continuar os outros |
| LNA / HTTPS | TI corrige origem | Veja [Chrome LNA](chrome-local-network-access.pt-BR.md) |

## Checklist

- [ ] Seleção de impressora por tamanho de papel / estação
- [ ] Envio em lote + status por job
- [ ] Página HTTPS alcança localhost (LNA)
- [ ] Testes de regressão de template para código de barras/QR
- [ ] Job ids idempotentes (reenvio seguro)
- [ ] Ops consegue exportar ids de jobs que falharam

## Relacionados

- [Impressão silenciosa remota](remote-silent-print.pt-BR.md)
- [Impressão silenciosa com HTML/CSS](html-css-silent-print.pt-BR.md)
- [Impressão silenciosa em térmica](thermal-receipt-silent-print.pt-BR.md)
- [Como escolher uma stack](choose-silent-print-stack.pt-BR.md)
