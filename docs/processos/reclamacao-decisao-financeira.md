# Processo — Reclamação/Caso com decisão financeira (CX → Workflow → Pipefy)

> Processo transversal de tratamento de reclamações e solicitações que, além do relacionamento, exigem uma **decisão financeira** (prorrogação de boleto, isenção de frete, cancelamento, estorno, devolução total/parcial, bonificação, crédito, negociação).
>
> **Status de plataforma**: usa o **módulo Workflow (WF) da NeoAssist**, que **ainda não está habilitado** (previsão: próximas semanas). A documentação já deixa o processo pronto para configurar quando o WF for ativado. Integração com **Pipefy** para o painel de etapas do CX (placeholder `[[INT_PIPEFY_CX]]`, I-21).

## 1. Problema que este processo resolve

Hoje o processo está **sem dono**: quem acaba investigando é o **Financeiro**, que corre atrás de todo o histórico para entender se segue ou não com troca, isenção, cancelamento, estorno ou negociação. Isso gera lentidão, falta de padrão e decisão sem visão completa.

**Novo desenho**: o **CX passa a ser o dono** da investigação e da condução do caso; a **decisão final é do CX em conjunto com o Head (C-level)**. Todo caso fica evidenciado, rastreável e com etapas claras.

## 2. Entradas (de onde o caso chega)

| Origem | Como entra | Canal/Origin |
|--------|-----------|--------------|
| **Cliente B2C** | Pelos agentes A1 (site) quando o caso tem decisão financeira | WhatsApp 16 |
| **Cliente B2B** (SP ou Nacional) | Pelos agentes A5/A6 — o agente acolhe o cliente e colhe o máximo de informações | WhatsApp 16 |
| **Representante / Supervisor / Gerente** (interno) | Pelo fluxo do representante (A7) ou direto no agente A11 | WhatsApp 16 |
| **Cliente/colaborador por e-mail** | CX abre o protocolo a partir do e-mail | Email 1 |
| **Cliente/colaborador por telefone** | CX abre o protocolo a partir da ligação | Telefonia 7 |
| **Agente A11 (WhatsApp 2388-3360)** | Intake conversacional profundo (seção 4) | WhatsApp 16 |

O **Origin do protocolo** reflete o canal de entrada (WhatsApp 16 / Email 1 / Telefonia 7 / Registro Manual 8), conforme a API de criação (`integracoes/neoassist-protocolo-criacao.md`).

## 3. Fluxo ponta a ponta

```mermaid
flowchart TD
    IN[Entrada: B2C / B2B / representante-supervisor / e-mail / telefone] --> AGENTE{Chegou por IA no WhatsApp?}
    AGENTE -- Sim --> A11[Agente A11 / A5 / A6 / A7\ncoleta causa raiz + evidências + dados]
    AGENTE -- Não e-mail/telefone --> CXABRE[CX abre o protocolo manualmente]
    A11 --> PROTO[Protocolo criado\nOrigin conforme canal\n(INT_PROTOCOLO_CRIAR)]
    CXABRE --> PROTO
    PROTO --> WF[Abre Workflow na NeoAssist\nopenWorkflow + WFObservacao (resumo)\nDepartamentoID = CX Casos\n(INT_PROTOCOLO_ATUALIZAR)]
    WF --> PIPE[Abre card no Pipefy\npainel de etapas do CX\n(INT_PIPEFY_CX)]
    PIPE --> INVEST[CX investiga e evidencia\nconsulta: sistema, logística, comercial,\nfinanceiro/faturamento, jurídico, board C-level]
    INVEST --> DEC{Decisão CX + Head}
    DEC -- Aprova ação --> EXEC[Executa: prorrogação, isenção de frete,\ncancelamento, estorno, devolução,\nbonificação, crédito, negociação]
    DEC -- Nega/ajusta --> RETORNO[Retorno ao solicitante com justificativa]
    EXEC --> FECHA[Registra decisão no protocolo + Pipefy\ne encerra o caso]
    RETORNO --> FECHA
```

## 4. Coleta no intake (o que o agente/CX precisa evidenciar)

O objetivo é entender a **causa raiz** e reunir tudo que permita a decisão — **evidenciando** cada ponto.

### 4.1 Dados obrigatórios do caso
- **Cliente**: nome/razão social e **CNPJ** (ou CPF no B2C).
- **Pedido relacionado** à reclamação (nº do pedido) e **valor** envolvido.
- **Comprador / responsável** e contato (no B2B, dossiê do cliente).
- **Causa raiz** (perguntas abertas — seção 4.2).
- **Fatos que comprovem** (evidências — seção 4.3), quando houver.
- **Sugestão de ação** do solicitante/CX (não é decisão — ver seção 6).
- **Canal de entrada** e data.

