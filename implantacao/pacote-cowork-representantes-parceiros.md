# Pacote Cowork — Base de Representantes (A7) e Base de Profissionais Parceiros (A8)

> **Para o Claude Cowork** (ambiente do usuário). Duas integrações de **validação/cadastro em bases internas**:
> - `[[INT_REPRESENTANTES]]` (I-09) — agente **A7**: validar que quem fala é um **representante ativo** (fase 1: primeiro nome + CPF), retornando categoria (Farma SP × demais canais) e carteira.
> - `[[INT_PARCEIROS]]` (I-11) — agente **A8**: cadastro e **validação de registro profissional** (CRM/CRN) dos profissionais de saúde parceiros.

## 1. Descobrir os sistemas de origem (com o usuário)

Estas são **bases internas** — não têm doc pública. Levantar com o usuário/TI:
- **Representantes**: onde está a base (ERP, CRM, planilha, sistema comercial próprio)? Há API? Campos: código do representante, CPF, nome, categoria, região, carteira, status (ativo).
- **Profissionais parceiros**: onde ficará o cadastro (CRM, base própria)? Campos: nome, profissão, CRM/CRN, UF, especialidade, contato, status da parceria. Há validação externa de CRM/CRN (site do conselho) ou só cadastro interno?

## 2. Definir o método de integração

Para cada base, com a TI:
- **API REST/GraphQL** (preferível): endpoints de consulta/validação e de cadastro.
- Se não houver API: alternativa (webhook, consulta a planilha/Sheets, middleware) — documentar a contingência.
- **Credenciais/acesso**: o usuário fornece; **não versionar**.

## 3. Contratos esperados

### 3.1 Representantes (I-09) — validação A7 (fase 1)
- **Entrada**: primeiro nome + CPF.
- **Saída**: `valido (bool)`, `categoria` (Farma SP / demais canais), `carteira`, `status`.
- **Fase 2 (futura)**: validação reforçada — documentar quando definida.
- Sem a base: A7 transborda direto para a gestão comercial (contingência atual).

### 3.2 Profissionais parceiros (I-11) — A8
- **Cadastro**: nome, profissão, CRM/CRN, UF, especialidade, contato.
- **Validação**: do registro no conselho e/ou status da parceria.
- **Consulta de status** do cadastro.
- Sem a base: A8 coleta cadastro completo + protocolo + transbordo (Parcerias).

## 4. Guardrails a preservar

- **A7**: sem dados fora da carteira do representante; comissões/contrato → humano; segregação por categoria.
- **A8**: nunca material técnico de prescrição sem registro validado; vedação ética (CFM/CFN) — sem vantagem por prescrição; LGPD no dado do registro profissional.

## 5. Entregável

**Atualizar o repositório**: criar `docs/integracoes/base-representantes.md` e `docs/integracoes/base-parceiros.md` com sistema de origem, método, endpoints/contrato e contingência; atualizar I-09 e I-11 em `docs/03-integracoes.md`. Abrir PR na branch `claude/fitoway-whatsapp-ai-x5uzrt`, **ou** reportar ao usuário.

## 6. Testar (após conectar)

- A7: validar um representante real (nome+CPF) → retorna categoria/carteira; representante inexistente → não fornece dados, transborda.
- A8: cadastrar um profissional → registra e retorna status; consultar status depois.
- Timeout/silêncio zero e Regra Anti-Espera na contingência.

## 7. Reportar
Sistemas de origem, método de integração, contratos (link do PR ou lista) e resultado dos testes.
