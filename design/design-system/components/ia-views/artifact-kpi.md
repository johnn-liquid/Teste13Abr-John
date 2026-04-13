# Componente — Artifact KPI

## Nome canônico
`ArtifactKPI`

## Descrição
Indicador de métrica chave gerado pela IA. Para exibir um valor numérico principal com contexto (variação, período, comparação). Comunica performance de forma imediata e destaque visual.

---

## Anatomia

```
ArtifactKPI
├── ArtifactHeader          ← label "KPI" + ações
├── KPIContent
│   ├── KPILabel            ← nome da métrica
│   ├── KPIValue            ← valor principal (destaque máximo)
│   ├── KPIVariation        ← variação (↑ +12% vs período anterior)
│   └── KPIPeriod           ← período de referência
└── KPIChart (opcional)     ← sparkline de tendência
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `single` | Um KPI por card |
| `group` | Grade de 2–4 KPIs lado a lado |

---

## Dados obrigatórios vs opcionais

| Campo | Obrigatoriedade |
|---|---|
| `label` | Obrigatório |
| `value` | Obrigatório |
| `period` | Recomendado |
| `variation` | Opcional |
| `sparkline` | Opcional |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `loading` | Skeleton do KPIValue (retângulo largo) + KPILabel |
| `populated` | KPI completo com valor |
| `error` | "—" no KPIValue, `--muted-foreground` + tooltip de erro |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `label` | `string` | obrigatório | Nome da métrica |
| `value` | `string \| number` | obrigatório | Valor principal formatado |
| `variation` | `{ value: number, direction: 'up' \| 'down' \| 'neutral' }` | — | Variação percentual |
| `period` | `string` | — | Período de referência |
| `sparklineData` | `number[]` | — | Dados para sparkline |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background | `--card` |
| Borda | `--border` |
| Shadow | `--shadow-sm` |
| KPILabel | `--muted-foreground` + `label-2` |
| KPIValue | `--card-foreground` + `heading-h4` (Geist Bold 24px) |
| Variação positiva (up) | `--chart-2` (azul-verde) |
| Variação negativa (down) | `--destructive` |
| Variação neutra | `--muted-foreground` |
| KPIPeriod | `--muted-foreground` + `body-sm` |
| Sparkline | `--chart-1` |
| Radius | `--radius-lg` |
| Padding | `--space-4` |

---

## Comportamento dentro do Chat

- KPI single: largura máxima 240px
- KPI group: grade 2×N, largura completa do container de mensagem
- Sparkline tem altura máxima de 48px para não competir com o KPIValue

---

## Anti-patterns

- **PROIBIDO**: mais de 4 KPIs no grupo (prejudica hierarquia visual)
- **PROIBIDO**: KPIValue sem formatação (ex: "1234567" → deve ser "1.234.567")
- **PROIBIDO**: variação sem indicador de direção (seta ou cor)
- **PROIBIDO**: usar ArtifactKPI para textos ou valores não numéricos — use ArtifactCard

## Snippet de código — shadcn/ui

Base shadcn/ui — usa Card como container do KPI.

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
