# NeoAssist — API de Histórico de Status do Protocolo (ProtocolStatusHistory)

> Documentação oficial fornecida pela NeoAssist. Base para os placeholders **`[[INT_PROTOCOLOS]]`** e **`[[INT_HISTORICO]]`** (integração I-02). Retorna a linha do tempo cronológica das mudanças de status das interações e do status customizado.

## Para que serve (dois cenários)

1. **Consulta unitária**: histórico de um protocolo específico (por `protocol`).
2. **Sincronização em lote**: todos os protocolos criados/modificados em um período (integração com BI/Data Warehouse).

A resposta é **agrupada pelo ID do Protocolo**.

## Endpoint

```
GET /API/ExternalServices/Reports/ProtocolStatusHistory.json
```

## Query Parameters

| Parâmetro | Tipo | Obrigatório | Descrição |
|-----------|------|-------------|-----------|
| `AppKey` | string | Sim | Autenticação |
| `AppInstanceKey` | string | Sim | Autenticação |
| `protocol` | string | Condicional | ID do protocolo. Obrigatório se **não** houver filtro de data |
| `origin` | int | Condicional | ID do canal (ex.: 16=WhatsApp). Obrigatório se usar filtros de data de protocolo |
| `protocolCreatedAtStart` / `protocolCreatedAtEnd` | string | Não | Range de criação do protocolo (Y-m-d) |
| `protocolUpdatedAtStart` / `protocolUpdatedAtEnd` | string | Não | Range de atualização do protocolo (Y-m-d) |
| `interactionCreatedAtStart` / `...End` | string | Não | Range de criação da interação (Y-m-d) |
| `interactionUpdatedAtStart` / `...End` | string | Não | Range de atualização da interação (Y-m-d) |

**Regra de validação**: informar `protocol` **OU** pelo menos um par de datas válido (Start/End). Com filtros de data de protocolo, `origin` é obrigatório.

### Exemplos
```
# Unitária
GET .../ProtocolStatusHistory.json?AppKey=..&AppInstanceKey=..&protocol=29881738

# Sincronização por data (Email, criados em 19/12 com interações em 22/12)
GET .../ProtocolStatusHistory.json?AppKey=..&AppInstanceKey=..&protocolCreatedAtStart=2025-12-19&protocolCreatedAtEnd=2025-12-19&interactionCreatedAtStart=2025-12-22&interactionCreatedAtEnd=2025-12-22&origin=1
```

## Resposta

Objeto `data` = mapa onde a **chave é o número do Protocolo**; cada protocolo tem `status[]` (interações) e `customStatus[]`.

### `status[]` (interações)
`interactionId` (o "0" é sempre a primeira interação; pode repetir para origens diferentes), `createdAt`, `updatedAt`, `status`, `statusName`, `resolution`, `resolutionName`, `category`, `categoryBreadcrumb`, `responseTime`, `expertId`, `expertName`, `origin`, `originName`, `interactionType`, `interactionName`.

### `customStatus[]`
`oldStatusId`, `oldStatusName`, `newStatusId`, `newStatusName`, `timeInStatus` (seg), `expertId`, `expertName`, `changedAt`.

### Códigos HTTP
`200` sucesso; `400` parâmetro faltando/mal formatado; `401` auth inválida; `404` nada encontrado (`{"data": {}, "message": "..."}`); `500` erro interno.

## Tabelas de referência (De/Para)

**Status (`statusName`)**: 1 Novo · 2 Respondido · 3 Consumidor respondeu/Resposta recebida/Nova Mensagem · 4 Aguardando complemento/Aguardando Resposta · 5 Apagado · 6 Erro/Erro na Entrega · 7 Histórico Importado do Facebook · 8 Encerrado sem resposta.

**Resolução (`resolutionName`)**: 1 Novo · 2 Em atendimento · 3 Resolvido pelo Operador · 4 Finalizado por Tempo · 5 Resolvido por Regra · 6 Em Atendimento pelo Bot.

**Origem (`originName`)**: 1 Email · 2 Facebook · 3 Twitter · 4 Reclame Aqui · 5 Neolive · 6 Atendimento Inteligente · 7 Telefonia · 8 Registro Manual · 10 Mercado Livre · 12 Chat · 14 Chatbot · 15 Instagram · **16 WhatsApp** · 20 Workflow.

**Tipo de interação (`interactionName`)**: 0 Interação do Consumidor · 1 Interação do Operador.

## Mapeamento no projeto

- Placeholders: **`[[INT_PROTOCOLOS]]`** (protocolos abertos/fechados) e **`[[INT_HISTORICO]]`** (linha do tempo) — integração **I-02**.
- **Fluxo mestre (documento 02)**: ao entrar, consulta unitária/por consumidor para **ler os últimos protocolos** e preparar o contexto (identificação persistente, oferta de continuidade, solução proativa). Usar `statusName`/`resolutionName` para saber se há protocolo **em aberto** (ex.: status 4 "Aguardando") vs. resolvido.
- **Métricas (documento 06)**: a sincronização em lote por data alimenta os KPIs e a curadoria (distribuição de status, tempo em status, transbordos).
- **Origem 16 = WhatsApp** é o filtro padrão do canal.
- A consulta por telefone/CPF do consumidor (I-01) fornece o vínculo; esta API detalha o histórico do(s) protocolo(s) daquele consumidor.
- Respeitar silêncio zero / timeout (documento 05, seção 2.2).
