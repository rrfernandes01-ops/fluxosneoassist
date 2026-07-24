# Tray (e-commerce FTW) — plano de integração para o A1

> **Status: rascunho/plano — a validar na documentação oficial.** A doc da Tray (<https://developers.tray.com.br/#novotray-api-plugin>) **não pôde ser acessada** deste ambiente (a política de rede bloqueia o domínio, HTTP 403). Este documento define o **mapeamento funcional** do que o A1 precisa; os endpoints, nomes de campos e a autenticação exatos devem ser confirmados na doc oficial (ou enviados em PDF/export, como foi feito com a NeoAssist).
>
> Base para o placeholder **`[[INT_ERP_B2C]]`** (I-04) e complementos de rastreio/pagamento do A1.

## Objetivo

Automatizar no A1 (Cliente Site FTW) o máximo possível do pós-compra, consultando **consumidor**, **pedidos** e **histórico/status** direto na plataforma do site (Tray), reduzindo transbordos.

## Necessidades funcionais do A1 (o que a integração precisa entregar)

| Necessidade | Uso no A1 | Placeholder |
|-------------|-----------|-------------|
| Localizar o **consumidor** por CPF/e-mail/telefone | Validação de identidade (CPF × pedido) e reconhecimento | `[[INT_CONSUMIDOR]]` (cruzado) |
| Listar **pedidos** do consumidor | "meus pedidos", status, itens, valores | `[[INT_ERP_B2C]]` |
| **Detalhe do pedido** (status, itens, pagamento, NF) | Status de pedido, 2ª via de NF | `[[INT_ERP_B2C]]` |
| **Rastreio/entrega** (transportadora, código, previsão) | "cadê meu pedido" | `[[INT_RASTREIO]]` |
| **Status de pagamento** (boleto/Pix; sem dados de cartão) | Reenvio de boleto/Pix | `[[INT_PAGAMENTOS]]` |
| **Catálogo** (produto, preço, estoque) | Dúvidas de produto e recompra (A1/A2) | `[[INT_CATALOGO]]` |

## Estrutura típica da API Tray (a confirmar na doc oficial)

> Baseado no padrão conhecido da Tray Commerce. **Não implementar sem confirmar** os caminhos e campos reais na documentação `novotray-api-plugin`.

- **Autenticação**: fluxo com `consumer_key` + `consumer_secret` (+ `code`) → retorna `access_token` com validade e `refresh_token`. Todas as chamadas usam o `access_token`.
- **Pedidos**: endpoint de listagem com filtros (por cliente, CPF, data, status) e endpoint de detalhe por ID do pedido.
- **Clientes/consumidores**: endpoint de busca por CPF/e-mail e detalhe por ID.
- **Produtos**: detalhe por ID (ficha, preço, estoque).
- Respostas em JSON, paginadas.

### Campos-alvo do mapeamento (a preencher após a doc)
`pedido.id`, `pedido.status`, `pedido.itens[]`, `pedido.valor`, `pedido.pagamento.status`, `pedido.nota_fiscal`, `pedido.transportadora`, `pedido.codigo_rastreio`, `pedido.previsao_entrega`, `cliente.cpf`, `cliente.email`, `cliente.telefone`.

## Regras (herdadas do projeto)

1. **Validação antes de expor** dado de pedido (CPF × pedido) — documento 02.
2. **Dados mascarados** na conversa (documento 01).
3. **Silêncio zero / timeout** em toda chamada (documento 05, seção 2.2): 10 s + 1 retentativa, mensagem de espera, e transbordo T9 em falha/demora.
4. **Sem navegação ao vivo no site**: a integração é via API; o agente não abre o site ftw.com.br durante a conversa (documento 05).
5. **Contingência** (sem a integração conectada): coletar nº do pedido + CPF, registrar protocolo (`[[INT_PROTOCOLO_CRIAR]]`) e transbordar para a fila Consumidor.

## Pendências para concluir esta integração

- [ ] Obter a documentação oficial da Tray (a rede bloqueia o acesso direto — enviar export/PDF, ou liberar o domínio).
- [ ] Confirmar endpoints reais de auth, pedidos, clientes, produtos.
- [ ] Mapear os campos reais → variáveis do A1.
- [ ] Definir credenciais (`consumer_key`/`secret`) e o modelo de renovação de token.
- [ ] Preencher o contrato I-04 no documento 03 e substituir o placeholder na NeoAssist.
