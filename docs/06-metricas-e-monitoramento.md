# 06 — Métricas, curadoria e melhoria contínua

## 1. KPIs do canal

| Métrica | Definição | Meta inicial sugerida |
|---------|-----------|----------------------|
| **Taxa de retenção na IA** | Atendimentos resolvidos sem transbordo ÷ total | ≥ 60% (crescer com maturidade) |
| **Taxa de resolução declarada** | Respostas positivas à pergunta de resolução ÷ encerramentos pela IA | ≥ 80% |
| **Tempo de primeira resposta** | Chegada da mensagem → primeira resposta da IA | < 10 s |
| **Taxa de triagem correta** | Conversas roteadas ao agente certo (auditoria amostral) | ≥ 90% |
| **Transbordos por motivo** | Distribuição T1–T8 (documento 05) | Monitorar T3 e T6 (lacunas de base) |
| **Conversão assistida (A2)** | Prospects que concluíram compra após atendimento | Baseline no 1º mês |
| **Leads B2B qualificados (A5/A6)** | Cadastros com CNPJ validado entregues ao comercial | Baseline no 1º mês |
| **Cadastros de afiliados (A9)** | Solicitações completas encaminhadas | Baseline no 1º mês |

## 2. Curadoria (rotina semanal)

1. Revisar amostra de conversas por agente, priorizando transbordos **T3** (incompreensão) e **T6** (baixa confiança) — são as lacunas da base de conhecimento.
2. Atualizar artigos da base oficial; a assistente só melhora pelo conteúdo oficial (regra do documento 01: nunca inventa).
3. Revisar falsos positivos/negativos do roteador de perfis e ajustar exemplos de intenção.
4. Auditar aderência aos guardrails (documento 04): amostrar conversas com termos sensíveis ("cura", "emagrece", "remédio", "processo", "CPF").

## 3. Alertas operacionais

- Pico de transbordo T2 (irritação) → possível incidente (atrasos em massa, site fora do ar).
- Falha recorrente de integração (documento 03, regra 4) → acionar TI; fluxo segue em contingência.
- Fila humana sem resposta dentro do prazo prometido → alerta ao supervisor (a promessa de retorno é vinculante — CDC).

## 4. Governança de mudanças

- Toda alteração de persona, guardrail ou fluxo passa por revisão de compliance (referência: documento 04) antes de publicar.
- Versões dos documentos deste repositório são a fonte da verdade; a configuração na NeoAssist deve refletir a versão vigente aqui.
