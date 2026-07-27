# Roteiro de testes dos agentes (na plataforma NeoAssist)

> Para executar **na plataforma** (pelo Cowork ou manualmente), após colar a versão atualizada dos 11 prompts. Foco principal: garantir que **nenhum agente deixa o cliente esperando** quando a integração não está conectada, e que o **transbordo para o operador** sempre funciona.

## Pré-condição

- [ ] Recolar os **11 prompts** atualizados (`passo-3-agentes/prompt-A1.md` … `prompt-A11.md`) — todos agora contêm a **REGRA ANTI-ESPERA**.
- [ ] Recolar o **artigo 02** da base de conhecimento (contém a regra anti-espera para todos).
- [ ] Confirmar que cada agente tem a **fila de transbordo** vinculada (passo 5).

## Teste-chave (roda em TODOS os 11 agentes) — sem integração conectada

Cenário: as integrações estão como placeholder (não conectadas), como na versão atual.

1. Provocar o agente a precisar de um dado de sistema (ex.: "qual o status do meu pedido?", "meu boleto", "minha carteira").
2. **Resultado esperado**:
   - O agente **não** responde "vou consultar / já te retorno" e some.
   - Ele **pede o dado à pessoa** (ex.: número do pedido + CPF) **ou** informa que vai transferir e **faz o transbordo na hora**, com o resumo.
   - A conversa **nunca fica sem resposta**.
3. **Falhou se**: o agente prometeu uma consulta e não voltou, ou ficou em silêncio.

## Teste de transbordo (por agente)

- [ ] Pedir "quero falar com um atendente" → transfere na primeira vez (T1) para a fila correta do agente.
- [ ] Fornecer dado inexistente / dizer "não tenho o número" → 1 tentativa de ajuda e transbordo com o que foi coletado (T9).
- [ ] Fora do horário (seg–sex 9h–18h) → registra protocolo + mensagem de retorno.

## Checklist por agente (fluxo específico)

- [ ] **A1 — Site FTW**: status de pedido sem integração → pede nº do pedido + CPF ou transborda (fila Consumidor).
- [ ] **A2 — Prospect**: dúvida de produto fora da base → posiciona e transborda; não promete consulta.
- [ ] **A3 — Marketplace**: tratativa de pedido → orienta o passo a passo do marketplace correto; não "consulta" o marketplace.
- [ ] **A4 — PDV**: troca/qualidade → pede lote + NF; sem o dado, transborda.
- [ ] **A5 — B2B Farma SP**: pedido/boleto sem ERP → pede dado ou transborda (fila B2B Farma SP); caso financeiro → CX Casos.
- [ ] **A6 — B2B Nacional**: idem A5 por canal (fila B2B Nacional).
- [ ] **A7 — Representantes**: valida nome+CPF; menu de assuntos; caso financeiro → CX Casos; Trade → checklist completo.
- [ ] **A8 — Profissionais**: cadastro CRM/CRN; sem base de parceiros, coleta e transborda (Parcerias).
- [ ] **A9 — Creators**: escolhe formato, aplica tag, coleta; sem Sheets, registra e transborda (Marketing/Afiliados).
- [ ] **A10 — Terceirização**: coleta lead; não responde pela FAQ desatualizada; transborda (Terceirização).
- [ ] **A11 — CX Caso Financeiro**: causa raiz + evidências; deixa claro que a decisão é CX + Head; abre protocolo/transbordo (CX Casos).

## Observação sobre timeout técnico

A **regra anti-espera** (no prompt) resolve o comportamento mesmo **sem** configuração de plataforma. Quando as integrações forem conectadas, configurar **também** o timeout técnico do nó (10 s + 1 retentativa + ramo de erro) descrito no documento 05, seção 2.2 — as duas camadas juntas garantem silêncio zero.
