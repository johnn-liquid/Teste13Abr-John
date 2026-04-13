# Componente — Input

## Nome canônico
`Input`

## Descrição
Campo de entrada de texto base. Baseado em shadcn/ui input. Usado para qualquer texto editável — login, busca, formulários. Sempre usar label visível. States: default, focus, error, disabled.

---

## Anatomia

```
Input
├── [Label] (opcional, acima)
├── Wrapper
│   ├── [PrefixIcon] (opcional, à esquerda)
│   └── InputField
│       └── [Placeholder]
│   └── [SuffixIcon] (opcional, à direita)
├── [HelperText] (opcional, abaixo)
└── [ErrorMessage] (substitui HelperText quando error)
```

---

## Tipos

| Tipo | shadcn | Descrição |
|---|---|---|
| `text` | text | Texto livre |
| `email` | email | Validação de e-mail nativa |
| `password` | password | Texto oculto, toggle de visibilidade |
| `search` | search | Campo de busca com ícone de lupa, clear button |
| `number` | number | Apenas números, controles de incremento |
| `tel` | tel | Telefone |
| `url` | url | URL |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `default` | Borda `--input`, fundo `--background` |
| `focus` | Borda `--ring`, sombra `--shadow-sm` |
| `error` | Borda `--destructive`, mensagem de erro abaixo |
| `disabled` | Opacidade 0.5, cursor not-allowed, valor não editável |
| `readonly` | Fundo `--muted`, cursor default |

---

## Tokens aplicados

| Região | Token | Valor |
|---|---|---|
| Background | `--background` | fundo base |
| Border (default) | `--input` | borda do campo |
| Border (focus) | `--ring` | borda de foco |
| Border (error) | `--destructive` | borda de erro |
| Text | `--foreground` | texto digitado |
| Placeholder | `--muted-foreground` | placeholder |
| Label | `--foreground` | label acima |
| Helper/Error | `--muted-foreground` / `--destructive` | texto de suporte |
| Radius | `--radius-md` | arredondamento |
| Padding (vertical) | `--space-3` | espaçamento interno |
| Padding (horizontal) | `--space-3` | espaçamento interno |
| Focus shadow | `--shadow-sm` | sombra de foco |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `type` | `text \| email \| password \| search \| number \| tel \| url` | `text` | Tipo do input |
| `label` | `string` | — | Label visível acima do campo |
| `placeholder` | `string` | — | Texto placeholder |
| `value` | `string` | — | Valor controlado |
| `defaultValue` | `string` | — | Valor inicial (não controlado) |
| `disabled` | `boolean` | `false` | Desabilitado |
| `readonly` | `boolean` | `false` | Somente leitura |
| `error` | `string` | — | Mensagem de erro (ativa estado error) |
| `helperText` | `string` | — | Texto de suporte |
| `prefixIcon` | `ReactNode` | — | Ícone à esquerda |
| `suffixIcon` | `ReactNode` | — | Ícone à direita |
| `required` | `boolean` | `false` | Indica campo obrigatório |
| `id` | `string` | auto | ID para accessibility |

---

## Regras de uso

1. **Label é obrigatório** — exceto em campos de busca isolados (SearchInput no FilterBar)
2. **Error substitui HelperText** — quando `error` tem valor, `helperText` não é exibido
3. **Search tem clear button** — tipo `search` deve ter botão para limpar o valor
4. **Password tem toggle** — tipo `password` deve ter ícone para mostrar/ocultar
5. **ID para accessibility** — `id` deve ser único, usado em `htmlFor` do label e `aria-describedby`
6. **ErrorMessage abaixo do campo** — usa `--destructive`, fonte `--text-sm`
7. **Prefix/Suffix icons** — não são clicáveis por padrão (para isso usar Input com Button anexado)

---

## Exemplo de uso

```jsx
// Text input com label
<Input
  type="text"
  label="Nome do agente"
  placeholder="Digite o nome"
  value={name}
  onChange={(e) => setName(e.target.value)}
/>

// Search input
<Input
  type="search"
  placeholder="Buscar agentes..."
  prefixIcon={<SearchIcon />}
  value={query}
  onChange={(e) => setQuery(e.target.value)}
/>

// Input com erro
<Input
  type="email"
  label="E-mail"
  placeholder="seu@email.com"
  error="E-mail inválido"
  value={email}
/>

// Input com helper text
<Input
  type="password"
  label="Senha"
  helperText="Mínimo 8 caracteres"
  required
/>
```

---

## Composição

Input é composto por primitives do shadcn. Para variações compostas:

```
Input + Label = FormField (usar em formulários)
Input + prefix/suffix = Input com ícones
Input + Button (suffix) = InputGroup
```

## shadcn/ui

**Fonte:** [shadcn/ui Input](https://ui.shadcn.com/docs/components/input)

**Instalação:**
```bash
pnpm dlx shadcn@latest add input
```

**Import:**
```tsx
import { Input } from "@/components/ui/input"
```

---

## Customização de Tokens

Para garantir que o componente siga a identidade visual do Design System, as variáveis CSS nativas do ShadCN devem ser mapeadas da seguinte forma no `globals.css` ou via classes utilitárias:

| Variável ShadCN | Token do DS | Papel |
|---|---|---|
| `--background` | `var(--background)` | Fundo base do input |
| `--input` | `var(--input)` | Borda e background do campo |
| `--ring` | `var(--ring)` | Cor do anel de foco |
| `--foreground` | `var(--foreground)` | Cor do texto digitado |
| `--muted-foreground` | `var(--muted-foreground)` | Cor do placeholder |
| `--radius` | `var(--radius-md)` | Arredondamento padrão |

---

## Exemplo de Código (Uso Real)

```tsx
import * as React from "react"

import { cn } from "@/lib/utils"

function Input({ className, type, ...props }: React.ComponentProps<"input">) {
  return (
    <input
      type={type}
      data-slot="input"
      className={cn(
        "h-9 w-full min-w-0 rounded-md border border-input bg-transparent px-3 py-1 text-base shadow-xs transition-[color,box-shadow] outline-none selection:bg-primary selection:text-primary-foreground file:inline-flex file:h-7 file:border-0 file:bg-transparent file:text-sm file:font-medium file:text-foreground placeholder:text-muted-foreground disabled:pointer-events-none disabled:cursor-not-allowed disabled:opacity-50 md:text-sm dark:bg-input/30",
        "focus-visible:border-ring focus-visible:ring-[3px] focus-visible:ring-ring/50",
        "aria-invalid:border-destructive aria-invalid:ring-destructive/20 dark:aria-invalid:ring-destructive/40",
        className
      )}
      {...props}
    />
  )
}

export { Input }
```
