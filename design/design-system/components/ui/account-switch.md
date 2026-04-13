# Componente — Account Switch

## Nome canônico
`AccountSwitch`

## Descrição
Controle de troca de conta ou workspace. Exibido no NavFooter da Navbar, permite ao usuário alternar entre diferentes contextos (workspaces, organizações) e acessar configurações de conta.

---

## Anatomia

```
AccountSwitch
├── CurrentAccount          ← exibe conta/workspace ativo
│   ├── AccountAvatar       ← avatar ou iniciais
│   ├── AccountName         ← nome do workspace/conta
│   └── ChevronIcon         ← indica que é clicável
└── AccountMenu (dropdown)  ← aberto ao clicar
    ├── AccountList         ← lista de workspaces disponíveis
    │   └── AccountItem[]   ← item de workspace
    │       ├── WorkspaceIcon
    │       ├── WorkspaceName
    │       └── ActiveIndicator (se for o atual)
    ├── Divider
    └── AccountActions      ← configurações, logout
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `expanded` | Avatar + nome + chevron — usado em sidebar expandida |
| `collapsed` | Apenas avatar — usado em sidebar colapsada, com tooltip |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `default` | CurrentAccount visível, AccountMenu fechado |
| `hover` | Background `--sidebar-accent` no CurrentAccount |
| `open` | AccountMenu visível como dropdown, ChevronIcon rotacionado 180° |
| `switching` | AccountItem com spinner, interações bloqueadas durante a troca |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `expanded \| collapsed` | `expanded` | Modo de exibição |
| `currentAccount` | `{ name: string, avatar?: string }` | obrigatório | Conta/workspace ativo |
| `accounts` | `Account[]` | — | Lista de workspaces para troca |
| `onSwitch` | `function(accountId)` | obrigatório se `accounts` presente | Callback de troca |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background CurrentAccount | transparente → `--sidebar-accent` (hover) |
| Nome da conta | `--sidebar-foreground` + `label-2` |
| ChevronIcon | `--sidebar-foreground` |
| Background AccountMenu | `--popover` |
| Borda AccountMenu | `--border` |
| Sombra AccountMenu | `--shadow-lg` |
| WorkspaceName | `--popover-foreground` + `body-md` |
| ActiveIndicator | `--primary` |
| Radius CurrentAccount | `--radius-md` |
| Radius AccountMenu | `--radius-lg` |
| Padding AccountItem | `--space-2` (vertical) + `--space-3` (horizontal) |
| Transição open | `--duration-normal` + `--ease-out` |

---

## Regras de uso

- **Use AccountSwitch** exclusivamente no NavFooter da Navbar
- O AccountMenu abre como dropdown posicionado acima do CurrentAccount (direção: up)
- Ao trocar de workspace, a aplicação deve indicar loading antes de recarregar
- Máximo de 10 workspaces na AccountList — se houver mais, adicionar busca inline

---

## Anti-patterns

- **PROIBIDO**: usar AccountSwitch fora da Navbar
- **PROIBIDO**: AccountMenu que abre para baixo (ficaria fora da viewport em sidebar)
- **PROIBIDO**: omitir ActiveIndicator no workspace atual
- **PROIBIDO**: AccountItem sem nome legível (apenas ícone)

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
