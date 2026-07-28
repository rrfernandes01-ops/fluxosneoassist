# Blueprint — Pipe Financeiro do CX no Pipefy (Casos com decisão financeira)

> Desenho completo do pipe **"CX — Casos com Decisão Financeira"** no Pipefy, para o processo `reclamacao-decisao-financeira.md`. Objetivo: **evidências fortes**, **etapas claras e fáceis para os analistas**, **pontos de validação do Head (você)** e um **gerador de relatórios conciso e assertivo** para prestação de contas ao Financeiro, à Diretoria e aos sócios.
>
> Integração: `[[INT_PIPEFY_CX]]` (I-21). Alimentado pelo Workflow da NeoAssist (`openWorkflow`) e pelo agente A11. Regra de governança: **decisão final = Head + CX** (nenhuma área isolada e nenhuma IA decide).

## 1. Princípios de desenho

- **Fácil de operar**: campos obrigatórios só onde necessário, checklists por causa raiz, rótulos e SLA por fase, responsável automático.
- **Forte em evidência**: cada caso só avança com as evidências mínimas da sua causa (seção 4).
- **Rastreável**: cada movimentação registra quem, quando e por quê; a decisão do Head fica gravada.
- **Pronto para prestar contas**: campos padronizados (valor, tipo de ação, causa) que alimentam relatórios automáticos (seção 7).

## 2. Fases do pipe

| # | Fase | Dono | O que acontece | SLA sugerido |
|---|------|------|----------------|--------------|
| 1 | **Entrada / Triagem** | CX (analista) | Caso chega (Workflow NeoAssist ou manual). Analista confere dados mínimos e classifica. | 4h úteis |
| 2 | **Investigação / Evidências** | CX (analista) | Coleta e anexa evidências; puxa dados de sistema (ERP/rastreio/histórico); preenche a causa raiz. | 1 dia útil |
| 3 | **Consulta às áreas** | CX + áreas | Aciona logística / comercial / financeiro-faturamento / jurídico conforme o caso (checklist). | 1–2 dias úteis |
| 4 | **Parecer do CX** | CX (analista/coord.) | Consolida causa raiz, impacto financeiro e **recomendação** (não é decisão). | 4h úteis |
| 5 | **Decisão do Head** | **Head (você)** | **Gate de validação**: aprova / aprova com ajuste / reprova; define a condição. | 1 dia útil |
| 6 | **Execução** | Financeiro / Logística | Executa a ação aprovada (prorrogação, isenção, estorno, crédito…). | 1–2 dias úteis |
| 7 | **Concluído** | CX | Registra o resultado e encerra; retorno ao solicitante/cliente. | — |
| — | **Reprovado / Arquivado** | CX | Caso negado ou sem ação, com justificativa. | — |

## 3. Formulário inicial (start form) — campos

Agrupados; marcar (**obrigatório**) os essenciais. Muitos vêm do A11/Workflow.

**Identificação do caso**
- Protocolo NeoAssist (**obrigatório**)
- Data de abertura (**obrigatório**) · Canal de origem (WhatsApp/e-mail/telefone) (**obrigatório**)
- Solicitante: tipo (cliente B2C/B2B, representante, supervisor, gerente) + nome + contato (**obrigatório**)

**Cliente e pedido**
- Razão social / nome (**obrigatório**) · CNPJ (ou CPF B2C) (**obrigatório**)
- Comprador / responsável
- Pedido relacionado (nº) (**obrigatório**) · Valor do pedido (**obrigatório**)
- Nota fiscal (nº) · Boleto/título (nº, vencimento, valor)

**Causa e pedido de decisão**
- Causa raiz — categoria (**obrigatório**): Logística/Entrega · Comercial/Relacionamento · Financeiro · Trade Marketing · Outro
- Descrição da causa raiz (**obrigatório**)
- Tipo de decisão solicitada (**obrigatório**): Prorrogação de boleto · Isenção de frete · Cancelamento · Estorno · Devolução total · Devolução parcial · Bonificação · Crédito (próxima compra) · Negociação · **Condição fora da Tabela Prazo x Valor**
- **Valor financeiro envolvido / impacto estimado (R$)** (**obrigatório** — base dos relatórios)
- Sugestão de ação (do solicitante/CX) — *registrada, não aprovada*
- Urgência / prazo

**Evidências** (anexos — seção 4)

## 4. Checklist de evidências por causa raiz (eficiência)

O card só avança da fase 2 com as evidências mínimas da sua causa:

