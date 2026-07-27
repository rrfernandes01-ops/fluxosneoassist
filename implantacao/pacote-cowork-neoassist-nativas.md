# Pacote Cowork — Integrações nativas NeoAssist (protocolo, consumidor, workflow) e IDs

> **Para o Claude Cowork** (ambiente do usuário, com NeoAssist logada). Este é o pacote **mais prioritário**: as APIs de protocolo já estão documentadas (`docs/integracoes/`), mas dependem de **IDs e chaves da conta** que só existem dentro da plataforma. Sem eles, nada de protocolo/transbordo funciona de verdade.
>
> Placeholders: `[[INT_CONSUMIDOR]]` (I-01), `[[INT_HISTORICO]]`/`[[INT_PROTOCOLOS]]` (I-02), `[[INT_PROTOCOLO_CRIAR]]` (I-19), `[[INT_PROTOCOLO_ATUALIZAR]]` (I-20), `[[INT_CONSENTIMENTO]]` (I-03).

## 1. Coletar as chaves de autenticação da API

No Painel de Controle da NeoAssist, obter (com o usuário/admin):
- **AppKey** e **AppInstanceKey** (autenticação das APIs de protocolo).
- Subdomínio da conta: `https://{cliente}.neoassist.com`.
- **NÃO** versionar essas chaves no repositório — guardar no cofre de credenciais que o usuário indicar.

## 2. Coletar os IDs de configuração (destravam os prompts)

Levantar na plataforma e registrar (no repositório, pois **não são segredo**):

| Item | Onde encontrar | Usado em |
|------|----------------|----------|
| **ExpertID** do operador/robô responsável | Painel de Controle → Operadores → ID | Criação/atualização de protocolo |
| **CategoryID** por assunto | Categorias/árvore de atendimento | `RegisterOnly`/`Update` |
| **DepartamentoID** de cada fila de Workflow | Fluxo de trabalho → Departamentos | Transbordo `openWorkflow` |
| **IDs da árvore de Tags e das Tags de classificação** | Painel de Controle → Tags → download | Atributo `Tags` (perfis creators, LGPD, assuntos) |
| **CustomStatusId / Name** | Painel de Controle → Status Customizados | Status dos protocolos |

Mapear os **DepartamentoID** para cada fila do documento `docs/05-transbordo-humano-e-filas.md`: Consumidor, Financeiro Consumidor, B2B Farma SP, B2B Nacional, JL Educa, Trade Marketing, Terceirização, Parcerias Profissionais, Marketing/Afiliados, Privacidade/DPO, Jurídico/Ouvidoria, Cobrança/Crédito, **CX Casos**.

Mapear as **Tags** usadas pelo projeto: `lead_influenciador`, `lead_UGC`, `lead_afiliado` (A9) e "Privacidade e proteção de dados (LGPD)" (doc 06) → IDs reais de árvore/tag.

## 3. Integração de consumidor por telefone (I-01)

Verificar como a conta expõe a **busca de consumidor por telefone/CPF/CNPJ** (integração de consumidor da NeoAssist usada no fluxo mestre, doc 02). Documentar entrada/saída e o campo de retorno do CustomerID. Se for a mesma base dos protocolos, indicar.

## 4. Habilitar o módulo Workflow

- Confirmar com a NeoAssist o **status de habilitação do módulo Workflow** (previsto). Sem ele, `openWorkflow` não funciona e o processo CX Casos roda em contingência (protocolo + transbordo manual).
- Quando habilitado, validar as ações (`openWorkflow`, `forwardTo...`, etc.) conforme `docs/integracoes/neoassist-protocolo-atualizacao.md`.

## 5. Entregável

**Atualizar o repositório** (sem credenciais): criar/editar um doc `docs/integracoes/neoassist-ids-conta.md` com a tabela de IDs (ExpertID, CategoryID, DepartamentoID por fila, IDs de Tags, CustomStatus) e o subdomínio. Abrir PR na branch `claude/fitoway-whatsapp-ai-x5uzrt`, **ou** reportar os IDs ao usuário para o Claude Code registrar.

## 6. Testar

- Criar um protocolo de teste via API (`RegisterOnly`) com `Origin: 16` e uma Tag → conferir criação e classificação.
- Atualizar com `Update` + `openWorkflow` (se o WF estiver habilitado) → conferir abertura de Workflow no departamento certo.
- Consultar `ProtocolStatusHistory` do protocolo → conferir a linha do tempo.

## 7. Reportar

IDs coletados (link do PR ou lista), status do módulo Workflow, resultado dos testes, e quais placeholders já podem ser substituídos pelos conectores reais.
