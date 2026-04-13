# Knowledge Base Indicator

Atualizado em: 2 de abril de 2026
Group: Chat/Interação
Status: Draft

## O que é

Indicador visual no chat que mostra quais documentos ou fontes da Knowledge Base estão sendo referenciados pelo agente na resposta atual. É uma âncora de transparência — o usuário sabe de onde veio a informação.

## Por que existe

O produto opera em mercado regulado. O usuário e a Liquid precisam saber que uma análise foi baseada em fontes confiáveis e auditáveis.

## Como existe

**Formatos:**

- `inline citation` — referência embutida no texto do agente
- `source panel` — lista expandível de fontes usadas na resposta
- `active context badge` — indicador persistente de quais documentos estão ativos na sessão

**Comportamento:**

- Aparece automaticamente quando o agente usou contexto da Knowledge Base
- Clicável — abre o documento referenciado ou destaca o trecho relevante
- Contexto deve ser referenciado explicitamente para evitar consumo desnecessário de tokens (regra arquitetural)
- Se nenhum documento foi usado, o indicador não aparece

## Onde é usado

Dentro das respostas do agente no Chat. No painel lateral da Knowledge Base quando ativa.

## Quem usa

Exibido para o usuário. Gerado automaticamente pelo sistema durante a execução das Skills.