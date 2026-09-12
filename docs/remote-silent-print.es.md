# Impresión silenciosa remota / empujada desde el servidor

## Cuándo la llamada local del navegador no basta

- Las estaciones de empaquetado no deberían abrir la SPA de negocio
- Los trabajos se crean en un WMS/OMS central
- Varios escritorios deben consumir la misma cola
- Turnos nocturnos imprimen trabajos creados en la UI de otro sitio

## Arquitectura habitual

```text
Servidor de negocio
    → cola / webhook / feed websocket
    → agente de impresión en escritorio
    → impresora local
```

El navegador puede usarse solo para configuración (vincular estación ↔ impresoras). El agente obtiene o recibe trabajos de forma continua.

### Dos estilos de transporte

| Estilo | Cómo funciona | Vigilar |
|---|---|---|
| Relay en la nube (p. ej. estilo PrintNode) | API del servidor → nube del fabricante → cliente local | Seguridad de cuenta, mapeo por sitio |
| Pull autohospedado | El agente consulta tu API con un token de estación | Auth, backoff, cola durable |
| Tránsito / relay para stacks con diseñador | Salto extra para clientes estilo hiprint | Complejidad operativa |

## Notas de diseño

- **Autentica el agente**; no expongas un puerto de impresión raw a internet
- Mantén el payload del trabajo idéntico a la forma de tu SDK local cuando sea posible (mismo HTML/PDF/raw)
- Gestiona escritorios offline con colas durables y dead-letter para trabajos envenenados
- Registra `job id → estación → impresora → resultado` para ops
- Versiona plantillas; un mal despliegue debe poder revertirse sin reimprimir el historial dos veces
- Limita la tasa por estación para que un escritorio no ahogue a los demás

## Checklist de seguridad

- [ ] Credenciales de estación rotativas
- [ ] TLS hacia tu API
- [ ] El agente se enlaza solo a localhost para funciones de navegador; el canal remoto es saliente
- [ ] Sin «imprimir URL arbitraria» desde campos de trabajo no confiables
- [ ] Auditoría de quién puede encolar a qué estación

## Runbook de ops (mínimo)

1. Estación offline → avisar on-call con hora del último heartbeat
2. Impresora sin papel → mostrar en UI de estación / LED si lo tienes
3. Plantilla envenenada → cuarentena del trabajo; alertar a dueños de plantilla
4. Replay → solo para ids explícitamente fallidos

## Relacionado

- [Impresión por lotes y etiquetas](batch-label-printing.es.md)
- [Elegir un stack](choose-silent-print-stack.es.md)
- Ejemplos en la práctica: PrintNode, relays de tránsito hiprint y agentes locales con pull remoto de trabajos (ver la [lista principal](../README.es.md#cloud--remote-print))
