# Componente — New Task

## Nome canônico
`NewTask`

## Descrição
Controle de criação de nova tarefa ou conversa. Botão ou área de entrada que inicia uma nova sessão de interação com o agente, limpando o contexto atual.

---

## Anatomia

```
NewTask
├── TriggerButton         ← botão principal de criação
│   ├── PlusIcon
│   └── Label ("Nova conversa")
└── QuickStartMenu (opcional)   ← dropdown com templates de início rápido
    └── TemplateItem[]
        ├── TemplateIcon
        └── TemplateLabel
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `button` | Botão padrão na Navbar |
| `inline` | Botão inline dentro do chat (após fim de conversa) |
| `with-templates` | Botão que abre QuickStartMenu com templates |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `default` | Botão com PlusIcon + label |
| `hover` | Background `--accent`, transição `--duration-fast` |
| `loading` | Spinner no lugar do PlusIcon durante criação |
| `menu-open` | QuickStartMenu visível |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `button \| inline \| with-templates` | `button` | Tipo do controle |
| `label` | `string` | `"Nova conversa"` | Texto do botão |
| `templates` | `Template[]` | — | Itens do QuickStartMenu |
| `onNew` | `function` | obrigatório | Callback de criação |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background TriggerButton | `--secondary` |
| Texto TriggerButton | `--secondary-foreground` + `label-2` |
| Hover TriggerButton | `--accent` |
| Background QuickStartMenu | `--popover` |
| Borda QuickStartMenu | `--border` |
| Sombra QuickStartMenu | `--shadow-md` |
| Radius TriggerButton | `--radius-md` |
| Radius QuickStartMenu | `--radius-lg` |

---

## Regras de uso

- **Use NewTask** no NavHeader ou NavPrimary da Navbar como ação global
- O botão deve ser visualmente distinto dos NavItems — usa `--secondary`, não `--sidebar-primary`
- Na variante `inline`, aparece ao final de uma conversa concluída como CTA

---

## Anti-patterns

- **PROIBIDO**: usar NewTask como botão primário da tela (é ação de navegação, não ação de conteúdo)
- **PROIBIDO**: label com mais de 20 caracteres
- **PROIBIDO**: abrir nova tarefa sem confirmar se a conversa atual tem conteúdo não salvo

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
