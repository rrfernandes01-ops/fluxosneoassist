# Passo 4 — Checklist de conexão das integrações

> Contratos completos, regras de uso e o **mapa de placeholders `[[INT_*]]`** em `../../docs/03-integracoes.md`. O fluxo funciona desde o dia 1 com as contingências; cada integração conectada substitui a contingência correspondente. Conectar na ordem de prioridade. As documentações técnicas das integrações ainda serão fornecidas — ao chegar cada uma: atualizar o contrato no doc 03, substituir o placeholder pelo conector real na plataforma e rodar os testes de aceitação.

## Onda 1 — P0 (necessárias para o go-live pleno)

- [ ] **I-01 — Consumidor por telefone (NeoAssist)**: consulta do cadastro pelo número do WhatsApp. *Sem ela*: todo contato passa pelo subfluxo de cadastro (N10–N13).
- [ ] **I-02 — Histórico de protocolos (NeoAssist)**: leitura dos últimos protocolos para continuidade. *Sem ela*: pular oferta de continuidade (N06–N08).
- [ ] **I-03 — Transparência e opt-in LGPD**: log da exibição do aviso de transparência (data/hora + versão) e registro do opt-in/opt-out de marketing (`optin_marketing`). *Sem ela*: registrar como nota/tag no protocolo (provisório, priorizar).
- [ ] **I-04 — E-commerce FTW**: pedidos, pagamento, NF, trocas. *Sem ela*: A1 coleta nº do pedido + CPF, registra protocolo e transborda.
- [ ] **I-05 — Rastreio (transportadoras)**: eventos e previsão de entrega. *Sem ela*: informar código e link público da transportadora.
- [ ] **I-06 — Catálogo de produtos**: fichas, alegações aprovadas, preço e estoque do site. *Sem ela*: A2 direciona ao site; dúvidas técnicas fora da base → transbordo (T6). **Crítica para compliance ANVISA** — priorizar as fichas na base de conhecimento mesmo antes da API.

## Onda 2 — P1 (destravam o B2B e os programas)

- [ ] **I-07 — ERP/faturamento (JL FIT e FTW B2B)**: pedidos, títulos, tabelas por canal → A5, A6, A7.
- [ ] **I-08 — CRM comercial B2B**: registro de leads, carteira por representante → A5, A6, A7.
- [ ] **I-09 — Base de representantes**: validação de vínculo/categoria/carteira → A7. *Sem ela*: A7 transborda direto.
- [ ] **I-10 — Plataforma de afiliados**: cadastro, aprovação, link/cupom, painel → A9.
- [ ] **I-11 — Base de profissionais parceiros**: cadastro e validação CRM/CRN → A8.

## Onda 3 — P2 (refinamentos)

- [ ] **I-12 — Consulta CNPJ**: validação de CNPJ/UF na triagem B2B.
- [ ] **I-13 — Pagamentos (gateway)**: status e reenvio de boleto/Pix → A1.
- [ ] **I-14 — Agenda de visitas**: agendamento de representante → A5, A6.
- [ ] **I-15 — Localizador de PDVs/lojas oficiais** → A2, A4.

## Regras ao conectar cada integração

1. Validação de identidade **antes** de qualquer consulta que retorne dado pessoal/financeiro.
2. Dados sempre mascarados na conversa.
3. Erro/timeout → a assistente não inventa: protocolo + régua de transbordo.
4. Toda escrita retorna confirmação com protocolo.
5. Log de chamadas vinculado ao protocolo (auditoria LGPD).
6. Ao ativar, testar o caminho feliz **e** a contingência (desligar e religar).
