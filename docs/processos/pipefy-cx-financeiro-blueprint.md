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
- **Campos de cálculo do impacto financeiro** (obrigatórios conforme o tipo — ver **seção 5. Cálculo do impacto financeiro**): `Valor base (R$)`, `Valor concedido (R$)` (a valor de venda), `Dias adicionais` e `Taxa financeira mensal (%)` (para prazo; padrão 2% a.m.), `Custo do prazo (R$)` e **`Impacto financeiro total (R$)`** (**obrigatório** — base dos relatórios), `Memória de cálculo` (texto).
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

## 5. Cálculo do impacto financeiro (DESTAQUE — obrigatório em todo card)

O **impacto financeiro** é o número que sustenta os relatórios à Diretoria/Sócios, então **cada card precisa calculá-lo de forma padronizada e com memória de cálculo**. A lógica **muda conforme o tipo de decisão**. Há dois componentes:

- **(A) Valor concedido** — o dinheiro que a empresa deixa de receber ou desembolsa (frete, estorno, devolução, bonificação, crédito, desconto), sempre a **valor de venda** (a empresa não usa CMV neste processo).
- **(B) Custo do prazo** — quando a decisão é dar mais prazo (prorrogação/negociação de prazo), não há perda do principal, mas há **custo financeiro do capital parado**: `Valor base × Taxa financeira mensal × (Dias adicionais ÷ 30)`.

**Impacto financeiro total (R$) = (A) + (B)**, conforme o tipo. É esse campo que alimenta os relatórios (seção 7).

### 5.1 Fórmula por tipo de decisão

| Tipo de decisão | Base de cálculo | Impacto financeiro (R$) |
|-----------------|-----------------|--------------------------|
| **Isenção de frete** | Valor do frete que a empresa absorve | = valor do frete isentado |
| **Estorno** (total/parcial) | Valor devolvido ao cliente em dinheiro | = valor estornado |
| **Devolução total** | Mercadoria que retorna | = **valor de venda** dos itens devolvidos + frete da logística reversa − valor recuperável (revenda) |
| **Devolução parcial** | Itens devolvidos | = **valor de venda** dos itens devolvidos (proporcional) + reversa |
| **Cancelamento** | Pedido não faturado | Se **já faturado** → tratar como estorno/devolução. Se **não faturado** → impacto de caixa = 0; registrar **receita não realizada** (referência) e custos incorridos (ex.: frete já pago) |
| **Bonificação** | Produtos dados sem cobrança | = **valor de venda** dos itens bonificados |
| **Crédito (próxima compra)** | Crédito concedido | = valor do crédito (provisão até ser usado) |
| **Prorrogação de boleto** | Prazo adicional | = Valor base × Taxa financeira mensal × (Dias adicionais ÷ 30) — componente (B) |
| **Negociação** | Desconto + prazo | = desconto concedido (A) **+** custo do prazo (B), quando houver ambos |
| **Condição fora da Tabela** | Depende do que foi concedido | Aplicar a fórmula do efeito concedido (desconto, prazo, isenção…) |

### 5.2 Convenção de valoração (definida pelo Head/Financeiro)

- **Produtos dados/devolvidos** (bonificação, devolução) e **saídas de caixa** (estorno, frete, crédito): tudo valorado a **valor de venda** — a empresa **não usa CMV** neste processo (decisão registrada).
- **Taxa financeira mensal** (para o custo do prazo): **2% ao mês** como padrão conservador de mercado (custo de capital de giro). É um **parâmetro configurável** no pipe — o Head e o Financeiro ajustam ao longo do projeto sem mudar a metodologia. Manter como constante/campo `Taxa financeira mensal (%)` para facilitar a troca.

> Convenções já decididas: valor de venda para tudo; taxa **2% a.m.** ajustável. Se o Financeiro definir outra taxa, basta alterar o parâmetro no pipe — os cálculos e relatórios seguem iguais.

