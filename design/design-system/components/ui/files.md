# Componente — Files

## Nome canônico
`Files`

## Descrição
Gerenciamento de arquivos do usuário no contexto do produto. Painel que lista, organiza e permite operações em arquivos que o agente pode acessar ou que foram gerados pelo agente.

---

## Anatomia

```
Files
├── FilesHeader             ← título + botão de upload + busca
├── FileTree (opcional)     ← estrutura de pastas
└── FileList                ← lista de arquivos
    └── FileItem[]          ← item individual
        ├── FileIcon        ← ícone por tipo de arquivo
        ├── FileName        ← nome do arquivo
        ├── FileMeta        ← tamanho + data de modificação
        └── FileActions     ← download, renomear, excluir
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `panel` | Painel lateral de gestão de arquivos |
| `inline` | Lista compacta dentro de outra seção |
| `picker` | Seletor de arquivo para StructuredInputs ou Attachments |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `empty` | Ilustração + CTA "Fazer upload do primeiro arquivo" |
| `loading` | FileList com skeletons |
| `populated` | Lista de arquivos com ações |
| `uploading` | FileItem com ProgressBar e status "Enviando..." |
| `selected` | FileItem com background `--accent`, checkbox marcado |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `panel \| inline \| picker` | `panel` | Modo de uso |
| `files` | `File[]` | obrigatório | Lista de arquivos |
| `onUpload` | `function` | obrigatório | Inicia upload |
| `onDelete` | `function(fileId)` | obrigatório | Exclui arquivo |
| `selectable` | `boolean` | `false` | Habilita seleção múltipla |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background painel | `--sidebar` |
| FileItem hover | `--accent` background |
| FileItem selected | `--accent` background + `--primary` checkbox |
| FileName | `--foreground` + `body-md` |
| FileMeta | `--muted-foreground` + `body-sm` |
| Ícone de arquivo | `--muted-foreground` |
| ProgressBar | `--primary` |
| Borda separadora | `--border` |
| Padding FileItem | `--space-2` (vertical) + `--space-3` (horizontal) |

---

## Regras de uso

- **Use Files** para gerenciar o conjunto de arquivos acessível ao agente ou ao usuário
- FileActions aparecem apenas no hover para manter a lista limpa
- Exclusão de arquivo exige confirmação via modal destrutivo
- Arquivos gerados pelo agente são marcados com badge "Gerado por IA"

---

## Anti-patterns

- **PROIBIDO**: excluir arquivos sem confirmação destrutiva
- **PROIBIDO**: exibir FileActions permanentemente (apenas no hover)
- **PROIBIDO**: FileName truncado sem tooltip com nome completo

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
