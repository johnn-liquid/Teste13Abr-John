# Componente — Structured Inputs

## Nome canônico
`StructuredInputs`

## Descrição
Conjunto de inputs com estrutura definida apresentados dentro do chat quando o agente solicita dados do usuário de forma estruturada (formulários inline, seletores, escalas). Diferente do UserInput (texto livre), o StructuredInputs guia a entrada com campos pré-definidos.

---

## Anatomia

```
StructuredInputs
├── FormHeader              ← título da solicitação + descrição opcional
├── FieldList               ← lista de campos
│   ├── TextField           ← input de texto simples
│   ├── SelectField         ← dropdown de opções
│   ├── MultiSelectField    ← seleção múltipla com chips
│   ├── DateField           ← seletor de data
│   ├── RangeField          ← slider numérico
│   ├── RadioGroup          ← escolha exclusiva com labels
│   └── CheckboxGroup       ← escolha múltipla com labels
└── FormActions             ← botão de Submit + botão Cancel (opcional)
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `inline` | Formulário diretamente na thread do chat |
| `card` | Formulário dentro de um card elevado (`--card`) |
| `minimal` | Apenas os campos, sem FormHeader ou ações explícitas |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `idle` | Campos editáveis, Submit desabilitado se campos obrigatórios vazios |
| `filled` | Campos preenchidos, Submit habilitado |
| `submitting` | Submit com spinner, campos readonly |
| `error` | Campo com erro: borda `--destructive`, mensagem inline abaixo |
| `success` | Formulário substituído por mensagem de confirmação (Text role=system) |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `inline \| card \| minimal` | `inline` | Layout do container |
| `title` | `string` | — | Título da solicitação |
| `description` | `string` | — | Instrução adicional |
| `fields` | `Field[]` | obrigatório | Lista de campos definidos |
| `onSubmit` | `function` | obrigatório | Callback com dados do formulário |
| `onCancel` | `function` | — | Se presente, exibe botão Cancel |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background (inline) | transparente |
| Background (card) | `--card` |
| Título | `--foreground` + `label-1` |
| Descrição | `--muted-foreground` + `body-md` |
| Label do campo | `--foreground` + `label-2` |
| Input do campo | `--input` background + `--border` borda |
| Input em focus | `--ring` borda |
| Mensagem de erro | `--destructive` + `body-sm` |
| Botão Submit | `--primary` |
| Botão Cancel | outline `--border` |
| Radius dos inputs | `--radius-md` |
| Gap entre campos | `--space-4` |
| Radius do card | `--radius-lg` |
| Padding do card | `--space-4` |

---

## Regras de uso

- **Use StructuredInputs** quando o agente precisa de dados estruturados antes de prosseguir
- Aparecem como resposta do agente na thread, não como modal externo
- Campos obrigatórios marcados com `*` após o label
- Validação inline: erros aparecem abaixo do campo em `body-sm` `--destructive`
- Após submit, o formulário é substituído por uma Text role=system confirmando o envio

---

## Anti-patterns

- **PROIBIDO**: usar StructuredInputs para formulários de configuração fora do chat — use Form Flow
- **PROIBIDO**: mais de 5 campos em um StructuredInputs inline — divida em múltiplos passos
- **PROIBIDO**: Submit habilitado com campos obrigatórios vazios
- **PROIBIDO**: abrir modal para o usuário preencher dados que cabem inline
- **PROIBIDO**: manter o formulário visível após o submit — substitua por confirmação

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
