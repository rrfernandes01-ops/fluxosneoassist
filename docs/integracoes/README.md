# Documentação técnica das integrações

Documentos oficiais e planos das integrações do projeto. O catálogo e o mapa de placeholders `[[INT_*]]` estão em [`../03-integracoes.md`](../03-integracoes.md).

## NeoAssist — APIs de Protocolo (documentação oficial recebida)

| Documento | API | Placeholder / integração |
|-----------|-----|--------------------------|
| [neoassist-protocolo-criacao.md](neoassist-protocolo-criacao.md) | `RegisterOnly.json` (POST) — cria/registra protocolo | `[[INT_PROTOCOLO_CRIAR]]` (I-19) |
| [neoassist-protocolo-atualizacao.md](neoassist-protocolo-atualizacao.md) | `Update.json` (POST) — atualiza, classifica, encerra e executa ações de Workflow (transbordo) | `[[INT_PROTOCOLO_ATUALIZAR]]` (I-20) |
| [neoassist-protocolo-historico-status.md](neoassist-protocolo-historico-status.md) | `ProtocolStatusHistory.json` (GET) — histórico/linha do tempo, unitária e em lote | `[[INT_PROTOCOLOS]]` / `[[INT_HISTORICO]]` (I-02) |

**Autenticação NeoAssist** (comum): query string `AppKey`, `AppInstanceKey` e (nas de escrita) `ExpertID`. Base `https://{cliente}.neoassist.com`. `Origin: 16` = WhatsApp.

## Tray (e-commerce FTW)

| Documento | Status |
|-----------|--------|
| [tray-ecommerce-a1.md](tray-ecommerce-a1.md) | **Plano** — doc oficial não acessível deste ambiente (rede bloqueia o domínio). Mapeamento funcional pronto; endpoints/campos a validar. |

## Como estas integrações servem o projeto

- **Abrir protocolo** (todos os agentes): `[[INT_PROTOCOLO_CRIAR]]` — inclui `Tags` (perfil/classificação, ex.: creators do A9 e LGPD) e `Origin: 16`.
- **Transbordo com contexto** (documento 05): `[[INT_PROTOCOLO_ATUALIZAR]]` com `workflowAction: openWorkflow` + `WFObservacao` (resumo) + `DepartamentoID` da fila.
- **Ler histórico/protocolos** (documento 02 — identificação persistente e continuidade): `ProtocolStatusHistory`.
- **A1 pós-compra** (pedidos/rastreio/pagamento): Tray (a concluir).
- Todas as chamadas seguem **silêncio zero / timeout** (documento 05, seção 2.2).
