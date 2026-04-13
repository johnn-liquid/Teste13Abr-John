# Componente — Button

## Nome canônico
`Button`

## Descrição
Componente primitivo de ação. Baseado em shadcn/ui button. Disparador de eventos — qualquer ação que precise de clique passa por Button. Um único Button com `variant="default"` (primary) é permitido por viewport.

---

## Anatomia

```
Button
├── [icon] (opcional, à esquerda)
├── [children] (label)
└── [icon] (opcional, à direita)
```

---

## Variantes

| Variante | shadcn | Descrição |
|---|---|---|
| `default` | default | Ação primária. Background `--primary`, foreground escuro. **Máximo 1 por viewport.** |
| `secondary` | secondary | Ação secundária. Background `--secondary`. |
| `outline` | outline | Ação terciária. Borda `--border`, fundo transparente. |
| `ghost` | ghost | Ação mínima. Sem borda, sem fundo. Hover: `--accent`. |
| `destructive` | destructive | Ação destrutiva (delete, reset, revoke). Background `--destructive`. |
| `link` | link | Texto clicável. Sem borda, cor `--primary`, underline no hover. |

---

## Tamanhos

| Tamanho | shadcn | Height | Padding | Uso |
|---|---|---|---|---|
| `sm` | sm | 32px | `--space-2` | Elementos compactos, tabelas |
| `default` | default | 40px | `--space-4` | Padrão |
| `lg` | lg | 48px | `--space-6` | CTAs destacados |
| `icon` | icon | 40px | 0 | Botão com apenas ícone |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `default` | Estado inicial |
| `hover` | Opacidade reduz para 0.9, cursor pointer |
| `active` | Opacidade reduz para 0.8, scale 0.98 |
| `disabled` | Opacidade 0.5, cursor not-allowed, pointer-events none |
| `loading` | Spinner inline à esquerda do texto, texto com opacity 0.5, disabled |
| `focus` | Ring `--ring` ao redor do botão |

---

## Tokens aplicados

| Região | Token | Valor |
|---|---|---|
| Background (default) | `--primary` | ação primária |
| Background (secondary) | `--secondary` | ação secundária |
| Background (destructive) | `--destructive` | ação destrutiva |
| Foreground (default) | `--primary-foreground` | texto sobre primary |
| Foreground (secondary) | `--secondary-foreground` | texto sobre secondary |
| Foreground (destructive) | `--primary-foreground` | texto sobre destructive |
| Border (outline) | `--border` | borda |
| Background (ghost hover) | `--accent` | hover |
| Ring (focus) | `--ring` | outline de foco |
| Radius | `--radius-md` | arredondamento |
| Transition | `--ease-out`, `--duration-fast` | hover/active |
| Transition (loading) | `--ease-in-out`, `--duration-normal` | spinner fade |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `default \| secondary \| outline \| ghost \| destructive \| link` | `default` | Variante visual |
| `size` | `sm \| default \| lg \| icon` | `default` | Tamanho |
| `disabled` | `boolean` | `false` | Desabilitado |
| `loading` | `boolean` | `false` | Estado de loading |
| `children` | `ReactNode` | — | Label do botão |
| `leftIcon` | `ReactNode` | — | Ícone à esquerda |
| `rightIcon` | `ReactNode` | — | Ícone à direita |
| `onClick` | `() => void` | — | Handler de clique |

---

## Regras de uso

1. **Máximo 1 `variant="default"` (primary) por viewport** — regra absoluta
2. **Destructive em Modal de confirmação** — único caso onde `--destructive` pode ser `variant="default"` junto com ação primária
3. **Ghost para ações de tabela** — editar, excluir, ver em linhas de tabela
4. **Link para navegação** — usar `variant="link"` com `router.push` ou `href`
5. **Loading sempre desabilita** — botão em loading deve ter `disabled={true}`
6. **Spinner substitui ícone** — quando `loading={true}`, `leftIcon` vira Spinner
7. **Variantes de tamanho `icon`** — não usam texto, apenas ícone centralizado

---

## Exemplo de uso

