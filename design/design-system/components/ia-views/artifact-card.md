# Componente — Artifact Card

## Nome canônico
`ArtifactCard`

## Descrição
Exibição de dado estruturado em formato card gerado pela IA. Usado quando o agente precisa apresentar uma entidade individual com múltiplos atributos de forma visual e organizada.

---

## Anatomia

```
ArtifactCard
├── ArtifactHeader          ← label "Artefato" + tipo + ações
│   ├── ArtifactTypeLabel   ← "Card" em label-3 muted
│   └── ArtifactActions     ← copiar, expandir, exportar
├── CardContent             ← conteúdo principal
│   ├── CardTitle           ← título da entidade
│   ├── CardSubtitle        ← subtítulo ou categoria
│   ├── AttributeList       ← lista de pares chave:valor
│   │   └── AttributeItem[]
│   │       ├── AttributeKey
│   │       └── AttributeValue
│   └── CardImage (opcional) ← imagem associada
└── CardFooter (opcional)   ← ações contextuais da entidade
```

---

## Dados obrigatórios vs opcionais

| Campo | Obrigatoriedade |
|---|---|
| `title` | Obrigatório |
| `attributes` | Obrigatório (mínimo 1) |
| `subtitle` | Opcional |
| `image` | Opcional |
| `footer actions` | Opcional |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `loading` | Skeleton do card — título, 3 atributos como placeholders |
| `populated` | Card com todos os dados preenchidos |
| `error` | Mensagem "Não foi possível carregar este card" + retry |
| `empty` | Sem conteúdo após query — "Nenhum dado disponível" |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `title` | `string` | obrigatório | Título da entidade |
| `subtitle` | `string` | — | Subtítulo ou categoria |
| `attributes` | `{ key: string, value: string }[]` | obrigatório | Pares de atributos |
| `image` | `string` | — | URL da imagem |
| `actions` | `Action[]` | — | Ações no footer |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background | `--card` |
| Borda | `--border` |
| Shadow | `--shadow-sm` |
| CardTitle | `--card-foreground` + `heading-card` |
| CardSubtitle | `--muted-foreground` + `body-md` |
| AttributeKey | `--muted-foreground` + `label-3` |
| AttributeValue | `--card-foreground` + `body-md` |
| ArtifactTypeLabel | `--muted-foreground` + `label-3` |
| Radius | `--radius-lg` |
| Padding | `--space-4` |
| Gap atributos | `--space-2` |

---

## Comportamento dentro do Chat

- Renderizado diretamente na MessageList após mensagem do agente
- Máximo de 6 atributos visíveis — excedente colapsado com "Ver mais"
- Largura máxima: 480px (mesma do container de mensagem)
- ArtifactActions visíveis sempre (não apenas no hover) para cards no chat

---

## Anti-patterns

- **PROIBIDO**: usar ArtifactCard para listas de múltiplos itens — use ArtifactList
- **PROIBIDO**: mais de 10 atributos sem colapso
- **PROIBIDO**: valores de atributo como listas longas — use ArtifactDocument para textos longos
- **PROIBIDO**: ArtifactCard sem título

## Snippet de código — shadcn/ui

Base shadcn/ui — usa Card, CardHeader, CardContent, CardFooter como estrutura.

```tsx
import * as React from "react"

import { cn } from "@/lib/utils"

function Card({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card"
      className={cn(
        "flex flex-col gap-6 rounded-xl border bg-card py-6 text-card-foreground shadow-sm",
        className
      )}
      {...props}
    />
  )
}

function CardHeader({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-header"
      className={cn(
        "@container/card-header grid auto-rows-min grid-rows-[auto_auto] items-start gap-2 px-6 has-data-[slot=card-action]:grid-cols-[1fr_auto] [.border-b]:pb-6",
        className
      )}
      {...props}
    />
  )
}

function CardTitle({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-title"
      className={cn("leading-none font-semibold", className)}
      {...props}
    />
  )
}

function CardDescription({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-description"
      className={cn("text-sm text-muted-foreground", className)}
      {...props}
    />
  )
}

function CardAction({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-action"
      className={cn(
        "col-start-2 row-span-2 row-start-1 self-start justify-self-end",
        className
      )}
      {...props}
    />
  )
}

function CardContent({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-content"
      className={cn("px-6", className)}
      {...props}
    />
  )
}

function CardFooter({ className, ...props }: React.ComponentProps<"div">) {
  return (
    <div
      data-slot="card-footer"
      className={cn("flex items-center px-6 [.border-t]:pt-6", className)}
      {...props}
    />
  )
}

export {
  Card,
  CardHeader,
  CardFooter,
  CardTitle,
  CardAction,
  CardDescription,
  CardContent,
}
```