### 5.3 Campos no card (para o cálculo ser automático e auditável)

- `Valor base (R$)` — valor do pedido/itens/frete conforme o tipo.
- `Valor concedido (R$)` — componente (A).
- `Dias adicionais` e `Taxa financeira mensal (%)` — para o componente (B); a taxa vem do parâmetro padrão **2% a.m.** (ajustável).
- `Custo do prazo (R$)` — calculado por fórmula (Pipefy: campo de fórmula ou preenchido pelo analista com a conta).
- **`Impacto financeiro total (R$)`** — (A)+(B); **é o campo somado nos relatórios**.
- `Memória de cálculo` (texto) — o analista descreve como chegou ao número (evidência/auditoria).

### 5.4 Exemplos rápidos

- **Isenção de frete** de R$ 80 → impacto = **R$ 80**.
- **Bonificação** de 12 unidades a R$ 30/un (valor de venda) → impacto = **R$ 360**.
- **Prorrogação** de R$ 5.000 por +30 dias, taxa 2%/mês → impacto = 5.000 × 0,02 × (30/30) = **R$ 100**.
- **Devolução parcial**: 5 itens a R$ 40/un (venda) + reversa R$ 30 → impacto = 200 + 30 = **R$ 230**.

## 6. Gate de decisão do Head (fase 5)

Campos preenchidos **pelo Head**:
- **Decisão** (**obrigatório**): Aprovado · Aprovado com ajuste · Reprovado
- **Condição aprovada**: novo prazo / valor de crédito / % de devolução / valor de isenção etc.
- **Justificativa da decisão** (**obrigatório**)
- **Responsável pela decisão** (Head) + data (automático)
- (Opcional) **Alçada**: definir um valor-limite abaixo do qual o Coordenador de CX decide e acima do qual exige o Head — se você quiser delegar casos menores. Padrão: **todos passam pelo Head**.

Só após "Aprovado/Aprovado com ajuste" o card vai para **Execução**. "Reprovado" → fase Reprovado com justificativa e retorno ao solicitante.

## 7. Rótulos, automações e responsáveis (facilitar a operação)

- **Etiquetas (labels)**: por tipo de decisão, por causa raiz, por urgência (alta/média/baixa).
- **Responsável automático** por fase (analista CX na 1–4; Head na 5; Financeiro/Logística na 6).
- **Automações**: mover card ao concluir checklist; **alertas de SLA** (prazo estourando); notificar o Head quando um card entra na fase 5; notificar o solicitante ao concluir/reprovar.
- **Campos condicionais**: mostrar campos de logística só quando a causa é logística, etc.

## 8. Relatórios de prestação de contas (conciso e assertivo)

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

## 9. Ligação com a NeoAssist e o A11

- O A11 abre protocolo → Workflow (`openWorkflow`) → **cria o card** no pipe com o start form preenchido (mapa de campos no pacote Cowork Pipefy).
- Decisão e execução ficam no Pipefy; o **resultado** pode voltar ao protocolo NeoAssist (atualização) para fechar o ciclo com o cliente.
- Enquanto o Workflow não estiver habilitado, o card é criado manualmente pelo CX a partir do protocolo (contingência).

## 10. Checklist de construção (resumo)
1. Criar o pipe com as **7 fases** (+ Reprovado).
2. Montar o **start form** (seção 3) e os **campos por fase** (2, 4, gate 6).
3. Configurar **checklist de evidências** (seção 4) e o campo "Evidências completas?".
4. Configurar os **campos e a fórmula de impacto financeiro** (seção 5) — campo `Impacto financeiro total (R$)` e `Memória de cálculo`.
5. Configurar o **gate do Head** (seção 6).
6. Criar **etiquetas, responsáveis, automações e alertas de SLA** (seção 7).
7. Montar os **dashboards/relatórios** (seção 8), somando o `Impacto financeiro total`.
8. Testar com 2–3 casos fictícios ponta a ponta, conferindo os cálculos.
