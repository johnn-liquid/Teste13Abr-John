# Componente — Market

## Nome canônico
`Market`

## Descrição
Marketplace de agentes ou skills disponíveis para o usuário adicionar ao seu workspace. Galeria navegável com cards de agentes/skills, filtros e ações de instalação.

---

## Anatomia

```
Market
├── MarketHeader            ← título + busca + filtros por categoria
├── MarketGrid              ← grade de cards
│   └── MarketCard[]        ← card de agente ou skill
│       ├── CardImage       ← imagem/ícone do produto
│       ├── CardName        ← nome
│       ├── CardDescription ← descrição curta (2 linhas)
│       ├── CardMeta        ← categoria + avaliação
│       └── CardAction      ← botão Instalar / Instalado / Ver detalhes
└── MarketPagination        ← paginação ou carregamento infinito
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `grid` | Grade de cards — vista padrão do marketplace |
| `list` | Lista compacta para resultados de busca |
| `featured` | Destaque horizontal para agentes/skills em promoção |

---

## Estados do MarketCard

| Estado | Comportamento visual |
|---|---|
| `available` | CardAction com botão "Instalar" outline |
| `installed` | CardAction com badge "Instalado" + botão "Remover" ghost |
| `loading` | Skeleton do card |
| `installing` | CardAction com spinner + "Instalando..." |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `grid \| list \| featured` | `grid` | Layout do Market |
| `items` | `MarketItem[]` | obrigatório | Lista de agentes/skills |
| `onInstall` | `function(itemId)` | obrigatório | Callback de instalação |
| `onRemove` | `function(itemId)` | obrigatório | Callback de remoção |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background MarketCard | `--card` |
| Borda MarketCard | `--border` |
| Shadow MarketCard | `--shadow-sm` |
| CardName | `--card-foreground` + `label-2` |
| CardDescription | `--muted-foreground` + `body-sm` |
| Badge installed | background `--chart-2` 20%, texto `--chart-2` |
| Botão Instalar | outline `--border` |
| Radius MarketCard | `--radius-lg` |
| Gap no grid | `--space-4` |

---

## Regras de uso

- **Use Market** como seção dedicada — não como modal ou dropdown
- MarketCard tem cursor pointer — ao clicar abre detalhes antes de instalar
- Busca filtra em tempo real após 300ms de debounce

---

## Anti-patterns

- **PROIBIDO**: instalar agente/skill sem confirmação para itens que afetam dados do workspace
- **PROIBIDO**: MarketCard sem CardDescription
- **PROIBIDO**: misturar agentes e skills na mesma lista sem separação visual por tipo

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
