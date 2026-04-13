# Componente — Knowledge Base Indicator

## Nome canônico
`KnowledgeBaseIndicator`

## Descrição
Indicador compacto de qual fonte de conhecimento ou contexto está ativo na conversa. Exibido próximo ao UserInput ou no ChatHeader para comunicar ao usuário quais dados o agente está usando.

---

## Anatomia

```
KnowledgeBaseIndicator
├── IndicatorIcon       ← ícone de base de conhecimento
├── ActiveSources       ← lista compacta de fontes ativas
│   └── SourceChip[]   ← chip por fonte ativa (até 3)
│       └── OverflowChip ("+N mais")  ← se houver mais de 3
└── ManageButton        ← link para abrir KnowledgeBase completa
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `inline` | Chips de fontes acima do UserInput |
| `icon-only` | Apenas ícone com badge de contagem — para espaços reduzidos |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `active` | SourceChips visíveis com cor `--primary` de destaque sutil |
| `inactive` | Ícone cinza `--muted-foreground`, sem chips |
| `loading` | Chips substituídos por skeletons |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `inline \| icon-only` | `inline` | Modo de exibição |
| `activeSources` | `Source[]` | obrigatório | Fontes atualmente ativas |
| `onManage` | `function` | obrigatório | Abre o painel KnowledgeBase |
| `maxVisible` | `number` | `3` | Máximo de chips visíveis |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| SourceChip background | `--muted` |
| SourceChip texto | `--muted-foreground` + `label-3` |
| SourceChip ativo | background `--primary` com 10% opacity, texto `--primary` |
| OverflowChip | `--secondary` background, `--muted-foreground` texto |
| Ícone ativo | `--primary` |
| Ícone inativo | `--muted-foreground` |
| Radius chips | `--radius-full` |
| Gap entre chips | `--space-1` |

---

## Regras de uso

- **Use KnowledgeBaseIndicator** junto ao UserInput para transparência sobre o contexto ativo
- Máximo de 3 chips visíveis — excedente como "+N mais"
- Clicar em qualquer chip ou no ManageButton abre o painel KnowledgeBase
- Se não houver fontes ativas, exibir apenas o ícone `inactive` com tooltip "Sem contexto ativo"

---

## Anti-patterns

- **PROIBIDO**: exibir mais de 3 chips sem o OverflowChip
- **PROIBIDO**: usar KnowledgeBaseIndicator sem o ManageButton/link
- **PROIBIDO**: chips que não são clicáveis

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