```jsx
// Primary (máximo 1 por viewport)
<Button variant="default" onClick={handleCreate}>
  Criar novo
</Button>

// Secondary (ação secundária)
<Button variant="secondary" onClick={handleCancel}>
  Cancelar
</Button>

// Destructive
<Button variant="destructive" onClick={handleDelete}>
  Excluir
</Button>

// Loading
<Button variant="default" loading={isLoading} onClick={handleSubmit}>
  Salvar
</Button>

// Ghost em tabela
<Button variant="ghost" size="sm" onClick={handleEdit}>
  Editar
</Button>
```

## shadcn/ui

**Fonte:** [shadcn/ui Button](https://ui.shadcn.com/docs/components/button)

**Instalação:**
```bash
pnpm dlx shadcn@latest add button
```

**Import:**
```tsx
import { Button } from "@/components/ui/button"
```

---

## Customização de Tokens

Para garantir que o componente siga a identidade visual do Design System, as variáveis CSS nativas do ShadCN devem ser mapeadas da seguinte forma no `globals.css` ou via classes utilitárias:

| Variável ShadCN | Token do DS | Papel |
|---|---|---|
| `--primary` | `var(--primary)` | Cor de fundo da ação principal |
| `--primary-foreground` | `var(--primary-foreground)` | Texto sobre o fundo primary |
| `--secondary` | `var(--secondary)` | Cor de fundo da ação secundária |
| `--secondary-foreground` | `var(--secondary-foreground)` | Texto sobre o fundo secondary |
| `--destructive` | `var(--destructive)` | Cor de fundo de ações críticas |
| `--ring` | `var(--ring)` | Cor do anel de foco |
| `--radius` | `var(--radius-md)` | Arredondamento padrão |

---

## Exemplo de Código (Uso Real)

```tsx
import * as React from "react"
import { cva, type VariantProps } from "class-variance-authority"
import { Slot } from "radix-ui"

import { cn } from "@/lib/utils"

const buttonVariants = cva(
  "inline-flex shrink-0 items-center justify-center gap-2 rounded-md text-sm font-medium whitespace-nowrap transition-all outline-none focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50 disabled:pointer-events-none disabled:opacity-50 aria-invalid:border-destructive aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40 [&_svg]:pointer-events-none [&_svg]:shrink-0 [&_svg:not([class*='size-'])]:size-4",
  {
    variants: {
      variant: {
        default: "bg-primary text-primary-foreground hover:bg-primary/90",
        destructive:
          "bg-destructive text-white hover:bg-destructive/90 focus-visible:ring-destructive/20 dark:bg-destructive/60 dark:focus-visible:ring-destructive/40",
        outline:
          "border bg-background shadow-xs hover:bg-accent hover:text-accent-foreground dark:border-input dark:bg-input/30 dark:hover:bg-input/50",
        secondary:
          "bg-secondary text-secondary-foreground hover:bg-secondary/80",
        ghost:
          "hover:bg-accent hover:text-accent-foreground dark:hover:bg-accent/50",
        link: "text-primary underline-offset-4 hover:underline",
      },
      size: {
        default: "h-9 px-4 py-2 has-[>svg]:px-3",
        xs: "h-6 gap-1 rounded-md px-2 text-xs has-[>svg]:px-1.5 [&_svg:not([class*='size-'])]:size-3",
        sm: "h-8 gap-1.5 rounded-md px-3 has-[>svg]:px-2.5",
        lg: "h-10 rounded-md px-6 has-[>svg]:px-4",
        icon: "size-9",
        "icon-xs": "size-6 rounded-md [&_svg:not([class*='size-'])]:size-3",
        "icon-sm": "size-8",
        "icon-lg": "size-10",
      },
    },
    defaultVariants: {
      variant: "default",
      size: "default",
    },
  }
)

function Button({
  className,
  variant = "default",
  size = "default",
  asChild = false,
  ...props
}: React.ComponentProps<"button"> &
  VariantProps<typeof buttonVariants> & {
    asChild?: boolean
  }) {
  const Comp = asChild ? Slot.Root : "button"

  return (
    <Comp
      data-slot="button"
      data-variant={variant}
      data-size={size}
      className={cn(buttonVariants({ variant, size, className }))}
      {...props}
    />
  )
}

export { Button, buttonVariants }
```