### 4.2 Causa raiz — perguntas abertas (exemplos)
O agente/CX conduz com perguntas abertas para chegar à origem do problema. O que geralmente o supervisor/representante ou o cliente traz:
- O que aconteceu, quando, e qual o impacto para o cliente?
- Foi problema de **entrega/logística**? (atraso de transportadora, **carga retida**, atraso dos Correios, processo de entrega do site não cumprido, **pagou frete mais rápido e não recebeu no prazo**)
- É questão **comercial/relacionamento**? (cliente com **estoque alto**, **estoque parado**, dificuldade de **sellout**, cliente com **bom relacionamento**, exceção de relacionamento)
- É **financeiro**? (necessidade de **prorrogação de pagamento de boleto**, negociação)
- Envolve **trade marketing**? (atraso em ação de trade)
- Há **prazo/urgência**?

### 4.3 Evidências (anexar quando houver)
Print/registro de rastreio, comprovante de frete pago, e-mails/mensagens, protocolo da transportadora, foto do produto/carga, extrato de pedidos/sellout, histórico de relacionamento. Anexos entram no protocolo (`ChamadoAnexo`) e no card do Pipefy.

### 4.4 Integrações reduzem perguntas (princípio — eleva o projeto)
**Muito importante**: sempre que uma integração puder trazer o dado, o agente **não pergunta** — puxa automaticamente e apenas confirma. Isso vale para cliente e para representante. Exemplos:
- `[[INT_ERP_B2B]]` / `[[INT_ERP_B2C]]` (Tray): número do pedido, valor, itens, status de faturamento, **boletos/títulos em aberto**.
- `[[INT_RASTREIO]]`: status de entrega, transportadora, previsão, atraso.
- `[[INT_HISTORICO]]`/`[[INT_PROTOCOLOS]]`: relacionamento, reclamações anteriores, protocolos abertos.
- `[[INT_CRM_B2B]]`: carteira, sellout, estoque do cliente (quando disponível).

Quanto mais integração conectada, **menos perguntas** e mais o caso já chega ao CX pré-instruído. Enquanto uma integração não existir, o agente pergunta (contingência).

## 5. Áreas de consulta (durante a investigação do CX)
Sistema · **Logística** · **Comercial** · **Financeiro/Faturamento** · **Jurídico** · **Board C-level**. O Pipefy organiza a passagem por cada área conforme o caso.

## 6. Decisão (governança)
- **Sugestão de ação**: pode vir do representante/supervisor, do cliente ou do próprio CX — é registrada **como sugestão**, nunca como aprovação.
- **Decisão final**: **CX + Head (C-level)**. Nenhum agente de IA e nenhuma área isolada decide.
- **Ações possíveis**: prorrogação de prazo de boleto · isenção de frete · cancelamento · estorno · devolução total ou parcial · bonificação · crédito (próxima compra) · negociação.
- **Condições de pagamento fora da Tabela Prazo x Valor 2026**: também são decisão deste processo. A IA (A7) só oferece condições **dentro** da tabela (artigo 07); qualquer condição fora dela — prazo acima do limite da faixa, parcelamento não listado, valor abaixo do mínimo fora da exceção de introdução/mix — só é autorizada aqui, com **aval do Head + CX**.

## 7. Papel da IA vs. humano
- A **IA acolhe, investiga a causa raiz, evidencia e registra** — e deixa **explícito** que quem dá o veredito final é o **CX junto com o Head**.
- A IA **nunca** promete, aprova ou nega qualquer ação financeira.
- A IA abre o protocolo, aciona o Workflow e transborda para a fila **CX Casos**.

## 8. Integrações e placeholders do processo
- `[[INT_PROTOCOLO_CRIAR]]` (I-19) — abre o protocolo (Origin conforme canal).
- `[[INT_PROTOCOLO_ATUALIZAR]]` (I-20) — `openWorkflow` + `WFObservacao` (resumo) + `DepartamentoID` da fila CX Casos (módulo WF).
- `[[INT_PIPEFY_CX]]` (I-21) — cria o card no pipe do CX e permite leitura das etapas.
- `[[INT_ERP_B2B]]`, `[[INT_ERP_B2C]]`, `[[INT_RASTREIO]]`, `[[INT_HISTORICO]]`, `[[INT_CRM_B2B]]` — reduzem perguntas (seção 4.4).

## 9. Dependências para ativar
- [ ] **Módulo Workflow da NeoAssist habilitado** (previsto para as próximas semanas).
- [ ] **Pipe do CX no Pipefy** criado conforme o blueprint `pipefy-cx-financeiro-blueprint.md` (7 fases, evidências por causa, gate do Head, dashboards de prestação de contas). Construção pelo Cowork: `../../implantacao/pacote-cowork-build-pipefy-cx.md`.
- [ ] Fila/categoria **CX Casos** e `DepartamentoID` do WF (documento 05).
- [ ] Integrações da seção 4.4 conectadas (incrementais — o processo funciona com contingência até lá).
