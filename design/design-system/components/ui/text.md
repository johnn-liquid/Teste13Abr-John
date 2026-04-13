# Componente — Text

## Nome canônico
`Text`

## Descrição
Mensagem de texto exibida no chat — tanto do usuário quanto do agente. Inclui conteúdo, metadados (timestamp, status) e suporta markdown básico para mensagens do agente.

---

## Anatomia

```
Text
├── Avatar / AgentIcon     ← identificação visual do emissor
├── MessageBubble          ← container da mensagem
│   ├── Content            ← texto da mensagem (suporta markdown em role=agent)
│   ├── Timestamp          ← hora de envio
│   └── StatusIndicator    ← apenas role=user: enviando / enviado / erro
└── Actions (hover)        ← copiar, reagir, referenciar (apenas visível no hover)
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `role="user"` | Mensagem do usuário — alinhada à direita, `--muted` background |
| `role="agent"` | Mensagem do agente — alinhada à esquerda, sem background ou `--card` |
| `role="system"` | Mensagem de sistema — centralizada, label-3, `--muted-foreground` |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `default` | Mensagem completa exibida normalmente |
| `streaming` | Texto aparece progressivamente com cursor piscante no final |
| `error` | StatusIndicator vermelho (`--destructive`) com ação de reenvio |
| `sending` | StatusIndicator com spinner pequeno |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `role` | `user \| agent \| system` | obrigatório | Define alinhamento e estilo |
| `content` | `string` | obrigatório | Conteúdo da mensagem (suporta markdown) |
| `timestamp` | `string` | obrigatório | Hora no formato HH:MM |
| `status` | `sending \| sent \| error` | `sent` | Apenas para role=user |
| `streaming` | `boolean` | `false` | Ativa modo de stream progressivo |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background bubble (user) | `--muted` |
| Background bubble (agent) | transparente ou `--card` |
| Texto da mensagem | `--foreground` + `body-lg` |
| Timestamp | `--muted-foreground` + `body-sm` |
| Cursor de streaming | `--primary` |
| Erro (status) | `--destructive` |
| Radius da bubble | `--radius-lg` |
| Gap entre mensagens | `--space-3` |
| Padding da bubble | `--space-3` (vertical) + `--space-4` (horizontal) |

---

## Suporte a markdown (apenas role=agent)

| Elemento | Renderização |
|---|---|
| `**bold**` | `font-semibold` |
| `*italic*` | `font-italic` |
| `` `code` `` | `code-base`, background `--muted`, radius `--radius-sm` |
| `\`\`\`codeblock\`\`\`` | Bloco com header de linguagem, botão de copiar |
| `- lista` | Lista não ordenada com indent |
| `1. lista` | Lista ordenada |

---

## Regras de uso

- **Use Text** para toda mensagem de texto no chat
- Mensagens do agente com conteúdo estruturado (tabela, gráfico) devem usar Artifact, não Text
- Mensagens longas do agente (>500 caracteres) devem ter botão "Mostrar mais" após 300 caracteres
- O hover nas actions deve ser `--duration-fast` + `--ease-out`
- Sistema de mensagens deve preservar a ordem cronológica — novas mensagens na base

---

## Anti-patterns

- **PROIBIDO**: usar Text para exibir dados tabulares — use `Artifact — Datatable`
- **PROIBIDO**: usar Text para exibir código longo — use `Artifact — Document`
- **PROIBIDO**: centralizar mensagens de usuário ou agente (apenas `system` é centralizado)
- **PROIBIDO**: exibir markdown em mensagens de usuário — renderize como texto plano
- **PROIBIDO**: omitir timestamp

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
