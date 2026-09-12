# Impressão silenciosa remota / push do servidor

## Quando a chamada local do navegador não basta

- Estações de packing não devem abrir a SPA de negócio
- Jobs são criados em um WMS/OMS central
- Várias mesas devem consumir a mesma fila
- Turnos noturnos imprimem jobs criados na UI de outro site

## Arquitetura comum

```text
Servidor de negócio
    → fila / webhook / feed websocket
    → agente de impressão na mesa
    → impressora local
```

O navegador pode servir só para configuração (vincular estação ↔ impressoras). O agente puxa ou recebe jobs continuamente.

### Dois estilos de transporte

| Estilo | Como funciona | Fique atento a |
|---|---|---|
| Relay na nuvem (ex.: estilo PrintNode) | API do servidor → cloud do fabricante → client local | Segurança da conta, mapeamento por site |
| Pull self-hosted | Agente consulta sua API com token da estação | Auth, backoff, fila durável |
| Transit / relay para stacks com designer | Hop extra para clients estilo hiprint | Complexidade operacional |

## Notas de design

- **Autentique o agente**; não exponha uma porta raw de impressão na internet
- Mantenha o payload do job idêntico ao formato do SDK local quando possível (mesmo HTML/PDF/raw)
- Trate mesas offline com filas duráveis e dead-letter para jobs envenenados
- Registre `job id → estação → impressora → resultado` para ops
- Versione templates; um deploy ruim deve ser reversível sem reimprimir histórico duas vezes
- Rate-limit por estação para uma mesa não starvar as outras

## Checklist de segurança

- [ ] Credenciais da estação são rotacionáveis
- [ ] TLS para sua API
- [ ] Agente escuta só localhost para features do navegador; canal remoto é outbound
- [ ] Sem “imprimir URL arbitrária” a partir de campos não confiáveis do job
- [ ] Auditar quem pode enfileirar para qual estação

## Runbook de ops (mínimo)

1. Estação offline → acionar on-call com horário do último heartbeat
2. Impressora sem papel → mostrar na UI da estação / LED se tiver
3. Template envenenado → colocar job em quarentena; alertar donos do template
4. Replay → só para ids explicitamente falhos

## Relacionados

- [Impressão em lote e etiquetas](batch-label-printing.pt-BR.md)
- [Como escolher uma stack](choose-silent-print-stack.pt-BR.md)
- Exemplos no mundo real: PrintNode, relays transit do hiprint e agentes locais com pull remoto de jobs (veja a [lista principal](../README.pt-BR.md#cloud--remote-print))
