# Integração — Pipefy CX (`[[INT_PIPEFY_CX]]`, I-21)

> Pipe do processo `../processos/reclamacao-decisao-financeira.md`, desenhado em `../processos/pipefy-cx-financeiro-blueprint.md`. Alimentado pelo Workflow da NeoAssist (`openWorkflow`) e pelo agente A11 — e, por hand-off, pelos agentes A1, A5, A6 e A7 quando o atendimento evolui para um caso de decisão financeira.

## Pipe

- **Nome**: CX — Casos com Decisão Financeira
- **URL**: https://app.pipefy.com/pipes/307229758
- **pipe_id**: `307229758`
- **Cobertura**: B2B e B2C. Entradas típicas: A1 (B2C, site), A5/A6 (B2B direto, farma/alimentar/varejo/BodyShop/canal verde), A7 (representante em nome do cliente), e-mail, telefone.
- **field_ids do start form**: pendente. **Na próxima atuação do Cowork neste pipe, os field_ids devem ser mapeados E JÁ INTEGRADOS** (conexão real NeoAssist ↔ Pipefy funcionando — não apenas levantados/reportados em texto) — ver seção "Pendências" abaixo.

## Objetivo de negócio

Centralizar no CX toda questão comercial de pós-venda (cancelamento, estorno, prorrogação de boleto, troca, devolução, negociação, isenção de frete, bonificação, crédito), liberando o representante comercial (A7) para focar em venda. Nenhum agente de IA decide — decisão final é sempre CX + Head, conforme o gate de aprovação do pipe (fase 5).

## Diferença de evidência por canal (importante para o intake)

- **B2B**: pedido sempre faturado com boleto/título específico — coletar número e vencimento do boleto/título e da nota fiscal é esperado.
- **B2C**: majoritariamente pago por cartão ou Pix direto na compra — não pedir boleto sem antes confirmar a forma de pagamento pela integração do ERP (Tray, I-04). Pedir boleto de compra em cartão é fricção desnecessária.

## Pendências (próxima atuação do Cowork — não deixar para depois)

- [ ] Auditar o pipe existente contra o blueprint (fases, start form, checklist de evidências, gate do Head, dashboards) e completar o que faltar.
- [ ] **Mapear os `field_ids` do start form E integrá-los** — a integração real (não só o levantamento) é obrigatória nesta atuação: cada campo do start form precisa estar de fato conectado para receber os dados enviados pelo A11/Workflow.
- [ ] Definir e implementar o mecanismo de criação do card a partir do Workflow da NeoAssist (nativo, webhook ou middleware) — enquanto o Workflow não estiver habilitado, o CX cria o card manualmente a partir do protocolo, mas o mapeamento de campos já deve estar pronto para quando o Workflow for ligado.
- [ ] Atualizar este documento com os `field_ids` reais e o status da integração assim que concluída.
