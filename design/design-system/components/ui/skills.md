# Componente — Skills

## Nome canônico
`Skills`

## Descrição
Capacidades disponíveis para invocação no contexto do agente. Lista ou painel de skills que o agente pode executar, com status e ação de ativação. Diferente de Market (onde você instala), Skills mostra o que já está disponível e ativo.

---

## Anatomia

```
Skills
├── SkillsHeader            ← título + busca
├── SkillList               ← lista de skills disponíveis
│   └── SkillItem[]         ← item individual
│       ├── SkillIcon       ← ícone da skill
│       ├── SkillName       ← nome da skill
│       ├── SkillDescription ← descrição de uma linha
│       ├── SkillToggle     ← ativar/desativar
│       └── SkillTrigger    ← sintaxe de invocação (ex: "/generate")
└── SkillsFooter            ← link para Market (adicionar mais)
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `panel` | Painel de gerenciamento completo |
| `inline` | Lista compacta dentro de settings ou chat |
| `autocomplete` | Dropdown de sugestão ao digitar "/" no UserInput |

---

## Estados do SkillItem

| Estado | Comportamento visual |
|---|---|
| `active` | Toggle ON (`--primary`), SkillName em `--foreground` |
| `inactive` | Toggle OFF (`--muted`), SkillName em `--muted-foreground` |
| `running` | Spinner animado, SkillToggle bloqueado |
| `error` | Badge de erro `--destructive`, tooltip com descrição |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `panel \| inline \| autocomplete` | `panel` | Modo de exibição |
| `skills` | `Skill[]` | obrigatório | Lista de skills |
| `onToggle` | `function(skillId, active)` | obrigatório | Callback de ativação |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background painel | `--card` |
| SkillName (ativo) | `--foreground` + `label-2` |
| SkillName (inativo) | `--muted-foreground` + `label-2` |
| SkillDescription | `--muted-foreground` + `body-sm` |
| SkillTrigger | `--muted` background + `--muted-foreground` texto + `code-sm` |
| Toggle ON | `--primary` |
| Toggle OFF | `--muted` |
| Padding SkillItem | `--space-3` |
| Gap entre items | `--space-2` |
| Borda separadora | `--border` |

---

## Regras de uso

- **Use Skills** para mostrar capacidades disponíveis, não para instalação (use Market)
- SkillTrigger (ex: `/generate`) deve sempre ser visível — é o gatilho de invocação
- Na variante `autocomplete`, lista até 6 skills filtradas pelo texto após "/"
- Skills `inactive` ainda aparecem na lista com toggle para fácil ativação

---

## Anti-patterns

- **PROIBIDO**: esconder skills inativas da lista
- **PROIBIDO**: SkillItem sem SkillTrigger visível
- **PROIBIDO**: usar Skills como substituto de Market para instalação de novas skills

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
