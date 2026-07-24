# Integração Google Sheets — Cadastro de Creators (A9)

> Destino dos cadastros do agente A9 (Influenciadores, UGC e Afiliados). Espelha o passo `google_sheets → inserir_linha` do fluxo original do Direct, mantendo os campos e as tags. **Decisão do projeto**: três planilhas separadas, uma por perfil.
>
> **Status**: as três planilhas **foram criadas** no Google Drive de rr.fernandes01@gmail.com (raiz do Meu Drive), com a linha de cabeçalho. Falta apenas conectar o passo `inserir_linha` na NeoAssist, substituindo o placeholder `[[INT_SHEETS_CREATORS]]` pelo conector real (integração I-18).

## Planilhas (Google Sheets)

| Perfil | ID | Link |
|--------|----|----|
| Influenciadores | `1NzWtvI5YyPphneuFX8rDhEVg7l8DLJO1QhJGIE0vEug` | <https://docs.google.com/spreadsheets/d/1NzWtvI5YyPphneuFX8rDhEVg7l8DLJO1QhJGIE0vEug/edit> |
| UGC | `1DygZNq3LrTOFopREfeULMKzw3RS_XmxMz5bpEw8wKMs` | <https://docs.google.com/spreadsheets/d/1DygZNq3LrTOFopREfeULMKzw3RS_XmxMz5bpEw8wKMs/edit> |
| Afiliados | `1Gh9eodQ4ygOTL88WDjRKsDsl7x-8VJKarhyGJ4TxX2Y` | <https://docs.google.com/spreadsheets/d/1Gh9eodQ4ygOTL88WDjRKsDsl7x-8VJKarhyGJ4TxX2Y/edit> |

> Ao conectar na NeoAssist, apontar a gravação de cada perfil para o ID correspondente. Conferir a ordem das colunas do cabeçalho (abaixo) no mapeamento.

## Placeholder e integração

- Placeholder: `[[INT_SHEETS_CREATORS]]`
- Integração: **I-18 — Google Sheets Creators** (documento 03).
- Operação: `inserir_linha` na planilha correspondente ao perfil escolhido.
- Contingência (sem a integração): registrar o cadastro no protocolo/campos do contato e transbordar para a fila Marketing / Afiliados (fluxo T9).

## Planilha 1 — Influenciadores (tag `lead_influenciador`)

| Coluna | Origem |
|--------|--------|
| `data_hora_cadastro` | datetime da ação |
| `arroba_usuario` | @ do Instagram (perguntado no WhatsApp) |
| `nome` | coletado |
| `numero_seguidores` | De 10 a 50 mil / De 50 a 100 mil / Mais de 100 mil |
| `nicho` | campo aberto |
| `email` | coletado |
| `whatsapp` | número do contato (confirmado) |
| `tag` | `lead_influenciador` |
| `canal_origem` | WhatsApp (11) 2388-3360 |
| `protocolo` | protocolo NeoAssist |
| `optin_marketing` | sim/não (se ofertado) |
| `observacoes` | livre |

## Planilha 2 — UGC (tag `lead_UGC`)

| Coluna | Origem |
|--------|--------|
| `data_hora_cadastro` | datetime da ação |
| `arroba_usuario` | @ do Instagram |
| `nome` | coletado |
| `ja_criou_conteudo_para_marcas` | Sim / Ainda não |
| `grava_video_para_camera` | Sim / Depende do briefing / Vídeos com IA |
| `email` | coletado |
| `whatsapp` | número do contato (confirmado) |
| `tag` | `lead_UGC` |
| `canal_origem` | WhatsApp (11) 2388-3360 |
| `protocolo` | protocolo NeoAssist |
| `optin_marketing` | sim/não |
| `observacoes` | livre |

## Planilha 3 — Afiliados (tag `lead_afiliado`)

> Programa tratado como **ativo/completo** no WhatsApp (diferente do Direct, que estava "em reestruturação").

| Coluna | Origem |
|--------|--------|
| `data_hora_cadastro` | datetime da ação |
| `arroba_usuario` | @ do Instagram |
| `nome` | coletado |
| `redes_handles` | outras redes/@s |
| `audiencia_declarada` | coletado |
| `documento` | CPF ou CNPJ (MEI/empresa) |
| `email` | coletado |
| `whatsapp` | número do contato (confirmado) |
| `tag` | `lead_afiliado` |
| `canal_origem` | WhatsApp (11) 2388-3360 |
| `protocolo` | protocolo NeoAssist |
| `optin_marketing` | sim/não |
| `observacoes` | livre |

## Notas de implementação

- O agente aplica a **tag do perfil** antes da gravação (mantido do fluxo original) — a tag também vai na planilha para rastreio cruzado.
- A gravação segue as regras de integração do documento 03 (validação, timeout/silêncio zero, log no protocolo).
- LGPD: dados coletados para a finalidade de avaliação/parceria; opt-in de marketing é separado (documento 06).
