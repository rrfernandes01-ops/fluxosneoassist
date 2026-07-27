# Pacote Cowork — ERP e CRM B2B (JL FIT / FTW)

> **Para o Claude Cowork** (ambiente do usuário, com acesso à TI/sistemas). Duas integrações que sustentam todo o atendimento B2B:
> - `[[INT_ERP_B2B]]` (I-07) — ERP/faturamento da **JL FIT** (e FTW B2B): pedidos, faturamento, títulos/boletos, tabelas por canal.
> - `[[INT_CRM_B2B]]` (I-08) — CRM comercial: cadastro de leads, carteira por representante, status de crédito (só sinalização).
>
> Usadas por **A5** (B2B Farma SP), **A6** (B2B Nacional), **A7** (Representantes) e **A11** (Caso CX financeiro — puxar pedido/valor/boleto para reduzir perguntas).

## 1. Descobrir os sistemas (com o usuário/TI) — passo obrigatório

Estes são **sistemas internos**; não há doc pública. Levantar com a TI da JL FIT/FTW:
- **Qual ERP** a JL FIT usa (ex.: Totvs/Protheus, SAP, Bling, Omie, Sankhya, Tiny, sistema próprio…)?
- **Qual CRM** comercial (ex.: nativo do ERP, Salesforce, RD Station, Pipedrive, HubSpot, planilha…)?
- Há **API** (REST/GraphQL/SOAP)? Documentação interna? Ambiente de homologação?
- Quem é o responsável técnico/parceiro de integração?
- Como é a **autenticação** (token, OAuth, usuário/senha de serviço)? — **credenciais não vão para o repositório**.

## 2. Necessidades funcionais (o que os agentes precisam)

### 2.1 ERP B2B (I-07)
| Necessidade | Usado por |
|-------------|-----------|
| Buscar **cliente por CNPJ** (razão social, situação, canal) | A5, A6, A7, A11 |
| **Pedidos** do cliente: nº, status, itens, valor, faturamento, previsão de entrega | A5, A6, A7, A11 |
| **Títulos/boletos em aberto** (vencimento, valor, 2ª via) — sem negociar | A5, A6, A11 |
| **Tabela de preço/condições por canal** (farma SP, alimentar, varejo, BodyShop, canal verde, farma nacional) — só para cliente validado | A5, A6 |
| **Nota fiscal** do pedido | A5, A6, A11 |

### 2.2 CRM B2B (I-08)
| Necessidade | Usado por |
|-------------|-----------|
| **Registrar lead** novo (CNPJ, razão social, canal, cidade/UF, responsável, contato) | A5, A6, A10 |
| **Carteira por representante** (clientes vinculados) | A7 |
| **Oportunidades** (clientes sem compra recente, sugestões de mix) | A7 |
| **Campanhas** vigentes por canal | A7 |
| **Sellout / estoque do cliente** (quando disponível) | A11 (reduz perguntas) |
| Status de crédito — **apenas sinalização** (negociação é humana) | A5, A6 |

## 3. Contratos esperados (a preencher após a doc da TI)

- **`GET cliente por CNPJ`** → dados cadastrais + canal + status.
- **`GET pedidos por CNPJ`** → lista com status, valor, faturamento, NF, rastreio/previsão.
- **`GET títulos por CNPJ`** → boletos em aberto (vencimento, valor, link 2ª via).
- **`GET tabela por canal`** → condições vigentes (restrito a cliente validado).
- **`POST lead`** (CRM) → cria o lead com o canal marcado; retorna protocolo/ID.
- **`GET carteira por representante`** → clientes, desempenho, oportunidades, campanhas.
- Definir paginação, limites e formato de resposta reais.

## 4. Regras e guardrails a preservar

- **Nenhum dado comercial antes da validação do CNPJ** (sigilo comercial) — A5/A6.
- **Segregação por canal**: não expor condição de um canal para cliente de outro.
- **A7**: só dados da **carteira** do representante validado; comissões/contrato → humano.
- **Negociação de dívida/crédito** → transbordo (Cobrança/Crédito); a IA não negocia.
- **Caso com decisão financeira (A11)**: o ERP/CRM serve para **puxar pedido, valor, boleto, sellout** e reduzir perguntas — a decisão segue sendo CX + Head.
- Dados mascarados; LGPD; **credenciais nunca versionadas**.

## 5. Entregável

**Atualizar o repositório**: criar `docs/integracoes/erp-crm-b2b.md` com o sistema identificado, método de integração, endpoints/contratos e contingência; atualizar os contratos I-07 e I-08 em `docs/03-integracoes.md`. Abrir PR na branch `claude/fitoway-whatsapp-ai-x5uzrt`, **ou** reportar ao usuário para o Claude Code registrar.

## 6. Testar (após conectar)

- A5/A6: validar CNPJ real → puxa pedidos, títulos e tabela do canal; cliente novo → cria lead no CRM.
- A7: consultar carteira de um representante validado → desempenho/oportunidades/campanhas.
- A11: abrir um caso → o agente já traz pedido/valor/boleto sem perguntar.
- **Sem a integração**: confirmar que os agentes caem na contingência (pedem o dado ou transbordam) sem deixar o cliente esperando (Regra Anti-Espera).
- Configurar **timeout do nó** (10 s + 1 retentativa + ramo de erro — doc 05, seção 2.2).

## 7. Reportar
Sistemas identificados (ERP e CRM), método de integração, endpoints/contratos (link do PR ou lista), e resultado dos testes por agente.
