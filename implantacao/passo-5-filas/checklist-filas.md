# Passo 5 — Checklist de filas humanas

> Regras completas em `../../docs/05-transbordo-humano-e-filas.md`. Nenhum agente é publicado sem sua fila criada — transferência nunca é negada.

## Configuração geral (todas as filas)

- [ ] Horário: **segunda a sexta, 9h–18h**.
- [ ] Calendário de exceções: **feriados nacionais + feriados da cidade de São Paulo**.
- [ ] Fora do horário: registrar protocolo + mensagem padrão do Artigo 04 (retorno até o fim do próximo dia útil).
- [ ] Ticket de transbordo recebe automaticamente: perfil (A1–A9), variáveis coletadas, resumo da conversa e motivo (T1–T8).

## Filas a criar

- [ ] **Consumidor** — recebe A1, A2, A3, A4 (pedidos, entregas, trocas, produtos, fluxo mestre sem consentimento).
- [ ] **Financeiro Consumidor** — recebe A1, A3 (reembolso, estorno, pagamento).
- [ ] **B2B Farma SP** — recebe A5 e A7 (categoria farma SP).
- [ ] **B2B Nacional** — recebe A6 e A7 (demais canais).
- [ ] **Parcerias Profissionais** — recebe A8.
- [ ] **Marketing / Afiliados** — recebe A9.
- [ ] **Privacidade / DPO** — recebe todos (LGPD e direitos de titular).
- [ ] **Jurídico / Ouvidoria** — recebe todos (Procon, Reclame Aqui, advogado, processo, imprensa).
- [ ] **Cobrança / Crédito** — recebe A5, A6 (negociação de dívida, crédito, cobrança).

## Vinculação agente → fila padrão

| Agente | Fila padrão | Filas secundárias |
|--------|-------------|-------------------|
| A1 | Consumidor | Financeiro Consumidor |
| A2 | Consumidor | — |
| A3 | Consumidor | Financeiro Consumidor |
| A4 | Consumidor (qualidade com prioridade) | — |
| A5 | B2B Farma SP | Cobrança / Crédito |
| A6 | B2B Nacional | Cobrança / Crédito |
| A7 | B2B Farma SP ou B2B Nacional (pela categoria) | Gestão comercial (comissões/contrato) |
| A8 | Parcerias Profissionais | — |
| A9 | Marketing / Afiliados | — |
| Todos | Privacidade / DPO e Jurídico / Ouvidoria (assuntos restritos, ramo N19) | — |

## Testes de aceitação (antes do go-live)

- [ ] T1: pedir "quero falar com um humano" em cada agente → transferência na primeira vez.
- [ ] T2: simular irritação (caixa alta / cobranças curtas) → transferência com mensagem de reconhecimento.
- [ ] T3: duas mensagens incompreensíveis → oferta de transferência.
- [ ] T4: cobrar o mesmo atraso de entrega pela 2ª vez → transferência direta.
- [ ] T5: citar "Procon" / "advogado" / "LGPD" / "imprensa" → transferência imediata para a fila correta.
- [ ] T7: responder "não resolveu" à pergunta de encerramento → oferta imediata de transferência.
- [ ] Fora do horário: verificar protocolo + mensagem de prazo de retorno.
