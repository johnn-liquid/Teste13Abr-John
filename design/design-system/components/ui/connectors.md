# Componente — Connectors

## Nome canônico
`Connectors`

## Descrição
Integrações e conexões com serviços externos disponíveis para o agente. Painel de gerenciamento de conectores que mostra quais integrações estão ativas, disponíveis ou com erro.

---

## Anatomia

```
Connectors
├── ConnectorsHeader        ← título + filtro por categoria
├── ConnectorGrid           ← grade de conectores disponíveis
│   └── ConnectorCard[]     ← card de cada integração
│       ├── ConnectorLogo   ← logo do serviço
│       ├── ConnectorName   ← nome do serviço
│       ├── ConnectorStatus ← status da conexão
│       └── ConnectorAction ← conectar / desconectar / configurar
└── ConnectedList           ← lista de conectores já ativos (separado)
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `grid` | Grade de cards — vista principal de marketplace de integrações |
| `list` | Lista compacta — sidebar ou configurações |

---

## Estados do ConnectorCard

| Estado | Comportamento visual |
|---|---|
| `available` | Logo + nome + botão "Conectar" outline |
| `connected` | Logo + nome + badge "Conectado" verde + botão "Configurar" |
| `error` | Logo + nome + badge "Erro" vermelho + botão "Reconectar" |
| `loading` | Skeleton do card |
| `disabled` | Opacity 40%, tooltip "Indisponível no plano atual" |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `grid \| list` | `grid` | Layout do componente |
| `connectors` | `Connector[]` | obrigatório | Lista de conectores |
| `onConnect` | `function(connectorId)` | obrigatório | Inicia fluxo de autorização |
| `onDisconnect` | `function(connectorId)` | obrigatório | Desconecta integração |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background ConnectorCard | `--card` |
| Borda ConnectorCard | `--border` |
| ConnectorName | `--card-foreground` + `label-2` |
| Badge connected | background `--chart-2` 20%, texto `--chart-2` |
| Badge error | background `--destructive` 20%, texto `--destructive` |
| Botão Conectar | outline `--border` |
| Radius ConnectorCard | `--radius-lg` |
| Shadow ConnectorCard | `--shadow-sm` |
| Gap no grid | `--space-4` |

---

## Regras de uso

- **Use Connectors** na seção de configurações ou como painel lateral de integrações
- ConnectorCard sempre exibe o status atual — nunca omita o ConnectorStatus
- O fluxo de autorização (OAuth) abre em popup/modal, não redireciona a página inteira

---

## Anti-patterns

- **PROIBIDO**: ConnectorCard sem status visual
- **PROIBIDO**: desconectar sem confirmar se o agente depende daquele conector
- **PROIBIDO**: mostrar conectores `disabled` sem explicar o motivo no tooltip

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