- **Logística/Entrega**: código de rastreio + print do status; protocolo da transportadora; comprovante de frete pago (se "pagou frete rápido e não recebeu"); data prometida × data real.
- **Comercial/Relacionamento**: extrato de sellout/estoque do cliente; histórico de compras/relacionamento; evidência da exceção (e-mail/acordo).
- **Financeiro**: boleto/título (nº, vencimento, valor); posição de outros títulos em aberto.
- **Trade Marketing**: registro da ação e do atraso; card de Trade relacionado.
- **Sempre**: print/registro da conversa/protocolo NeoAssist.

> Campo "Evidências completas?" (Sim/Não) como condição para avançar — evita caso mal instruído chegar ao Head.

## 5. Gate de decisão do Head (fase 5)

Campos preenchidos **pelo Head**:
- **Decisão** (**obrigatório**): Aprovado · Aprovado com ajuste · Reprovado
- **Condição aprovada**: novo prazo / valor de crédito / % de devolução / valor de isenção etc.
- **Justificativa da decisão** (**obrigatório**)
- **Responsável pela decisão** (Head) + data (automático)
- (Opcional) **Alçada**: definir um valor-limite abaixo do qual o Coordenador de CX decide e acima do qual exige o Head — se você quiser delegar casos menores. Padrão: **todos passam pelo Head**.

Só após "Aprovado/Aprovado com ajuste" o card vai para **Execução**. "Reprovado" → fase Reprovado com justificativa e retorno ao solicitante.

## 6. Rótulos, automações e responsáveis (facilitar a operação)

- **Etiquetas (labels)**: por tipo de decisão, por causa raiz, por urgência (alta/média/baixa).
- **Responsável automático** por fase (analista CX na 1–4; Head na 5; Financeiro/Logística na 6).
- **Automações**: mover card ao concluir checklist; **alertas de SLA** (prazo estourando); notificar o Head quando um card entra na fase 5; notificar o solicitante ao concluir/reprovar.
- **Campos condicionais**: mostrar campos de logística só quando a causa é logística, etc.

## 7. Relatórios de prestação de contas (conciso e assertivo)

Configurar **dashboards/relatórios do Pipefy** (e/ou export para Sheets) com:

**Painel executivo (mensal)** — para Financeiro / Diretoria / Sócios:
- **Valor total concedido no mês** (soma do impacto financeiro dos casos aprovados), quebrado por **tipo de ação** (isenção, estorno, devolução, crédito, bonificação, prorrogação…).
- **Nº de casos** por status (aprovado / reprovado / em andamento) e **ticket médio**.
- **Causa raiz predominante** (%) — onde o dinheiro está indo e por quê.
- **Top clientes** por volume e por valor de casos.
- **Taxa de aprovação × reprovação**.
- **Tempo médio por fase (SLA)** e gargalos.
- **Ação corretiva** (texto): o que está sendo feito para reduzir a recorrência da causa dominante.

**Modelo de sumário (1 página) — "Prestação de contas CX — mês/ano"**:
> No mês, foram tratados **N casos** (R$ X de impacto financeiro total). Aprovados: **A** (R$ …); reprovados: **R**. Principal causa raiz: **[causa]** (Y%). Ações mais concedidas: **[tipo]** (R$ …). Tempo médio de resolução: **Z dias**. Destaques/《clientes recorrentes》: … Ações corretivas em curso: …

> Todos os valores saem dos campos padronizados do card (valor/tipo/causa/decisão), então o relatório é gerado automaticamente — sem retrabalho.

## 8. Ligação com a NeoAssist e o A11

- O A11 abre protocolo → Workflow (`openWorkflow`) → **cria o card** no pipe com o start form preenchido (mapa de campos no pacote Cowork Pipefy).
- Decisão e execução ficam no Pipefy; o **resultado** pode voltar ao protocolo NeoAssist (atualização) para fechar o ciclo com o cliente.
- Enquanto o Workflow não estiver habilitado, o card é criado manualmente pelo CX a partir do protocolo (contingência).

## 9. Checklist de construção (resumo)
1. Criar o pipe com as **7 fases** (+ Reprovado).
2. Montar o **start form** (seção 3) e os **campos por fase** (2, 4, 5).
3. Configurar **checklist de evidências** (seção 4) e o campo "Evidências completas?".
4. Configurar o **gate do Head** (seção 5).
5. Criar **etiquetas, responsáveis, automações e alertas de SLA** (seção 6).
6. Montar os **dashboards/relatórios** (seção 7).
7. Testar com 2–3 casos fictícios ponta a ponta.
