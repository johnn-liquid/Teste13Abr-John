# Componente — Artifact Document

## Nome canônico
`ArtifactDocument`

## Descrição
Documento textual gerado pela IA. Para outputs de texto longo estruturado — relatórios, resumos, e-mails, código, análises. Suporta markdown completo e ações de exportação.

---

## Anatomia

```
ArtifactDocument
├── ArtifactHeader          ← label "Documento" + título + ações
│   ├── DocumentTitle
│   └── ArtifactActions     ← copiar, exportar (MD, PDF, DOCX), expandir
├── DocumentContent         ← conteúdo com markdown renderizado
│   ├── HeadingBlocks       ← H1, H2, H3
│   ├── ParagraphBlocks     ← texto corrido
│   ├── CodeBlocks          ← blocos de código com highlight
│   ├── ListBlocks          ← listas ordenadas e não-ordenadas
│   └── TableBlocks         ← tabelas markdown
└── DocumentFooter          ← contagem de palavras + timestamp
```

---

## Dados obrigatórios vs opcionais

| Campo | Obrigatoriedade |
|---|---|
| `content` | Obrigatório |
| `title` | Recomendado |
| `wordCount` | Opcional |
| `exportFormats` | Opcional |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `streaming` | Texto aparece progressivamente, cursor piscante ao final |
| `populated` | Documento completo com renderização markdown |
| `loading` | Skeleton de parágrafos |
| `error` | "Erro ao gerar documento" + retry |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `title` | `string` | — | Título do documento |
| `content` | `string` | obrigatório | Conteúdo em markdown |
| `streaming` | `boolean` | `false` | Modo de geração progressiva |
| `exportFormats` | `('md' \| 'pdf' \| 'docx')[]` | `['md']` | Formatos disponíveis para export |
| `maxHeight` | `number \| 'auto'` | `480` | Altura máxima antes de scroll |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background | `--card` |
| Borda | `--border` |
| Shadow | `--shadow-sm` |
| DocumentTitle | `--card-foreground` + `heading-card` |
| H1 no conteúdo | `--card-foreground` + `heading-section` |
| H2 no conteúdo | `--card-foreground` + `heading-card` |
| Texto corrido | `--card-foreground` + `body-lg` |
| Código inline | `--muted` background + `--foreground` texto + `code-base` |
| Bloco de código | `--muted` background + `--border` borda |
| Link | `--primary` com underline |
| Cursor streaming | `--primary` |
| Radius | `--radius-lg` |
| Padding conteúdo | `--space-5` |

---

## Comportamento dentro do Chat

- Altura máxima de 480px — documento maior ativa scroll interno
- Botão "Expandir" abre em modal fullscreen para leitura
- Durante streaming, scroll automático acompanha o fim do texto

---

## Anti-patterns

- **PROIBIDO**: documento sem scroll quando conteúdo excede 480px
- **PROIBIDO**: renderizar HTML arbitrário do conteúdo — somente markdown sanitizado
- **PROIBIDO**: usar ArtifactDocument para dados tabulares — use ArtifactDatatable
- **PROIBIDO**: código inline misturado em texto sem formatação `code-base`

## Snippet de código — shadcn/ui

Base shadcn/ui — usa ScrollArea como container de conteúdo.

```tsx
"use client"

import * as React from "react"
import { ScrollArea as ScrollAreaPrimitive } from "radix-ui"

import { cn } from "@/lib/utils"

function ScrollArea({
  className,
  children,
  ...props
}: React.ComponentProps<typeof ScrollAreaPrimitive.Root>) {
  return (
    <ScrollAreaPrimitive.Root
      data-slot="scroll-area"
      className={cn("relative", className)}
      {...props}
    >
      <ScrollAreaPrimitive.Viewport
        data-slot="scroll-area-viewport"
        className="size-full rounded-[inherit] transition-[color,box-shadow] outline-none focus-visible:ring-[3px] focus-visible:ring-ring/50 focus-visible:outline-1"
      >
        {children}
      </ScrollAreaPrimitive.Viewport>
      <ScrollBar />
      <ScrollAreaPrimitive.Corner />
    </ScrollAreaPrimitive.Root>
  )
}

function ScrollBar({
  className,
  orientation = "vertical",
  ...props
}: React.ComponentProps<typeof ScrollAreaPrimitive.ScrollAreaScrollbar>) {
  return (
    <ScrollAreaPrimitive.ScrollAreaScrollbar
      data-slot="scroll-area-scrollbar"
      orientation={orientation}
      className={cn(
        "flex touch-none p-px transition-colors select-none",
        orientation === "vertical" &&
          "h-full w-2.5 border-l border-l-transparent",
        orientation === "horizontal" &&
          "h-2.5 flex-col border-t border-t-transparent",
        className
      )}
      {...props}
    >
      <ScrollAreaPrimitive.ScrollAreaThumb
        data-slot="scroll-area-thumb"
        className="relative flex-1 rounded-full bg-border"
      />
    </ScrollAreaPrimitive.ScrollAreaScrollbar>
  )
}

export { ScrollArea, ScrollBar }
```
