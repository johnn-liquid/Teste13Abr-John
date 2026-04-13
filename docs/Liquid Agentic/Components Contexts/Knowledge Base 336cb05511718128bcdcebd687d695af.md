# Knowledge Base

Atualizado em: 2 de abril de 2026
Group: Sistema
Status: Draft

## O que é

Repositório onde o cliente armazena arquivos, dados e documentos que alimentam o contexto global das conversas. É a memória persistente do cliente dentro do produto — o que define a camada de fidelização.

## Por que existe

O ponto de inflexão da fidelização acontece quando o cliente traz seus dados para dentro da plataforma. A Knowledge Base é esse repositório — ela transforma o produto de uma ferramenta genérica em um sistema personalizado para aquela carteira, empresa ou mercado.

## Como existe

**Estrutura:**

- Projeto único na raiz — sem segmentação por workspace no momento inicial
- Arquivos: documentos, contratos, laudos, tabelas
- Dados: planilhas estruturadas, exports de sistemas
- Integrações: Google Drive, Notion, CRM como fontes alternativas (futuro)

**Comportamento:**

- Contexto da KB é referenciado **explicitamente** pelo agente — não automaticamente, para evitar consumo desnecessário de tokens
- Usuário pode indicar quais documentos estão "ativos" para a sessão atual
- Agente cita a fonte quando usa um documento da KB (ver Knowledge Base Indicator)
- Busca semântica dentro dos documentos indexados

**Painel de gerenciamento (Navbar → Knowledge Base):**

- Lista de arquivos e documentos indexados
- Upload em massa
- Status de indexação por arquivo
- Tags e organização manual

## Onde é usado

- Chat: contexto disponível para as Skills quando referenciado
- Navbar: painel de gerenciamento (Files + Docs)

## Quem usa

- Usuário: sobe e organiza documentos
- Skills: consultam conteúdo indexado para análises
- Knowledge Base Indicator: exibe as fontes utilizadas