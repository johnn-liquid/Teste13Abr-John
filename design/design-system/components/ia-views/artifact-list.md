# Componente — Artifact List

## Nome canônico
`ArtifactList`

## Descrição
Lista de itens gerada pela IA. Para outputs com múltiplos itens similares que não precisam de colunas (ao contrário do ArtifactDatatable). Suporta itens simples (texto) e itens ricos (ícone + texto + meta).

---

## Anatomia

```
ArtifactList
├── ArtifactHeader          ← label + título + ações
│   ├── ListTitle
│   └── ArtifactActions     ← copiar, exportar
├── ListContent             ← itens da lista
│   └── ListItem[]
│       ├── ItemIndex (opcional)   ← numeração
│       ├── ItemIcon (opcional)    ← ícone por tipo
│       ├── ItemContent            ← conteúdo principal
│       │   ├── ItemLabel          ← texto principal
│       │   └── ItemMeta (opcional) ← informação secundária
│       └── ItemAction (opcional)  ← ação rápida por item
└── ListFooter              ← contagem total + "Ver mais" se truncado
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `simple` | Apenas texto por item |
| `numbered` | Com índice numérico |
| `rich` | Com ícone + label + meta por item |
| `checklist` | Com checkbox por item (para tarefas) |

---

## Dados obrigatórios vs opcionais

| Campo | Obrigatoriedade |
|---|---|
| `items` | Obrigatório |
| `title` | Recomendado |
| `variant` | Obrigatório |
| `item.meta` | Opcional |
| `item.action` | Opcional |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `loading` | Skeleton de 4 linhas |
| `populated` | Lista completa |
| `empty` | "Nenhum item disponível" centralizado |
| `truncated` | Primeiros 5 itens + botão "Ver mais N itens" |
| `error` | "Erro ao carregar lista" + retry |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `title` | `string` | — | Título da lista |
| `variant` | `simple \| numbered \| rich \| checklist` | `simple` | Tipo de lista |
| `items` | `ListItem[]` | obrigatório | Itens da lista |
| `maxVisible` | `number` | `5` | Itens visíveis antes do colapso |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background | `--card` |
| Borda | `--border` |
| ItemLabel | `--card-foreground` + `body-md` |
| ItemMeta | `--muted-foreground` + `body-sm` |
| ItemIndex | `--muted-foreground` + `label-3` |
| ItemIcon | `--muted-foreground` |
| Borda entre itens | `--border` |
| Checkbox (checklist) | `--primary` quando marcado |
| Radius | `--radius-lg` |
| Padding item | `--space-3` |

---

## Anti-patterns

- **PROIBIDO**: lista com mais de 10 itens sem truncamento
- **PROIBIDO**: usar ArtifactList quando os itens têm múltiplas colunas — use ArtifactDatatable
- **PROIBIDO**: checklist sem indicação do estado inicial dos checkboxes

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
