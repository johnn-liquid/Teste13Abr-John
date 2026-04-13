# Componente — Attachments

## Nome canônico
`Attachments`

## Descrição
Gerenciamento de arquivos anexados à conversa antes do envio. Exibe previews dos arquivos selecionados com opção de remoção. Aparece entre o UserInput e o conteúdo do chat quando há arquivos pendentes.

---

## Anatomia

```
Attachments
└── AttachmentList          ← lista horizontal de arquivos
    └── AttachmentItem[]    ← item individual por arquivo
        ├── FileIcon        ← ícone por tipo de arquivo
        ├── FileName        ← nome truncado do arquivo
        ├── FileSize        ← tamanho formatado (KB/MB)
        ├── ProgressBar     ← progresso de upload (quando uploading)
        └── RemoveButton    ← botão X para remover
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `preview` | Visualização de arquivos pendentes antes do envio |
| `sent` | Arquivos já enviados dentro de uma mensagem Text |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `idle` | Arquivo selecionado, aguardando envio |
| `uploading` | ProgressBar ativa, RemoveButton desabilitado |
| `uploaded` | Checkmark verde, sem ProgressBar |
| `error` | Ícone de erro `--destructive`, mensagem "Falha no upload", opção de retry |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `files` | `File[]` | obrigatório | Lista de arquivos selecionados |
| `variant` | `preview \| sent` | `preview` | Estado da lista |
| `onRemove` | `function(fileId)` | obrigatório para preview | Remove arquivo da lista |
| `maxFiles` | `number` | `5` | Máximo de arquivos simultâneos |
| `maxSizeMB` | `number` | `10` | Tamanho máximo por arquivo em MB |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background do item | `--muted` |
| Borda do item | `--border` |
| Nome do arquivo | `--foreground` + `label-2` |
| Tamanho do arquivo | `--muted-foreground` + `body-sm` |
| Ícone de arquivo | `--muted-foreground` |
| ProgressBar fill | `--primary` |
| ProgressBar track | `--border` |
| Erro | `--destructive` |
| RemoveButton | `--muted-foreground` → `--destructive` (hover) |
| Radius do item | `--radius-md` |
| Padding do item | `--space-2` (vertical) + `--space-3` (horizontal) |
| Gap entre itens | `--space-2` |

---

## Regras de uso

- **Use Attachments** dentro do `ChatInputArea`, acima do UserInput
- AttachmentList deve ter scroll horizontal se houver mais de 3 arquivos
- Nomes longos devem ser truncados com "..." mantendo a extensão visível
- Limite de `maxFiles` é indicado com toast ao tentar adicionar mais
- Arquivos são enviados juntos com a mensagem, nunca antes

---

## Anti-patterns

- **PROIBIDO**: exibir mais de 5 AttachmentItems sem scroll
- **PROIBIDO**: RemoveButton inacessível durante upload
- **PROIBIDO**: enviar a mensagem enquanto há arquivos com estado `uploading`
- **PROIBIDO**: truncar a extensão do arquivo no FileName

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
