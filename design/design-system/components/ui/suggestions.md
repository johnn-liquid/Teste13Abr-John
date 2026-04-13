# Componente — Suggestions

## Nome canônico
`Suggestions`

## Descrição
Sugestões de ações ou respostas rápidas exibidas no chat. Aparecem proativamente acima do UserInput ou após uma mensagem do agente, permitindo ao usuário responder ou continuar a conversa sem digitar.

---

## Anatomia

```
Suggestions
└── SuggestionList          ← container horizontal com scroll se necessário
    └── SuggestionChip[]    ← chips clicáveis com label
        ├── Icon (opcional) ← ícone contextual
        └── Label           ← texto da sugestão
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `inline` | Chips após uma mensagem do agente, alinhados à esquerda |
| `bar` | Barra acima do UserInput, linha horizontal com scroll |
| `grid` | Grade 2xN para sugestões de onboarding ou boas-vindas |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `default` | Chips visíveis e clicáveis |
| `selected` | Chip com background `--primary`, texto `--primary-foreground` |
| `disabled` | Opacity 40%, sem hover, sem clique |
| `loading` | Chips substituídos por skeletons (3–4 placeholders) |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `inline \| bar \| grid` | `bar` | Layout dos chips |
| `suggestions` | `{ label: string, icon?: ReactNode }[]` | obrigatório | Lista de sugestões |
| `onSelect` | `function(label: string)` | obrigatório | Callback ao clicar |
| `maxVisible` | `number` | `4` | Máximo de chips visíveis sem scroll |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background do chip (default) | `--secondary` |
| Background do chip (hover) | `--accent` |
| Background do chip (selected) | `--primary` |
| Texto do chip (default) | `--secondary-foreground` + `label-2` |
| Texto do chip (selected) | `--primary-foreground` + `label-2` |
| Borda do chip | `--border` |
| Radius do chip | `--radius-full` |
| Padding do chip | `--space-3` (horizontal) + `--space-1-5` (vertical) |
| Gap entre chips | `--space-2` |
| Transição hover | `--duration-fast` + `--ease-out` |

---

## Regras de uso

- **Use Suggestions** para reduzir fricção em conversas estruturadas com opções previsíveis
- Máximo de 6 sugestões visíveis por vez — se houver mais, use scroll horizontal
- Após o usuário selecionar uma sugestão, a lista desaparece (`--duration-fast` fade out)
- As sugestões devem refletir o contexto da última mensagem do agente
- Na variante `grid`, máximo de 6 chips em grade 2×3

---

## Anti-patterns

- **PROIBIDO**: exibir mais de 6 sugestões visíveis ao mesmo tempo
- **PROIBIDO**: usar Suggestions para substituir ações primárias da tela
- **PROIBIDO**: chips com texto maior que 40 caracteres — truncar com "..."
- **PROIBIDO**: manter chips visíveis após o usuário selecionar um
- **PROIBIDO**: chips sem label legível (apenas ícone)

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
