# Componente — User Input

## Nome canônico
`UserInput`

## Descrição
Campo de entrada principal do usuário no chat. Suporta texto multiline, anexos e ações de envio. É o ponto de interação mais frequente da interface.

---

## Anatomia

```
UserInput
├── TextArea              ← área de digitação (auto-resize)
├── AttachButton          ← botão de anexar arquivo (opcional)
├── VoiceButton           ← botão de input por voz (opcional)
└── SendButton            ← botão de enviar (ativo quando há conteúdo)
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `default` | Textarea com send button |
| `with-attachments` | Inclui AttachButton à esquerda |
| `with-voice` | Inclui VoiceButton |
| `full` | Todos os controles (attach + voice + send) |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `empty` | Textarea com placeholder, SendButton desabilitado (opacity 40%) |
| `typing` | Textarea com texto, SendButton habilitado (`--primary`) |
| `focused` | Border `--ring`, shadow `--shadow-sm` |
| `disabled` | Toda a área com opacity 40%, interações bloqueadas |
| `loading` | SendButton substituído por spinner, textarea readonly |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `placeholder` | `string` | `"Mensagem..."` | Texto de placeholder |
| `variant` | `default \| with-attachments \| with-voice \| full` | `default` | Controles visíveis |
| `maxRows` | `number` | `6` | Máximo de linhas antes de scroll interno |
| `disabled` | `boolean` | `false` | Bloqueia toda a interação |
| `onSend` | `function` | obrigatório | Callback de envio |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background do input | `--input` |
| Borda default | `--border` |
| Borda em focus | `--ring` |
| Texto digitado | `--foreground` + `body-lg` |
| Placeholder | `--muted-foreground` + `body-lg` |
| Radius | `--radius-lg` |
| Padding | `--space-3` (vertical) + `--space-4` (horizontal) |
| SendButton ativo | `--primary` |
| SendButton inativo | `--muted` |
| Ícones de ação | `--muted-foreground` (default) → `--foreground` (hover) |

---

## Regras de uso

- **Use UserInput** exclusivamente dentro de `ChatInputArea` do componente `Chat`
- O SendButton deve estar desabilitado quando a textarea estiver vazia
- O textarea cresce verticalmente conforme o usuário digita, até `maxRows`
- Após `maxRows`, o conteúdo interno rola sem expandir o componente
- Enter sem Shift envia a mensagem. Shift+Enter cria nova linha
- Nunca remova o SendButton — é o indicador visual de "pode enviar"

---

## Anti-patterns

- **PROIBIDO**: usar UserInput fora do contexto de chat (para formulários, use input padrão)
- **PROIBIDO**: SendButton ativo com textarea vazia
- **PROIBIDO**: textarea de altura fixa (deve auto-redimensionar)
- **PROIBIDO**: placeholder muito longo — máximo 30 caracteres
- **PROIBIDO**: remover o focus ring durante digitação

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
