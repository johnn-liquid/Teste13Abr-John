# Componente — Voice

## Nome canônico
`Voice`

## Descrição
Controle de input e output por voz no chat. Permite ao usuário falar ao invés de digitar, e ao agente responder em áudio. Pode ser ativado como substituto ou complemento ao UserInput.

---

## Anatomia

```
Voice
├── VoiceButton             ← botão que ativa/desativa a escuta
│   ├── MicIcon             ← ícone de microfone
│   └── WaveAnimation       ← animação de ondas durante gravação
├── TranscriptPreview       ← texto transcrito em tempo real (opcional)
└── VoiceOutputIndicator    ← indicador de que o agente está falando (opcional)
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `input-only` | Apenas captura de voz do usuário — transcreve para UserInput |
| `output-only` | Apenas reprodução de áudio do agente |
| `full-duplex` | Input e output de voz simultâneos |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `idle` | VoiceButton com MicIcon, cor `--muted-foreground` |
| `listening` | VoiceButton pulsando, WaveAnimation ativa, cor `--primary` |
| `processing` | VoiceButton com spinner, WaveAnimation parada |
| `speaking` | VoiceOutputIndicator ativo com animação de áudio |
| `error` | VoiceButton com ícone de erro, `--destructive`, toast com descrição |
| `disabled` | VoiceButton com opacity 40%, sem interação |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `input-only \| output-only \| full-duplex` | `input-only` | Modo de operação |
| `showTranscript` | `boolean` | `true` | Exibir TranscriptPreview durante escuta |
| `onTranscript` | `function(text: string)` | obrigatório para input | Callback com texto transcrito |
| `disabled` | `boolean` | `false` | Desabilita o controle |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Ícone idle | `--muted-foreground` |
| Ícone listening | `--primary` |
| Background do botão listening | `--primary` com opacity 10% |
| Borda pulsante | `--primary` |
| Transcrição de texto | `--muted-foreground` + `body-sm` |
| Erro | `--destructive` |
| Animação pulsante | `--duration-deliberate` + `--ease-in-out` |

---

## Regras de uso

- **Use Voice** como controle secundário, nunca substituindo UserInput por padrão
- O TranscriptPreview deve preencher o UserInput após o processamento
- Durante a escuta, UserInput deve ficar readonly mas visível
- A animação de ondas (WaveAnimation) é obrigatória no estado `listening` — comunica que o sistema está ativo

---

## Anti-patterns

- **PROIBIDO**: ativar Voice sem permissão explícita do usuário (prompt de permissão do browser)
- **PROIBIDO**: iniciar escuta automaticamente sem ação do usuário
- **PROIBIDO**: exibir VoiceButton sem indicação visual clara do estado atual
- **PROIBIDO**: remover UserInput quando Voice estiver ativo

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.
