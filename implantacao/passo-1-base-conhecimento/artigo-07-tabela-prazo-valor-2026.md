# Artigo 07 — Tabela Prazo x Valor 2026 (condições de pagamento B2B)

> **Como cadastrar**: Base de conhecimento → novo artigo → título "Tabela Prazo x Valor 2026" → colar o conteúdo abaixo → vincular ao agente **A7 (Representantes)** (e opcionalmente aos A5/A6 se forem informar condições de pagamento a clientes B2B validados).
>
> **Curadoria**: a tabela é anual — revisar e atualizar a versão a cada ciclo (título indica o ano vigente).

---

## Regras gerais de negociação

- **Tabela de referência**: TABELA PRAZO x VALOR 2026.
- **Prazo máximo permitido**: 90 dias direto.
- **Valor mínimo para pedido / parcela mínima**: R$ 250,00.
- **Exceção do valor mínimo**: a regra de R$ 250,00 vale **somente** para introdução de cliente ou mix de produtos.
- **Definição de ID (importante)**: o número listado como "ID" em cada opção de parcelamento refere-se **estritamente ao ID do método de pagamento cadastrado no sistema SAP**. Utilize esse código para registrar ou consultar a condição de pagamento correspondente.

## Como escolher a faixa

A faixa é definida pelo **valor total da venda**. Cada faixa tem um **prazo limite de recebimento** e uma lista de condições (ID → parcelamento). Só ofereça condições **dentro da faixa** do valor do pedido.

## FAIXA A — Vendas de R$ 250,00 a R$ 500,00 (prazo limite: 45 dias)

| ID | Parcelamento |
|----|--------------|
| 1 | 7 dias (1 parcela) |
| 2 | 7 e 14 dias (2 parcelas) |
| 33 | 15 dias (1 parcela) |
| 34 | 15 e 30 dias (2 parcelas) |
| 7 | 21 dias (1 parcela) |
| 38 | 21 e 28 dias (2 parcelas) |
| 41 | 28 dias (1 parcela) |
| 42 | 28 e 35 dias (2 parcelas) |
| 45 | 30 dias (1 parcela) |
| 65 | 30 e 40 dias (2 parcelas) |
| 46 | 30 e 45 dias (2 parcelas) |

## FAIXA B — Vendas de R$ 501,00 a R$ 1.000,00 (prazo limite: 60 dias)

| ID | Parcelamento |
|----|--------------|
| 6 | 7, 14 e 21 dias (3 parcelas) |
| 83 | 7, 30 e 60 dias (3 parcelas) |
| 31 | 14, 21 e 28 dias (3 parcelas) |
| 36 | 15, 30 e 45 dias (3 parcelas) |
| 26 | 20, 30 e 40 dias (3 parcelas) |
| 47 | 30, 45 e 60 dias (3 parcelas) |
| 50 | 45 e 60 dias (2 parcelas) |
| 53 | 50 dias (1 parcela) |
| 51 | 60 dias (1 parcela) |
| 49 | 45 dias (1 parcela) |
| 48 | 30 e 60 dias (2 parcelas) |
| 70 | 35 dias (1 parcela) |
| 54 | 40 dias (4 parcelas) |

## FAIXA C — Vendas de R$ 1.001,00 a R$ 1.500,00 (prazo limite: 75 dias)

| ID | Parcelamento |
|----|--------------|
| 61 | 30, 45, 60 e 75 dias (4 parcelas) |
| 72 | 45, 60 e 75 dias (3 parcelas) |
| 84 | 60 e 75 dias (2 parcelas) |

## FAIXA D — Vendas acima de R$ 1.500,00 (prazo limite: 90 dias)

| ID | Parcelamento |
|----|--------------|
| 134 | 7, 30, 60 e 90 dias (4 parcelas) |
| 94 | 20, 35, 50, 65 e 80 dias (5 parcelas) |
| 57 | 30, 45, 60, 75 e 90 dias (5 parcelas) |
| 58 | 30, 60 e 90 dias (3 parcelas) |
| 87 | 45, 60, 75 e 90 dias (4 parcelas) |
| 99 | 60, 75 e 90 dias (3 parcelas) |
| 35 | 15, 30, 45, 60, 75 e 90 dias (6 parcelas) |
| 74 | 30, 60, 75 e 90 dias (4 parcelas) |

## Condições fora da tabela (exceção)

Qualquer condição **fora desta tabela** (prazo acima do limite da faixa, parcelamento não listado, valor abaixo do mínimo fora da exceção de introdução/mix) **não é oferecida nem prometida pela IA**. Ela só pode ser autorizada **dentro do processo de Caso CX (Pipefy)**, com **decisão do Head (aval do Head) em conjunto com o CX**, e apenas para os casos de decisão financeira já definidos (prorrogação de boleto, isenção de frete, cancelamento, estorno, devolução, bonificação, crédito, negociação — ver processo de Caso CX). A IA, ao receber um pedido de exceção, registra o caso e encaminha para o CX Casos — nunca decide.
