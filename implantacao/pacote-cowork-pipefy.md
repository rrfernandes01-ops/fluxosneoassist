# Pacote Cowork — Pipefy (Trade Marketing e CX Casos)

> **Para o Claude Cowork** (ambiente do usuário, com navegador e Pipefy acessíveis). Cobre as **duas** integrações Pipefy do projeto:
> - `[[INT_PIPEFY_TRADE]]` (I-17) — pipe de **Trade Marketing** (agente A7): criar card da ação de trade e ler a etapa.
> - `[[INT_PIPEFY_CX]]` (I-21) — pipe de **CX Casos** (agente A11 + processo `docs/processos/reclamacao-decisao-financeira.md`): abrir card do caso com decisão financeira (via Workflow) e ler as etapas.

## 1. Ler a documentação da API do Pipefy

- Abrir <https://developers.pipefy.com/> (API **GraphQL**). Capturar:
  - **Autenticação**: token pessoal/da organização (header `Authorization: Bearer`).
  - **Criar card** (`mutation createCard`): `pipe_id`, `fields_attributes` (field_id → value).
  - **Ler card/fase** (`query card`/`phase`): como obter a **fase atual** e os campos.
  - **Mover card** entre fases (se necessário para leitura de etapa).
  - Webhooks (opcional) para o NeoAssist saber quando a fase muda.
- **Token do Pipefy**: o usuário fornece; **não versionar** no repositório.

## 2. Levantar a estrutura dos pipes (com o usuário)

Para **cada** pipe (Trade e CX Casos):
- **pipe_id**.
- Lista de **fases** (etapas) na ordem do processo.
- **field_id** de cada campo do formulário inicial, para o mapeamento.

### 2.1 Pipe Trade Marketing — campos esperados (do A7)
Tipo de ação, objetivo/resultado; dossiê do cliente (CNPJ, nome, comprador, responsável); local (endereço, tipo, contato do responsável); se complexo comercial/evento (autorização, horários, carga/descarga, energia, espaço); data + alternativa; público; produtos/SKUs e quantidades; materiais e executor; contrapartida comercial; observações. (Fonte: `docs/agentes/A7-agente-representantes.md`, seção 5.4.)

### 2.2 Pipe CX Casos — campos esperados (do A11)
Cliente + CNPJ (ou CPF); pedido relacionado e valor; comprador/responsável; causa raiz; evidências (anexos); sugestão de ação; canal e data; decisão (a preencher pelo CX+Head). Etapas sugeridas: triagem → investigação por área (logística/comercial/financeiro/jurídico/board) → decisão CX+Head → execução → encerramento. (Fonte: `docs/processos/reclamacao-decisao-financeira.md`.)

## 3. Mapear (card do Pipefy ↔ dados do agente)

Preencher a tabela `field_id` do Pipefy → variável coletada pelo agente, para cada pipe.

## 4. Ligação com a NeoAssist

- **Trade (A7)**: o agente cria o card diretamente no pipe de Trade quando a solicitação está completa; depois lê a fase para informar o representante.
- **CX Casos (A11)**: o card é aberto **a partir do Workflow** da NeoAssist (`openWorkflow` → integração que cria o card no Pipefy). Confirmar o mecanismo (integração nativa NeoAssist↔Pipefy, webhook, ou middleware) com o usuário.

## 5. Entregável

**Atualizar o repositório** (sem token): criar `docs/integracoes/pipefy-trade.md` e `docs/integracoes/pipefy-cx.md` com pipe_id, fases, field_ids e o mapeamento; atualizar os contratos I-17 e I-21 em `docs/03-integracoes.md`. Abrir PR na branch `claude/fitoway-whatsapp-ai-x5uzrt`, **ou** reportar ao usuário para o Claude Code registrar.

## 6. Testar (após conectar)

- Trade: simular uma solicitação completa no A7 → card criado no pipe certo com todos os campos; consultar etapa → agente informa a fase.
- CX Casos: abrir um caso no A11 → protocolo + Workflow + card no Pipefy; conferir campos e etapa inicial.
- Timeout/silêncio zero no nó (doc 05, seção 2.2) e Regra Anti-Espera na contingência.

## 7. Reportar
pipe_ids, fases, mapeamento (link do PR ou lista), mecanismo de ligação NeoAssist↔Pipefy e resultado dos testes.
