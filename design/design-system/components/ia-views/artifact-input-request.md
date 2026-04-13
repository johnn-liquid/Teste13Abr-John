# Componente — Artifact Input Request

## Nome canônico
`ArtifactInputRequest`

## Descrição
Solicitação estruturada de input ao usuário feita pela IA. Quando o agente precisa de dados específicos para continuar, usa este artefato para comunicar claramente o que precisa e como o usuário deve fornecer.

---

## Anatomia

```
ArtifactInputRequest
├── ArtifactHeader          ← label "Solicitação" + ícone de pedido
├── RequestContent          ← conteúdo da solicitação
│   ├── RequestTitle        ← "O que preciso de você"
│   ├── RequestDescription  ← contexto e instrução em linguagem natural
│   └── RequestFields       ← campos necessários (StructuredInputs ou lista)
└── RequestActions          ← botão primário de resposta + skip opcional
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `structured` | Com campos de StructuredInputs — quando o input tem formato definido |
| `open` | Apenas descrição + UserInput livre abaixo — quando qualquer texto serve |
| `choice` | Lista de opções para o usuário escolher (Suggestions) |

---

## Dados obrigatórios vs opcionais

| Campo | Obrigatoriedade |
|---|---|
| `title` | Obrigatório |
| `description` | Obrigatório |
| `fields` | Obrigatório para variante `structured` |
| `choices` | Obrigatório para variante `choice` |
| `skippable` | Opcional (false por default) |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `pending` | Solicitação visível e interativa |
| `responded` | Artefato colapsado, substituído por Text confirmando resposta |
| `skipped` | Artefato colapsado com "Ignorado" em `--muted-foreground` |
| `expired` | Aviso "Esta solicitação expirou" em `--muted-foreground` |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `structured \| open \| choice` | `open` | Tipo de input esperado |
| `title` | `string` | obrigatório | O que o agente precisa |
| `description` | `string` | obrigatório | Contexto e instrução |
| `fields` | `Field[]` | — | Campos para variante `structured` |
| `choices` | `string[]` | — | Opções para variante `choice` |
| `skippable` | `boolean` | `false` | Permite o usuário ignorar |
| `onRespond` | `function` | obrigatório | Callback com a resposta |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background | `--card` |
| Borda esquerda (accent) | `--primary` (3px, marcador de solicitação ativa) |
| Borda container | `--border` |
| RequestTitle | `--card-foreground` + `heading-card` |
| RequestDescription | `--card-foreground` + `body-md` |
| Ícone de solicitação | `--primary` |
| Botão principal | `--primary` |
| Skip link | `--muted-foreground` + `body-sm` underline |
| Estado respondido | `--muted-foreground`, borda esquerda `--muted` |
| Radius | `--radius-lg` |
| Padding | `--space-4` |

---

## Comportamento dentro do Chat

- Borda esquerda colorida (`--primary`) indica solicitação pendente
- Após resposta, o artefato colapsa para não poluir o histórico
- Apenas uma ArtifactInputRequest pode estar `pending` por vez na thread

---

## Anti-patterns

- **PROIBIDO**: duas ArtifactInputRequest `pending` na mesma thread
- **PROIBIDO**: usar ArtifactInputRequest para confirmações destrutivas — use modal
- **PROIBIDO**: title genérico como "Dados necessários" — seja específico sobre o que precisa
- **PROIBIDO**: manter o artefato expandido após a resposta do usuário

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
