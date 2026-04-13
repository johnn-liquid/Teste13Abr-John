# Artifact — Input Request

Atualizado em: 7 de abril de 2026
Group: Artefatos
Status: Draft

## O que é

Formulário inline gerado pelo agente para coletar dados estruturados necessários à execução de uma skill. É o único artefato que bloqueia o fluxo — o agente aguarda a resposta do usuário antes de prosseguir.

## Por que existe

Análises de crédito, elegibilidade MCMV e scoring exigem dados precisos que o agente não pode inferir: CPF, renda, valor do imóvel. Coletar esses dados via texto livre introduz erro de parsing e ambiguidade. O Input Request formaliza a coleta com tipos, máscaras e validação — garantindo que a skill receba dados estruturados e corretos.

## Como existe

### Tipos

| Tipo | Quando usar |
| --- | --- |
| `single-field` | O agente precisa de um único dado para continuar. Ex: apenas o CPF |
| `multi-field` | Múltiplos dados necessários de uma vez. Ex: CPF + renda + valor do imóvel |
| `confirmation` | O agente inferiu dados com base no contexto e pede confirmação antes de executar a skill |

### Campos suportados

`text`, `number`, `currency`, `date`, `date_range`, `cpf`, `cnpj`, `select`, `multi_select`, `file_upload`, `toggle`

CPF e CNPJ são tipos nativos com máscara brasileira e validação de dígito verificador. São os gatilhos mais frequentes para conectores de crédito — devem ser solicitados isolados, em `single-field`, nunca enterrados em formulários longos.

### Estados

- `idle` — aguardando preenchimento pelo usuário
- `submitted` — preenchido e enviado, campos travados e imutáveis no histórico da conversa
- `error` — validação falhou, campo com mensagem de erro inline

### Regra de quando usar

Use Input Request quando **a skill não pode executar sem dados que o usuário ainda não forneceu**.

**Não use** quando o dado já está no contexto da sessão ou no perfil do usuário — o agente deve inferir e seguir sem interromper o fluxo.

**Não use** para confirmar algo que o agente pode inferir com alta confiança — inferir e agir é preferível a perguntar.

**Não use** `multi-field` com mais de 5 campos. Formulários longos aumentam abandono. Se necessário, quebre em múltiplos `single-field` sequenciais.

CPF e CNPJ devem ser solicitados sozinhos — nunca como campo secundário de um formulário maior.

Se o agente tem informação útil para mostrar enquanto espera a resposta, combine um Card ou texto antes do Input Request — nunca substitua conteúdo por formulário.

## Onde é usado

Inline no chat, como parte do fluxo da conversa. Nunca no painel split como artefato isolado — o contexto da solicitação deve estar visível acima do formulário.

## Quem usa

Gerado pelo Orchestrator (quando identifica que a intent exige dados não disponíveis na sessão) e pelos Skill Agents (quando a execução da skill depende de input direto do usuário). Consumido pelo usuário final para fornecer dados que desbloqueiam a análise.