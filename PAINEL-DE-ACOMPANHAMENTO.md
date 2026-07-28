# Painel de acompanhamento — Projeto IA FTW no WhatsApp

> Fonte única de status do projeto: o que está **pronto na documentação**, o que depende de **execução na plataforma (Cowork)** e o que depende de **material/decisão** de terceiros. Atualizado conforme o projeto evolui.

## 1. Visão geral

- **Canal**: WhatsApp Consumidor **(11) 2388-3360** · Plataforma **NeoAssist** (Núb.ia Resolve).
- **11 agentes** (A1–A11) + fluxo mestre + 7 artigos de base + 21 integrações mapeadas (`[[INT_*]]`).
- Toda a documentação está **pronta para colar/configurar**; a execução na plataforma é feita pelo **Claude Cowork** (este ambiente não acessa a NeoAssist).

## 2. Agentes (documentação pronta)

| Agente | Perfil | Fila | Observação |
|--------|--------|------|------------|
| A1 | Cliente Site FTW | Consumidor | Pós-compra; Tray (I-04) |
| A2 | Prospect Consumidor | Consumidor | Conversão |
| A3 | Cliente Marketplace | Consumidor | Regras por marketplace (art. 05) |
| A4 | Cliente PDV | Consumidor | Lote + NF |
| A5 | B2B Farma SP | B2B Farma SP | Tabela Prazo x Valor (art. 07 via A7) |
| A6 | B2B Nacional | B2B Nacional | Por canal |
| A7 | Representantes | B2B / JL Educa / Trade | Menu de assuntos; art. 07 |
| A8 | Profissionais de Saúde | Parcerias | CRM/CRN |
| A9 | Creators (Influ/UGC/Afiliado) | Marketing/Afiliados | Tags + Sheets (I-18) |
| A10 | Terceirização | Terceirização | Lead + orçamento |
| A11 | CX Caso Financeiro | CX Casos | Workflow → Pipefy; decisão CX+Head |

Todos com **Regra Anti-Espera** e **Silêncio Zero**.

## 3. Pendências de EXECUÇÃO (Claude Cowork) — pacotes prontos

| Item | Pacote | Status |
|------|--------|--------|
| Atualizar os 11 agentes + testar | `implantacao/pacote-cowork-atualizacao-agentes.md` | ⏳ a executar |
| IDs/chaves NeoAssist (destrava protocolo/WF) | `implantacao/pacote-cowork-neoassist-nativas.md` | ⏳ **P0** |
| Integração Tray (I-04) | `implantacao/pacote-cowork-tray-a1.md` | ⏳ **P0** |
| ERP/CRM B2B JL FIT (I-07/08) | `implantacao/pacote-cowork-erp-crm-b2b.md` | ⏳ descoberta |
| Construir pipe financeiro do CX | `implantacao/pacote-cowork-build-pipefy-cx.md` | ⏳ a construir |
| Pipefy Trade + CX (I-17/21) | `implantacao/pacote-cowork-pipefy.md` | ⏳ |
| Representantes/Parceiros (I-09/11) | `implantacao/pacote-cowork-representantes-parceiros.md` | ⏳ |
| Afiliados (I-10) | `implantacao/pacote-cowork-afiliados.md` | ⏳ |
| Calendário JL Educa (I-16) | `implantacao/pacote-cowork-jl-educa-calendario.md` | ⏳ |
| Utilitárias (CNPJ/rastreio/pagamentos/PDV) | `implantacao/pacote-cowork-utilitarias.md` | ⏳ |

Índice: `implantacao/pacotes-cowork-indice.md`.

## 4. Pendências de MATERIAL / DECISÃO (terceiros)

| Item | Depende de | Vira |
|------|-----------|------|
| **Nome oficial da persona** | Definição interna | Substituir `[Assistente de IA Fitoway]` |
| **Aviso de Privacidade validado + URL** | Jurídico/DPO | Substituir `[LINK_CONSENTIMENTO]`; campos 【CONFIRMAR】 |
| **Módulo Workflow NeoAssist** | NeoAssist (previsto) | Ativa A11/CX Casos pleno |
| **Doc oficial da Tray** | Cowork ler / export | Fecha contrato I-04 |
| **Sistema ERP/CRM da JL FIT** | Você indicar | Preenche pacote I-07/08 |
| **Regulamento do programa de afiliados** | Marketing | Artigo da base (A9) |
| **FAQ atualizada de terceirização** | Time terceirização | Artigo da base (A10) |
| **Estrutura dos pipes Pipefy** (Trade e CX) | Após construção | Mapeamento `pipe_id`/`field_id` |

## 5. Decisões de negócio já registradas

- Afiliados: programa **ativo/cadastro completo** no WhatsApp.
- Marketplace: mesmo em loja oficial, tratativa é do marketplace (comunicar com delicadeza).
- Identificação **única e persistente** (telefone/CPF/CNPJ validado uma vez).
- LGPD por **transparência** (não gate de consentimento); opt-in só para marketing.
- Tabela Prazo x Valor 2026 no A7; **exceções só via Caso CX (Pipefy) com aval do Head**.
- Casos com decisão financeira: **decisão sempre CX + Head**; IA nunca decide.
- Cálculo do impacto financeiro: tudo a **valor de venda** (sem CMV); custo do prazo com **taxa 2% a.m.** (ajustável pelo Financeiro).

## 6. Histórico de entregas (PRs mesclados)

Documentação-base → pacote de implantação → marketplace → T9/silêncio → placeholders/identificação → LGPD → A7 (CX/JL Educa) → A10 → A9 Creators → Sheets → APIs NeoAssist → processo Caso CX/A11 → Regra Anti-Espera → pacotes Cowork (todas as integrações) → Tabela Prazo x Valor → blueprint Pipefy CX.

## 7. Próximos passos sugeridos (ordem)

1. Cowork: **atualizar os 11 agentes + rodar os testes** (garante o comportamento anti-espera ao vivo).
2. Cowork: **IDs/chaves NeoAssist** (P0) e **Tray** (P0).
3. Definir **ERP/CRM da JL FIT** → gerar/rodar o pacote.
4. Cowork: **construir o pipe financeiro do CX** no Pipefy.
5. Trazer **materiais** (afiliados, FAQ terceirização) e **habilitar Workflow**.
