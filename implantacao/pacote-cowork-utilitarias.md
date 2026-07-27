# Pacote Cowork — Integrações utilitárias (CNPJ, rastreio, pagamentos, PDV)

> **Para o Claude Cowork** (ambiente do usuário). Integrações de apoio, de menor prioridade (P2), que refinam vários agentes:
> - `[[INT_CNPJ]]` (I-12) — validar CNPJ/UF na triagem B2B (A5/A6, fluxo mestre).
> - `[[INT_RASTREIO]]` (I-05) — rastreamento de entregas (A1/A5/A6/A11).
> - `[[INT_PAGAMENTOS]]` (I-13) — status/2ª via de boleto/Pix (A1) — pode vir da Tray.
> - `[[INT_PDV]]` (I-15) — localizador de PDVs/lojas oficiais (A2/A4).

## 1. CNPJ (I-12)

- Escolher a fonte: **Receita/BrasilAPI** (<https://brasilapi.com.br/docs> → `/cnpj/v1/{cnpj}`) ou serviço pago. Capturar endpoint, limites de uso e campos (razão social, situação, CNAE, UF).
- Uso: validar CNPJ e UF na triagem farma (SP → A5; senão A6) e no cadastro B2B.

## 2. Rastreio (I-05)

- Definir a origem: dado de rastreio **da Tray/pedido** (código + transportadora) e/ou API da transportadora/agregador de rastreio.
- Capturar: entrada (código de rastreio), saída (eventos + previsão).
- Contingência atual: informar o código e o link público da transportadora.

## 3. Pagamentos (I-13)

- Verificar se o **status de pagamento e 2ª via** (boleto/Pix) vêm da **Tray** (ver `pacote-cowork-tray-a1.md`) ou de um gateway específico.
- **Sem dados de cartão** — apenas status e reenvio de boleto/Pix.

## 4. PDV / lojas oficiais (I-15)

- Definir a fonte da lista de **PDVs e lojas oficiais** (planilha, base própria, API). Capturar como consultar por CEP/cidade.
- Uso: A2 (onde comprar) e A4 (cliente de PDV).

## 5. Regras comuns

- **Credenciais** (se houver): o usuário fornece; **não versionar**.
- Timeout/silêncio zero no nó (doc 05, seção 2.2) e Regra Anti-Espera na contingência.
- Dados mascarados; validação antes de expor dado pessoal.

## 6. Entregável

**Atualizar o repositório**: registrar as fontes e endpoints escolhidos nos contratos I-05, I-12, I-13, I-15 de `docs/03-integracoes.md` (e docs em `docs/integracoes/` se úteis). Abrir PR na branch `claude/fitoway-whatsapp-ai-x5uzrt`, **ou** reportar ao usuário.

## 7. Reportar
Fontes escolhidas, endpoints e resultado dos testes por integração.
