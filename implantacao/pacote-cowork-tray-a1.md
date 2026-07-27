# Pacote Cowork — Tray (e-commerce FTW) para o agente A1

> **Para o Claude Cowork** (roda no ambiente do usuário, com acesso ao navegador). Objetivo: **ler a documentação oficial da Tray** — que o ambiente do Claude Code **não acessa** (rede bloqueia `developers.tray.com.br`) — extrair os endpoints e campos reais e **preencher o contrato da integração I-04** para o agente A1 automatizar o pós-compra (consumidor, pedidos, rastreio, pagamento).
>
> Referência do que já foi planejado (necessidades funcionais + pendências): `docs/integracoes/tray-ecommerce-a1.md`. Repositório: `rrfernandes01-ops/fluxosneoassist` (branch `main`).

## 0. Contexto

- Loja: e-commerce **FTW** (`www.ftw.com.br`) rodando na plataforma **Tray**.
- Documentação: <https://developers.tray.com.br/#novotray-api-plugin> (e seções de reference/API).
- Placeholder a resolver: **`[[INT_ERP_B2C]]`** (I-04); complementos `[[INT_RASTREIO]]` (I-05) e `[[INT_PAGAMENTOS]]` (I-13) se a Tray os cobrir.
- Credenciais da API (consumer key/secret): o **usuário fornece** (ou gera no painel Tray). Não expor credenciais em nenhum arquivo do repositório.

## 1. O que extrair da documentação da Tray

Abrir a doc e capturar, com **caminho exato, método HTTP, parâmetros e formato de resposta**:

### 1.1 Autenticação
- Endpoint(s) de autenticação (ex.: obter `access_token` a partir de `consumer_key` + `consumer_secret` + `code`).
- Validade do token e endpoint de **refresh**.
- Como o token é enviado nas chamadas (query string, header).

### 1.2 Pedidos (orders)
- **Listar pedidos** com filtros por: cliente/CPF, e-mail, número do pedido, data, status.
- **Detalhe do pedido**: status, itens, valores, forma de pagamento, status de pagamento, **nota fiscal**, transportadora, **código de rastreio**, previsão de entrega.
- Paginação.

### 1.3 Clientes/consumidores (customers)
- **Buscar** por CPF e por e-mail; **detalhe** por ID (nome, CPF, e-mail, telefone).

### 1.4 Produtos (products)
- **Detalhe** por ID: ficha, preço, estoque (para dúvidas de produto e recompra — A1/A2).

### 1.5 Rastreio e pagamento
- Se a Tray expõe rastreio/eventos de entrega e status de pagamento (boleto/Pix) — anotar; senão, indicar a fonte (transportadora/gateway) para `[[INT_RASTREIO]]`/`[[INT_PAGAMENTOS]]`.

## 2. Mapear para os campos-alvo do A1

Preencher o mapeamento (campo Tray → variável do A1), com base na seção "Campos-alvo" de `docs/integracoes/tray-ecommerce-a1.md`:

`pedido.id`, `pedido.status`, `pedido.itens[]`, `pedido.valor`, `pedido.pagamento.status`, `pedido.nota_fiscal`, `pedido.transportadora`, `pedido.codigo_rastreio`, `pedido.previsao_entrega`, `cliente.cpf`, `cliente.email`, `cliente.telefone`.

## 3. Entregável (uma das opções)

**Opção A (preferida) — atualizar o repositório:**
- Editar `docs/integracoes/tray-ecommerce-a1.md`: preencher os endpoints reais, o mapeamento de campos e o modelo de token; marcar as pendências como resolvidas.
- Atualizar em `docs/03-integracoes.md` o contrato da I-04 (entrada/saída/contingência).
- Abrir PR na branch `claude/fitoway-whatsapp-ai-x5uzrt` (ou informar ao usuário para o Claude Code fazer a edição a partir do conteúdo coletado).

**Opção B — reportar ao usuário:**
- Colar aqui, de forma estruturada, os endpoints, parâmetros e o mapeamento, para o Claude Code finalizar a documentação.

## 4. (Opcional) Configurar a integração na NeoAssist

Somente se o usuário autorizar e fornecer as credenciais:
- Na NeoAssist, criar/conectar a integração de e-commerce (Tray) que o A1 usará no lugar do placeholder `[[INT_ERP_B2C]]`.
- Configurar **timeout do nó**: 10 s + 1 retentativa + ramo de erro (silêncio zero — `docs/05-transbordo-humano-e-filas.md`, seção 2.2).
- Testar: consultar um pedido real por CPF → o A1 deve retornar status/rastreio; desligar a integração → deve cair na contingência (pede dado / transborda), sem deixar o cliente esperando (Regra Anti-Espera).

## 5. Regras (herdadas do projeto)

- **Validação antes de expor** dado de pedido (CPF × pedido).
- **Dados mascarados** na conversa.
- **Sem navegação ao vivo no site** pelo agente — a integração é via API; link do site é enviado ao cliente.
- **Nunca** colocar credenciais (consumer key/secret, tokens) em arquivos versionados.

## 6. Reportar

Ao final: endpoints capturados, mapeamento preenchido, se atualizou o repositório (link do PR) ou se precisa do Claude Code para finalizar, e — se configurou — o resultado dos testes do A1 com a Tray.
