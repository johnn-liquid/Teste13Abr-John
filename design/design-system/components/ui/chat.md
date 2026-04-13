# Componente — Chat

## Nome canônico
`Chat`

## Descrição
Container principal da interface conversacional. Envolve toda a troca de mensagens entre usuário e agente. É o organismo central do produto — todos os outros componentes de interação vivem dentro ou ao redor dele.

---

## Anatomia

```
Chat
├── ChatHeader              ← barra superior com nome do agente + ações
│   ├── AgentIcon           ← identidade visual do agente ativo
│   ├── AgentName           ← label-1 (Geist Medium 14px)
│   └── ChatActions         ← ações contextuais (nova conversa, configurar)
├── ChatScrollArea          ← área rolável com histórico de mensagens
│   └── MessageList         ← lista de componentes Text (usuário e agente)
│       ├── Text (agente)
│       ├── Text (usuário)
│       └── Artifact (quando a IA gera um output)
├── SuggestionsBar          ← opcional — Suggestions logo acima do input
└── ChatInputArea           ← área fixa na base
    ├── UserInput           ← campo de digitação principal
    └── Attachments         ← opcional — lista de arquivos anexados
```

---

## Variantes

| Variante | Descrição |
|---|---|
| `default` | Largura máxima de conteúdo, sem sidebar |
| `with-sidebar` | Chat com painel lateral de contexto (Knowledge Base, Files) |
| `fullscreen` | Chat ocupa toda a viewport, sem navbar |
| `embedded` | Chat em painel dentro de outra página |

---

## Estados obrigatórios

| Estado | Comportamento visual |
|---|---|
| `empty` | ChatScrollArea vazio com mensagem de boas-vindas + Suggestions iniciais |
| `loading` | Indicador de digitação do agente (três pontos animados, cor `--muted-foreground`) |
| `streaming` | Mensagem do agente aparecendo gradualmente (token a token) |
| `error` | Banner inline no topo da ChatScrollArea com descrição do erro + ação de retry |
| `populated` | Estado normal com histórico de mensagens |

---

## Props e defaults

| Prop | Tipo | Default | Descrição |
|---|---|---|---|
| `variant` | `default \| with-sidebar \| fullscreen \| embedded` | `default` | Layout do container |
| `agentName` | `string` | obrigatório | Nome do agente exibido no header |
| `agentIcon` | `AgentIcon` | obrigatório | Componente de ícone do agente |
| `showSuggestions` | `boolean` | `true` | Exibir SuggestionsBar acima do input |
| `maxWidth` | `string` | `768px` | Largura máxima da área de conteúdo |

---

## Mapeamento de tokens

| Parte | Token |
|---|---|
| Background do container | `--background` |
| Background do ChatHeader | `--sidebar` |
| Borda do ChatHeader (bottom) | `--border` |
| Background do ChatInputArea | `--background` |
| Borda do ChatInputArea (top) | `--border` |
| Nome do agente | `--foreground` + `label-1` |
| Indicador de digitação | `--muted-foreground` |

---

## Regras de uso

- **Use Chat** quando toda a interface gira em torno de uma conversa com um agente
- Chat é o único componente que pode conter Artifacts diretamente na MessageList
- O ChatHeader sempre exibe AgentIcon + AgentName — nunca remova esses elementos
- ChatScrollArea deve ter scroll independente — nunca scroll da página inteira para o chat
- Novo conteúdo (mensagens, artefatos) sempre aparece na base da MessageList

---

## Anti-patterns

- **PROIBIDO**: usar Chat como wrapper genérico para conteúdo não-conversacional
- **PROIBIDO**: remover o ChatHeader — identificação do agente é obrigatória
- **PROIBIDO**: colocar dois componentes Chat na mesma tela
- **PROIBIDO**: scrollar a página inteira ao invés do ChatScrollArea
- **PROIBIDO**: exibir Artifacts fora do contexto de uma mensagem do agente

## shadcn/ui

Componente customizado — não existe nativamente no shadcn/ui. Implementação própria baseada em shadcn/ui primitives.

---

## Exemplo de composição

```
// Tela de chat padrão com agente
<Chat variant="default" agentName="Assistente" agentIcon={<AgentIcon id="default" />}>
  <ChatScrollArea>
    <Text role="agent" content="Como posso ajudar?" timestamp="10:30" />
    <Text role="user" content="Gere um relatório de vendas" timestamp="10:31" />
    <Text role="agent" content="Gerando o relatório..." timestamp="10:31" />
    <ArtifactDataTable data={salesData} />
  </ChatScrollArea>
  <SuggestionsBar suggestions={["Exportar PDF", "Filtrar por data"]} />
  <ChatInputArea>
    <UserInput placeholder="Mensagem..." />
  </ChatInputArea>
</Chat>
```
