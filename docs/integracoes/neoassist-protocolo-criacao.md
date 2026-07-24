# NeoAssist — API de Criação de Protocolo (RegisterOnly)

> Documentação oficial fornecida pela NeoAssist. Base para o placeholder **`[[INT_PROTOCOLO_CRIAR]]`** (integração I-19). Usada por **todos os agentes** para abrir/registrar o protocolo do atendimento antes do transbordo ou ao final.

## Endpoint

```
POST /API/ExternalServices/ProtocoloService/RegisterOnly.json
```

- Base: `https://{cliente}.neoassist.com` (substituir `{cliente}` pelo subdomínio da conta FTW).
- **Autenticação** (query string): `?AppKey={AppKey}&AppInstanceKey={AppInstanceKey}&ExpertID={ExpertID}`
- Header: `Content-Type: application/json`

## Payload padrão

```json
{
  "jsonData": {
    "CategoryID": "string",
    "Data": {
      "Name": "string",
      "EMail": "string",
      "FieldU": "string",
      "Field[Letra]": "string",
      "ProtocoloField[Letra]": "string"
    },
    "ExpertID": "string",
    "Origin": 16,
    "Observacao": "string",
    "ResolverProtocolo": "string",
    "AguardandoComplemento": "string",
    "ClassificacaoCRM": "string",
    "Tags": { "chave": "valor" },
    "CustomStatusId": 18,
    "CustomStatusName": "Pendente"
  }
}
```

## Campos principais

| Campo | Tipo | Obrigatório | Observação |
|-------|------|-------------|------------|
| `CategoryID` | String | Sim | ID da categoria (encontrado na plataforma) |
| `ExpertID` | String | Sim | ID do operador responsável (Painel de Controle → Operadores → ID) |
| `Observacao` | String | Sim | Interação/observação lançada no protocolo |
| `Origin` | Int | Sim | ID do canal: **1=Email, 8=Registro Manual, 16=WhatsApp** |
| `ResolverProtocolo` | String | Sim | `"on"` = marca como resolvido; `""` = não |
| `AguardandoComplemento` | String | Sim | `"on"` = protocolo aparece no MA (aberto) após criação; `""` = entra como resolvido |
| `Data.Name` | String | Sim | Nome do consumidor |
| `Data.EMail` | String | Sim | E-mail do consumidor |
| `Data.FieldU` | String | Sim | **CPF/CNPJ do consumidor** |
| `Data.Field[A..T]` | String | Cond. | Campos customizados do consumidor (se configurados como obrigatórios) |
| `Data.ProtocoloField[A..J]` | String | Cond. | Campos customizados do protocolo |
| `ClassificacaoCRM` | String | Não | ID do CRM |
| `Tags` | Objeto | Não | `{ "IdArvoreTag": "IdTagClassificacao" }` — chave = ID da árvore (tag mãe), valor = ID da tag (último nível) |
| `CustomStatusId` / `CustomStatusName` | Int/String | Não | Status customizado (informar **apenas um**; se ambos, vale o ID) |

### Atributo `Tags` (importante para o projeto)
- A **chave** deve ser o ID da **árvore de Tags** (sempre a tag mãe) e o **valor** o ID da **tag de classificação** (sempre o último nível).
- IDs obtidos em **Painel de Controle → Tags** (download). No arquivo, a árvore tem o campo `Origens` preenchido; a classificação precisa respeitar essas origens.
- Valores incorretos → resposta `success: false` com mensagem de erro.
- **Uso no projeto**: é por aqui que o agente aplica as tags de perfil e classificação — ex.: as tags de creators do A9 (`lead_influenciador`, `lead_UGC`, `lead_afiliado`) e a tag "Privacidade e proteção de dados (LGPD)" dos pedidos de titular (doc 06).

### Canais especiais
- **E-mail**: adicionar `"Assunto"` e usar `Origin: 1`.
- **Encaminhar para Workflow na criação**: adicionar `rbEncaminhar: "W"`, `WFObservacao`, `Prazo`, `DepartamentoID`.
- **SMS+ disparo**: `AccountId`, `Recipient` (DDI+DDD+número), `Origin: 17`.
- **WhatsApp + disparo HSM**: `Origin: 16`, `source` (nº do consumidor), `origin` minúsculo (nº do cliente), `HSMParams`, `HSMButtonsParams`, `Hsm.Header.Media.Image` (filename+base64). Requer regra de disparo HSM com status NOVO configurada.
- **Anexos**: `ChamadoAnexo` (array de `{filename, base64file}`) ou via form-data. Escolher **um** método: base64 **ou** form-data.

## Resposta (sucesso)

```json
{
  "success": true,
  "msg": "Mensagem Enviada. Protocolo #:29842002. Anexos 0",
  "Protocolo": {
    "success": true,
    "Status": 1,
    "Info": "Protocolo Criado",
    "Protocolo": 29842002,
    "DadosComplementaresAlterados": true
  },
  "DadosConsumidor": { "CustomerID": 1345866, "Name": "...", "FieldU": "..." },
  "RegistrouTempo": true,
  "TempoAberto": 0
}
```

- Atributos mais importantes do retorno: **`Protocolo.Protocolo`** (nº do protocolo criado) e **`DadosConsumidor.CustomerID`** (ID do consumidor na NeoAssist) — guardar ambos no contexto do atendimento.

## Erros comuns

| msg | Causa |
|-----|-------|
| "Ocorreu um problema ao cadastrar o consumidor do chamado" + `errors.FieldU` | CPF/CNPJ inválido |
| "O atributo jsonData é obrigatório..." | `jsonData` ausente |
| "A origem informada não é suportada pela API!" | `Origin` inválido |
| "O atributo source/origin para Whatsapp é obrigatório..." | faltou `source`/`origin` no HSM |

## Mapeamento no projeto

- Placeholder: **`[[INT_PROTOCOLO_CRIAR]]`** (I-19).
- Usado no fluxo mestre e em todos os agentes para **abrir/registrar protocolo** (documento 05). `Origin: 16` (WhatsApp).
- `AguardandoComplemento: "on"` quando o caso segue para humano (protocolo aberto no MA); `ResolverProtocolo: "on"` quando a IA resolve e encerra.
- `Tags` carrega a classificação do assunto e as tags de perfil.
- Respeitar as regras de **silêncio zero / timeout** (documento 05, seção 2.2) nas chamadas.
