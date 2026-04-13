# Structured Inputs

Atualizado em: 2 de abril de 2026
Group: Chat/Interação
Status: Draft

## O que é

Formulários contextuais gerados dinamicamente pelo sistema quando uma skill requer dados específicos e estruturados. Substituem temporariamente o User Input de texto livre.

## Por que existe

Algumas skills precisam de dados precisos para funcionar (ex: CPF, valor do imóvel, data de nascimento). O input estruturado garante a integridade dos dados e reduz alucinação do agente.

## Como existe

**Tipos de campo:**

- `text` — campo livre com validação de formato (CPF, CNPJ, CEP)
- `number` — valor numérico ou monetário
- `select` — opções predefinidas
- `date` — seletor de data
- `file` — upload de documento (integrado com Attachments)
- `cpf` / `cnpj` — campos especializados com máscara e validação

**Comportamento:**

- Gerado pelo Orchestrator quando a intenção detectada exige dados específicos
- Aparece no lugar do User Input ou como expansão do chat
- Validação em tempo real antes do envio
- Após envio, substituído por mensagem de confirmação no fluxo do chat

**Exemplo no MVP:** ao detectar intenção de aprovação → campos: CPF do comprador, tipo de renda, valor do imóvel

## Onde é usado

Na área do User Input, no Chat. Gerado dinamicamente conforme o contexto.

## Quem usa

Preenchido pelo usuário. Gerado pelo Orchestrator. Consumido pelas Skills.