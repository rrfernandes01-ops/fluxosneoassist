# NeoAssist — API de Atualização de Protocolo (Update)

> Documentação oficial fornecida pela NeoAssist. Base para o placeholder **`[[INT_PROTOCOLO_ATUALIZAR]]`** (integração I-20). Usada para adicionar interações a um protocolo existente, classificar, encerrar e — principalmente — **executar ações de Workflow** (o transbordo com contexto do documento 05).

## Endpoint

```
POST /API/ExternalServices/ProtocoloService/Update.json
```

- Autenticação (query string): `?AppKey={AppKey}&AppInstanceKey={AppInstanceKey}&ExpertID={ExpertID}`
- Header: `Content-Type: application/json`

## Payload padrão

```json
{
  "jsonData": {
    "Protocolo": "string",
    "CategoryID": "string",
    "Data": { "Name": "...", "EMail": "...", "FieldU": "...", "Field[Letra]": "...", "ProtocoloField[Letra]": "..." },
    "ExpertID": "string",
    "Origin": 8,
    "Observacao": "string",
    "ResponderChamado": "string",
    "IncluirHistorico": "string",
    "AguardandoComplemento": "string",
    "ResolverProtocolo": "string",
    "Tags": { "chave": "valor" },
    "CustomStatusId": 18,
    "CustomStatusName": "Pendente"
  }
}
```

## Campos principais (além dos de criação)

| Campo | Tipo | Obrigatório | Observação |
|-------|------|-------------|------------|
| `Protocolo` | String | Sim | **Nº do protocolo a atualizar** |
| `ResponderChamado` | String | Sim | `"on"` = responde o chamado; `""` = não |
| `IncluirHistorico` | String | Sim | `"on"` = a interação consta no histórico |
| `ResolverProtocolo` | String | Sim | `"on"` = marca como resolvido |
| `AguardandoComplemento` | String | Sim | `"on"` = mantém aberto no MA |
| `AssociarProtocolos` | Array | Não | Associa outros protocolos: `[{ "Origem": int, "Protocolo": int }]` |
| `ChamadoAnexo` | Array | Não | Anexos `{filename, base64file}` (ou form-data) |

## Ações de Workflow (`workflowAction`) — transbordo com contexto

A API permite acionar o Workflow via `workflowAction` (obrigatório sempre que o fluxo de workflow for acionado, junto de `DepartamentoID`).

| workflowAction | O que faz | Equivalente no frontend |
|----------------|-----------|-------------------------|
| `openWorkflow` | **Abre** o workflow (protocolo **não** está em WF). Usar `Observacao` **e** `WFObservacao` | Transferir → encaminhar para Workflow |
| `forwardToSameDepartment` | Encaminha para o **mesmo** departamento (protocolo **já** em WF). Usa depto atual, `WFObservacao` | Transferir → mesmo departamento |
| `forwardToDifferentDepartment` | Encaminha para **outro** departamento (já em WF); usa `DepartamentoID` + `WFObservacao` | Transferir → outro departamento |
| `standardWorkflow` | Segue o **fluxo padrão** configurado; `WFObservacao` | Enviar (em WF) |
| `insertObservationWithoutClosing` | Insere observação **sem encerrar**; `WFObservacao` | Checkbox "inserir sem encerrar" |
| `closeWithoutResponse` | **Encerra sem retornar** (protocolo origem não volta ao MA); `WFObservacao` | Checkbox "encerrar sem retornar" |

### Campos específicos de Workflow

| Campo | Tipo | Obrigatório | Observação |
|-------|------|-------------|------------|
| `workflowAction` | String | Sim (em WF) | Ação a executar |
| `DepartamentoID` | String | Sim (em WF) | ID do departamento |
| `WFObservacao` | String | Cond. | Obrigatório em `openWorkflow`, `forwardToSameDepartment`, `forwardToDifferentDepartment`; nas demais usar; para `openWorkflow` também preencher `Observacao` (interação do canal) |
| `WFAnexo` | Array | Não | Anexos do workflow `{filename, base64file}` (aparece no box de interação do WF). Para anexo na origem ao abrir WF, usar `ChamadoAnexo` |

### Retornos de Workflow

```json
{ "success": true, "protocolo": 29856692, "msg": "Workflow criado com sucesso." }
```
Mensagens por ação: "Workflow encaminhado com sucesso.", "Workflow encerrado sem resposta.", "Observação inserida sem finalizar workflow.", "Workflow atualizado com sucesso." / erro: `{ "success": false, "msg": "Ação de workflow não reconhecida" }`.

## Mapeamento no projeto

- Placeholder: **`[[INT_PROTOCOLO_ATUALIZAR]]`** (I-20).
- **Transbordo humano com contexto (documento 05)**: o hand-off para a fila humana é operacionalizado por `openWorkflow` (leva `WFObservacao` com o resumo do atendimento = "o usuário nunca repete a história") + `DepartamentoID` da fila da categoria.
- **Encerramento pela IA**: `ResolverProtocolo: "on"` + `Observacao` (resumo).
- **Classificação/tags**: `Tags` e `CustomStatusId` para status customizado (ex.: "Pendente", "Em Análise").
- Mapear `DepartamentoID` de cada **fila** do documento 05 (Consumidor, B2B Farma SP, B2B Nacional, JL Educa, Trade Marketing, Terceirização, Parcerias, Marketing/Afiliados, Privacidade/DPO, Jurídico/Ouvidoria, Cobrança/Crédito) — **a preencher na implantação** com os IDs reais.
- Respeitar silêncio zero / timeout (documento 05, seção 2.2).
