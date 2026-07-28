# Pacote Cowork — Construir o Pipe Financeiro do CX no Pipefy (ponta a ponta)

> **Para o Claude Cowork** (ambiente do usuário, com o navegador e o **Pipefy logado**). Objetivo: **construir do zero**, no Pipefy, o pipe **"CX — Casos com Decisão Financeira"**, seguindo o blueprint `docs/processos/pipefy-cx-financeiro-blueprint.md` do repositório `rrfernandes01-ops/fluxosneoassist` (branch `main`). Deve ficar **fácil para os analistas operarem**, **forte em evidências**, com o **gate de validação do Head** e **relatórios de prestação de contas**.

## 0. Pré-requisitos

- Pipefy logado no navegador, com permissão para criar pipe, campos, automações e dashboards.
- Ler o blueprint completo antes de começar: `docs/processos/pipefy-cx-financeiro-blueprint.md`.
- **Não** colocar tokens/credenciais em nenhum arquivo do repositório.

## 1. Criar o pipe e as fases

Criar o pipe **"CX — Casos com Decisão Financeira"** com as fases (seção 2 do blueprint), nesta ordem:
1. Entrada / Triagem → 2. Investigação / Evidências → 3. Consulta às áreas → 4. Parecer do CX → 5. **Decisão do Head** → 6. Execução → 7. Concluído → (+) **Reprovado / Arquivado**.

## 2. Montar o start form (formulário inicial)

Criar os campos da **seção 3** do blueprint, respeitando os **obrigatórios**. Tipos sugeridos:
- Protocolo NeoAssist (texto curto, obrigatório), Data de abertura (data), Canal (select), Solicitante tipo (select) + nome (texto) + contato (texto).
- Razão social (texto), CNPJ/CPF (texto), Comprador (texto), Pedido (texto), **Valor do pedido (número/currency)**, NF (texto), Boleto (texto + data de vencimento).
- Causa raiz categoria (select), Descrição (texto longo), **Tipo de decisão solicitada (select** com as opções do blueprint, incluindo "Condição fora da Tabela Prazo x Valor"**)**, Sugestão de ação (texto longo), Urgência (select), Evidências (attachment, múltiplos).
- **Campos de cálculo do impacto financeiro (seção 5 do blueprint — obrigatório e em destaque)**: `Valor base (R$)` (currency), `Valor concedido (R$)` (currency), `Dias adicionais` (número), `Taxa financeira mensal (%)` (número), `Custo do prazo (R$)` (fórmula: Valor base × Taxa/100 × Dias adicionais/30), **`Impacto financeiro total (R$)`** (fórmula/currency = Valor concedido + Custo do prazo — **obrigatório**, é o campo somado nos relatórios), `Valor de referência tabela/venda (R$)` (currency, só referência), `Memória de cálculo` (texto longo, obrigatório). Se o Pipefy permitir **campo de fórmula**, calcular `Custo do prazo` e `Impacto financeiro total` automaticamente; senão, o analista preenche seguindo a tabela da seção 5.1.

## 3. Campos por fase

- **Fase 2 (Evidências)**: checklist de evidências por causa raiz (seção 4) + campo "Evidências completas?" (Sim/Não). Usar campos condicionais por categoria de causa.
- **Fase 4 (Parecer do CX)**: Recomendação do CX (texto longo), Impacto financeiro confirmado (currency).
- **Fase 5 (Decisão do Head)**: **Decisão** (select: Aprovado / Aprovado com ajuste / Reprovado, obrigatório), Condição aprovada (texto), **Justificativa** (texto longo, obrigatório), Responsável (Head) + data. (Opcional) campo de alçada por valor.
- **Fase 6 (Execução)**: Ação executada (select), Data da execução, Comprovante (attachment).
- **Fase 7 (Concluído)**: Resultado (texto), Retorno ao cliente feito? (Sim/Não).

## 3.1 Cálculo do impacto financeiro (obrigatório, seção 5 do blueprint)

Deixar **evidente no formulário** (agrupar os campos sob o título "Impacto financeiro") a lógica por tipo de decisão:
- **Valor concedido (A)**: frete isentado, valor estornado, CMV de bonificação/devolução + reversa, valor do crédito, desconto.
- **Custo do prazo (B)** (prorrogação/negociação de prazo): `Valor base × Taxa mensal × (Dias adicionais ÷ 30)`.
- **Impacto financeiro total = A + B** (campo somado nos relatórios).
Valorar giveaways de produto a **custo (CMV)** e saídas de caixa pelo **valor de face**; a **taxa financeira mensal** é o parâmetro `[TAXA_FINANCEIRA_MENSAL]` a confirmar com o Head/Financeiro. Exigir sempre a **Memória de cálculo** preenchida.

## 4. Regras de avanço (fáceis para o analista)

- Fase 2 → 3/4 só com "Evidências completas? = Sim".
- Fase 4 → 5 sempre (parecer é recomendação).
- Fase 5: "Reprovado" → move para **Reprovado/Arquivado**; "Aprovado/ajuste" → **Execução**.
- Tornar obrigatórios os campos-chave de cada fase para não avançar incompleto.

## 5. Etiquetas, responsáveis e automações (seção 6 do blueprint)

- **Etiquetas**: tipo de decisão, causa raiz, urgência.
- **Responsável automático** por fase (analista CX 1–4; Head na 5; Financeiro/Logística na 6).
- **Automações**: alerta de SLA (prazo por fase da seção 2); notificar o Head ao entrar na fase 5; notificar o solicitante ao concluir/reprovar; mover card ao concluir checklist.

## 6. Dashboards / relatórios de prestação de contas (seção 7 do blueprint)

Montar um **dashboard executivo mensal** com:
- Valor total concedido no mês por tipo de ação; nº de casos por status; ticket médio.
- Causa raiz predominante (%); top clientes por valor; taxa aprovação × reprovação; tempo médio por fase.
- Configurar **export** (PDF/Sheets) do dashboard e uma **tabela (database view)** filtrável por período.
- Deixar pronto o **modelo de sumário de 1 página** (seção 7) para o Head apresentar a Financeiro/Diretoria/Sócios.

## 7. Ligação com a NeoAssist (mapeamento)

- Levantar o **pipe_id** e os **field_id** do start form e reportar (para o Claude Code registrar em `docs/integracoes/pipefy-cx.md`).
- Definir o mecanismo de criação do card a partir do **Workflow da NeoAssist** (`openWorkflow`): integração nativa NeoAssist↔Pipefy, webhook ou middleware (confirmar com o usuário). Enquanto o Workflow não estiver habilitado, o CX cria o card manualmente a partir do protocolo.

## 8. Testar ponta a ponta

Criar **2–3 casos fictícios** cobrindo causas diferentes (logística, financeiro/prorrogação, condição fora da tabela) e levar cada um até Concluído/Reprovado, passando pelo **gate do Head**. Conferir que os relatórios somam corretamente os valores.

## 9. Reportar ao usuário

- Link do pipe criado; pipe_id e field_ids; estrutura de fases/campos.
- Print/descrição dos dashboards e do modelo de sumário.
- Mecanismo de ligação com a NeoAssist e pendências (ex.: habilitar Workflow).
- Sugestões de ajuste que surgirem ao construir.
