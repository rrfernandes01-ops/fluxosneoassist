# Pacote Cowork — Calendário do JL Educa (agendamento de treinamentos)

> **Para o Claude Cowork** (ambiente do usuário). Integração `[[INT_AGENDA_JLEDUCA]]` (I-16) do agente **A7** (assunto 2 — JL Educa): consultar o **calendário disponível do time** para agendar treinamentos. Hoje o A7 faz coleta manual e transborda para a fila JL Educa; esta integração passa a mostrar disponibilidade.

## 1. Descobrir o sistema de calendário (com o usuário)

Perguntar/identificar **qual ferramenta** o time JL Educa usa para a agenda:
- Google Calendar (Google Workspace)?
- Microsoft 365 / Outlook Calendar?
- Agenda interna de outro sistema?

O caminho de integração depende disso.

## 2. Ler a documentação da API correspondente

- **Google Calendar API**: <https://developers.google.com/calendar> — `events.list` (freeBusy para disponibilidade), OAuth/服务 account, calendarId.
- **Microsoft Graph (Outlook)**: <https://learn.microsoft.com/graph/api/resources/calendar> — `getSchedule`/`calendarView`, OAuth.
- Capturar: autenticação, endpoint de **disponibilidade/eventos**, parâmetros (calendarId, janela de datas), formato de resposta.
- **Credenciais/OAuth**: o usuário fornece; **não versionar**.

## 3. Definir a regra de negócio do agendamento

Com o usuário/time JL Educa:
- Quais calendários/pessoas representam a disponibilidade do time.
- Janela de agendamento (ex.: próximos 30 dias, dias/horários úteis).
- **Decisão final do formato (presencial/online) e confirmação são do time JL Educa** — o A7 sugere e mostra disponibilidade, não confirma sozinho (manter regra do prompt A7).
- Dados que o A7 já coleta: CNPJ do cliente, rede ou loja única, nº de pessoas, sugestão de formato, observações.

## 4. Entregável

**Atualizar o repositório**: criar `docs/integracoes/jl-educa-calendario.md` com a ferramenta escolhida, endpoints, parâmetros e a regra de disponibilidade; atualizar o contrato I-16 em `docs/03-integracoes.md`. Abrir PR na branch `claude/fitoway-whatsapp-ai-x5uzrt`, **ou** reportar ao usuário.

## 5. Testar (após conectar)

- No A7, assunto JL Educa → o agente mostra janelas disponíveis reais e registra a solicitação; conferir que a confirmação final continua com o time JL Educa.
- Timeout/silêncio zero e Regra Anti-Espera na contingência (sem o calendário, volta à coleta manual + fila JL Educa).

## 6. Reportar
Ferramenta identificada, endpoints, regra de disponibilidade (link do PR ou lista) e resultado dos testes.
