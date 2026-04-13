# Componente — Artifact Datatable

## Nome canônico
`ArtifactDatatable`

## Descrição
Tabela de dados estruturados gerada pela IA. Para exibição de múltiplos registros com colunas definidas, ordenação, e ações por linha.

---

## Anatomia

```
ArtifactDatatable
├── ArtifactHeader          ← label + título + ações globais
│   ├── TableTitle
│   └── ArtifactActions     ← exportar CSV, copiar, expandir
├── TableControls           ← busca inline + filtros + contagem de linhas
├── TableContainer          ← área rolável horizontalmente
│   └── Table
│       ├── TableHeader     ← cabeçalho com labels + ordenação
│       │   └── HeaderCell[]
│       └── TableBody
│           └── TableRow[]
│               └── TableCell[]
├── TablePagination         ← navegação por páginas
└── TableFooter             ← total de registros
```

---

## Dados obrigatórios vs opcionais

| Campo | Obrigatoriedade |
|---|---|
| `columns` | Obrigatório |
| `rows` | Obrigatório |
| `title` | Recomendado |
| `sortable columns` | Opcional |
| `row actions` | Opcional |
| `pagination` | Obrigatório quando rows > 10 |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `loading` | Skeleton com 5 linhas de placeholder |
| `populated` | Tabela com dados |
| `empty` | "Nenhum resultado encontrado" centralizado |
| `error` | Banner inline "Erro ao carregar dados" + retry |
| `filtered-empty` | "Nenhum resultado para o filtro aplicado" + limpar filtro |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `title` | `string` | — | Título da tabela |
| `columns` | `Column[]` | obrigatório | Definição de colunas |
| `rows` | `Row[]` | obrigatório | Dados das linhas |
| `pageSize` | `number` | `10` | Itens por página |
| `sortable` | `boolean` | `true` | Habilita ordenação por coluna |
| `searchable` | `boolean` | `false` | Exibe campo de busca |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background container | `--card` |
| Background header | `--muted` |
| Background row (default) | transparente |
| Background row (hover) | `--accent` |
| Background row (selected) | `--accent` |
| Texto header | `--muted-foreground` + `label-3` |
| Texto cell | `--card-foreground` + `body-md` |
| Borda entre linhas | `--border` |
| Borda container | `--border` |
| Radius container | `--radius-lg` |
| Padding cell | `--space-3` (vertical) + `--space-4` (horizontal) |
| Ícone de sort | `--muted-foreground` |
| Ícone de sort ativo | `--foreground` |

---

## Comportamento dentro do Chat

- Máximo de 5 colunas visíveis sem scroll horizontal no chat
- Mais de 5 colunas: scroll horizontal habilitado automaticamente
- Altura máxima antes de scroll vertical: 400px
- ArtifactActions sempre visíveis

---

## Anti-patterns

- **PROIBIDO**: tabela sem paginação quando rows > 10
- **PROIBIDO**: scroll horizontal da página para visualizar tabela — scroll deve ser interno
- **PROIBIDO**: colunas sem label no HeaderCell
- **PROIBIDO**: usar ArtifactDatatable para dado único — use ArtifactCard
- **PROIBIDO**: ordenação que recarrega dados sem indicação de loading

## Snippet de código — shadcn/ui

Base shadcn/ui — usa Table, TableHeader, TableBody, TableRow, TableHead, TableCell como estrutura.

```tsx
"use client"

import * as React from "react"

import { cn } from "@/lib/utils"

function Table({ className, ...props }: React.ComponentProps<"table">) {
  return (
    <div
      data-slot="table-container"
      className="relative w-full overflow-x-auto"
    >
      <table
        data-slot="table"
        className={cn("w-full caption-bottom text-sm", className)}
        {...props}
      />
    </div>
  )
}

function TableHeader({ className, ...props }: React.ComponentProps<"thead">) {
  return (
    <thead
      data-slot="table-header"
      className={cn("[&_tr]:border-b", className)}
      {...props}
    />
  )
}

function TableBody({ className, ...props }: React.ComponentProps<"tbody">) {
  return (
    <tbody
      data-slot="table-body"
      className={cn("[&_tr:last-child]:border-0", className)}
      {...props}
    />
  )
}

function TableFooter({ className, ...props }: React.ComponentProps<"tfoot">) {
  return (
    <tfoot
      data-slot="table-footer"
      className={cn(
        "border-t bg-muted/50 font-medium [&>tr]:last:border-b-0",
        className
      )}
      {...props}
    />
  )
}

function TableRow({ className, ...props }: React.ComponentProps<"tr">) {
  return (
    <tr
      data-slot="table-row"
      className={cn(
        "border-b transition-colors hover:bg-muted/50 has-aria-expanded:bg-muted/50 data-[state=selected]:bg-muted",
        className
      )}
      {...props}
    />
  )
}

function TableHead({ className, ...props }: React.ComponentProps<"th">) {
  return (
    <th
      data-slot="table-head"
      className={cn(
        "h-10 px-2 text-left align-middle font-medium whitespace-nowrap text-foreground [&:has([role=checkbox])]:pr-0 [&>[role=checkbox]]:translate-y-[2px]",
        className
      )}
      {...props}
    />
  )
}

function TableCell({ className, ...props }: React.ComponentProps<"td">) {
  return (
    <td
      data-slot="table-cell"
      className={cn(
        "p-2 align-middle whitespace-nowrap [&:has([role=checkbox])]:pr-0 [&>[role=checkbox]]:translate-y-[2px]",
        className
      )}
      {...props}
    />
  )
}

function TableCaption({
  className,
  ...props
}: React.ComponentProps<"caption">) {
  return (
    <caption
      data-slot="table-caption"
      className={cn("mt-4 text-sm text-muted-foreground", className)}
      {...props}
    />
  )
}

export {
  Table,
  TableHeader,
  TableBody,
  TableFooter,
  TableHead,
  TableRow,
  TableCell,
  TableCaption,
}
```
