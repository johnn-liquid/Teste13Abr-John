# Componente — Navbar

## Nome canônico
`Navbar`

## Descrição
Barra de navegação principal do produto. Estrutura lateral (sidebar) ou superior (topbar) que provê acesso às seções principais, identidade da aplicação e ações globais.

---

## Anatomia

```
Navbar
├── NavHeader               ← logo / nome do produto
├── NavPrimary              ← itens de navegação principal
│   └── NavItem[]           ← item individual
│       ├── NavIcon         ← ícone do item
│       └── NavLabel        ← label do item
├── NavSecondary            ← itens secundários (configs, help)
│   └── NavItem[]
└── NavFooter               ← AccountSwitch + ações de usuário
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `sidebar-expanded` | Sidebar visível com ícones + labels |
| `sidebar-collapsed` | Sidebar comprimida — apenas ícones |
| `topbar` | Barra horizontal no topo — apenas para layouts sem sidebar |

---

## Estados dos NavItems

| Estado | Comportamento visual |
|---|---|
| `default` | Ícone + label em `--sidebar-foreground` |
| `hover` | Background `--sidebar-accent`, texto `--sidebar-accent-foreground` |
| `active` | Background `--sidebar-primary`, texto `--sidebar-primary-foreground` |
| `disabled` | Opacity 40%, sem hover, sem clique |
| `with-badge` | Badge numérico no canto superior direito do ícone |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `sidebar-expanded \| sidebar-collapsed \| topbar` | `sidebar-expanded` | Layout da navbar |
| `items` | `NavItem[]` | obrigatório | Lista de itens de navegação principal |
| `secondaryItems` | `NavItem[]` | — | Itens secundários (configurações, ajuda) |
| `activeItemId` | `string` | obrigatório | ID do item atualmente ativo |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background da navbar | `--sidebar` |
| Borda direita (sidebar) | `--sidebar-border` |
| NavLabel | `--sidebar-foreground` + `label-2` |
| NavItem hover | background `--sidebar-accent`, texto `--sidebar-accent-foreground` |
| NavItem active | background `--sidebar-primary`, texto `--sidebar-primary-foreground` |
| NavIcon | herda cor do contexto pai |
| Padding do NavItem | `--space-3` (vertical) + `--space-4` (horizontal) |
| Gap entre NavItems | `--space-1` |
| Radius do NavItem | `--radius-md` |
| Focus ring | `--sidebar-ring` |
| Transição hover | `--duration-fast` + `--ease-out` |

---

## Regras de uso

- **Use Navbar** como estrutura de navegação principal do produto
- Sidebar é o layout padrão — `topbar` apenas para produtos sem hierarquia lateral
- Apenas um NavItem pode estar `active` por vez
- NavPrimary deve ter entre 3 e 8 itens — mais que 8 indica necessidade de reorganização
- NavSecondary (configurações, ajuda) fica no NavFooter, separado visualmente
- A `sidebar-collapsed` mantém apenas ícones — labels em tooltip no hover

---

## Anti-patterns

- **PROIBIDO**: dois NavItems com estado `active` simultaneamente
- **PROIBIDO**: NavLabel com texto maior que 20 caracteres
- **PROIBIDO**: usar Navbar como container de conteúdo — é estrutura de navegação apenas
- **PROIBIDO**: omitir ícone nos NavItems — ícone é obrigatório para reconhecimento visual
- **PROIBIDO**: remover o NavFooter com AccountSwitch

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
