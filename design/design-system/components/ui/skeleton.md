# Componente — Skeleton

## Nome canônico
`Skeleton`

## Descrição
Placeholder animado para estados de loading. Baseado em shadcn/ui skeleton. Usado para substituir conteúdo que ainda está carregando — nunca spinner de página inteira para dados parciais. Animação: pulse suave de opacity 1 → 0.5 → 1.

---

## Anatomia

```
Skeleton
└── [children] ou wrapper implícito
    ├── Skeleton (variante text) — linha de texto
    ├── Skeleton (variante circle) — círculo/avatar
    └── Skeleton (variante rect) — retângulo genérico
```

---

## Variantes

| Variante | shadcn | Descrição | Uso típico |
|---|---|---|---|
| `text` | — | Linha de texto | Linhas de parágrafo, descrições |
| `circle` | — | Círculo perfeito | Avatar, ícone circular |
| `rect` | — | Retângulo | Cards, imagens, containers |
| `card` | — | Retângulo com sub-elementos | Card completo com título e texto |

---

## Tamanhos

| Tamanho | Dimensões | Uso |
|---|---|---|
| `xs` | width: 32px, height: 32px (circle) ou 8px (text) | Avatares pequenos |
| `sm` | width: 48px, height: 48px (circle) ou 12px (text) | Avatares, badges |
| `md` | width: 64px, height: 64px (circle) ou 16px (text) | Avatares médios |
| `lg` | width: 96px, height: 96px (circle) ou 20px (text) | Avatares grandes |
| `full` | width/height 100% do container | Textos e cards |

---

## Estados

| Estado | Comportamento visual |
|---|---|
| `loading` | Skeleton visível com animação pulse |
| `idle` | Skeleton invisível (não renderizar) |

---

## Tokens aplicados

| Região | Token | Valor |
|---|---|---|
| Background | `--muted` | cor do placeholder |
| Text line-height | `--text-sm` line-height | altura de linha para skeleton text |
| Radius (text) | `--radius-sm` | levemente arredondado |
| Radius (circle) | `--radius-full` | circular |
| Radius (rect/card) | `--radius-lg` | igual a cards |
| Animation | `opacity: 1 → 0.5 → 1` | pulse suave |
| Animation duration | `--duration-slow` (500ms) | transição lenta para leitura |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `text \| circle \| rect \| card` | `rect` | Variante visual |
| `size` | `xs \| sm \| md \| lg \| full` | `md` | Tamanho |
| `width` | `string \| number` | — | Largura customizada |
| `height` | `string \| number` | — | Altura customizada |
| `className` | `string` | — | Classes adicionais |
| `animated` | `boolean` | `true` | Ativa/desativa animação |

---

## Regras de uso

1. **Skeleton não substitui Spinner** — Skeleton para conteúdo inline; Spinner para ações em processamento
2. **Quantidade realista** — mostrar 3-5 linhas de skeleton para texto, não 10
3. **Skeleton em Card** — usar `variant="card"` com placeholders de título + 2-3 linhas
4. **Skeleton em TableRow** — 1 Skeleton rect por célula, manter estrutura da tabela
5. **Avatar skeleton** — usar `variant="circle"` + `size` correspondente
6. **Animation** — respeitar `prefers-reduced-motion` (desativar animação se preferir)
7. **Skeleton não é interativo** — nunca usar Skeleton em elementos clicáveis

---

## Exemplo de uso

```jsx
// Skeleton de texto
<Skeleton variant="text" width="80%" />
<Skeleton variant="text" width="60%" />

// Skeleton de avatar
<Skeleton variant="circle" size="md" />

// Skeleton de card
<Skeleton variant="card" />

// Skeleton de tabela
<table>
  <tr>
    <td><Skeleton variant="circle" size="sm" /></td>
    <td><Skeleton variant="text" width="60%" /></td>
    <td><Skeleton variant="text" width="40%" /></td>
  </tr>
</table>

// Composição de Card com skeleton
<Card>
  <CardHeader>
    <Skeleton variant="circle" size="sm" />
    <Skeleton variant="text" width="50%" />
  </CardHeader>
  <Skeleton variant="text" />
  <Skeleton variant="text" width="70%" />
</Card>
```

---

## shadcn/ui

**Fonte:** [shadcn/ui Skeleton](https://ui.shadcn.com/docs/components/skeleton)

**Instalação:**
```bash
pnpm dlx shadcn@latest add skeleton
```

**Import:**
```tsx
import { Skeleton } from "@/components/ui/skeleton"
```

**Exemplos:**
```tsx
<Skeleton className="h-4 w-[250px]" />
<Skeleton className="h-4 w-full" />
<Skeleton className="h-12 w-12 rounded-full" />
```
