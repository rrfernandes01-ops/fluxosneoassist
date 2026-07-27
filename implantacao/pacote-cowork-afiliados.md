# Pacote Cowork — Plataforma de Afiliados (agente A9)

> **Para o Claude Cowork** (ambiente do usuário). Integração `[[INT_AFILIADOS]]` (I-10) do agente **A9** (Creators): cadastro de afiliado, status de aprovação, geração de link/cupom e painel de comissões. Hoje o A9 coleta e grava no Google Sheets (I-18) e transborda; esta integração conecta a plataforma oficial de afiliados.

## 1. Descobrir a plataforma de afiliados (com o usuário)

Identificar **qual** plataforma a FTW usa (ou usará) para o programa de afiliados — ex.: solução nativa do e-commerce (Tray), Lomadee/Awin/outra rede, ou plataforma dedicada. O caminho depende disso.

## 2. Ler a documentação da API

Capturar, na doc da plataforma escolhida:
- **Autenticação** (token/OAuth).
- **Criar/cadastrar afiliado** (dados: nome, CPF/CNPJ, redes/handles, audiência, e-mail, WhatsApp).
- **Status de aprovação** do cadastro.
- **Gerar/consultar link e cupom** do afiliado.
- **Painel de comissões** (consulta por afiliado — dado financeiro pessoal; exige validação de identidade do titular).
- **Credenciais**: o usuário fornece; **não versionar**.

## 3. Regras herdadas (manter do A9)

- Afiliado tratado como **programa ativo/cadastro completo**.
- Não negociar percentuais/condições (alçada Marketing); não projetar ganhos; não dar comissão sem validar identidade do titular.
- Reforço CONAR (#publi) e ANVISA (sem alegações) ao entregar link/cupom.
- Vincular também ao **regulamento oficial do programa** (artigo da base a incorporar — pendente).

## 4. Entregável

**Atualizar o repositório**: criar `docs/integracoes/plataforma-afiliados.md` com a plataforma, endpoints e mapeamento; atualizar o contrato I-10 em `docs/03-integracoes.md`. Abrir PR na branch `claude/fitoway-whatsapp-ai-x5uzrt`, **ou** reportar ao usuário.

## 5. Testar (após conectar)

- No A9, perfil Afiliado → cadastro cria/registra na plataforma e retorna status; afiliado ativo → link/cupom e painel com validação de identidade.
- Gravação no Sheets (I-18) segue em paralelo para o Marketing.
- Timeout/silêncio zero e Regra Anti-Espera na contingência.

## 6. Reportar
Plataforma identificada, endpoints, mapeamento (link do PR ou lista) e resultado dos testes.
