# Pacote Cowork — atualizar os 11 agentes na NeoAssist e testar os fluxos

> **Para o Claude Cowork** (roda no ambiente do usuário, com acesso ao navegador/Chrome e à plataforma NeoAssist já aberta e logada). Este pacote é autocontido: siga na ordem. O repositório de referência é `rrfernandes01-ops/fluxosneoassist` (branch `main`).
>
> **Objetivo desta rodada**: corrigir o comportamento em que o agente "diz que vai consultar e não retorna" (integrações ainda não conectadas). A correção é a **Regra Anti-Espera**, já presente nos prompts atualizados. Depois, testar todos os fluxos.

## 0. Contexto e credenciais (o usuário fornece)

- Plataforma: **NeoAssist** (Núb.ia Resolve), conta do Grupo Fitoway, canal WhatsApp **(11) 2388-3360**.
- Acesso: usar a aba do Chrome já logada. Se pedir login, solicitar ao usuário.
- Persona: substituir `[Assistente de IA Fitoway]` pelo **nome oficial da persona** (perguntar ao usuário se ainda não definido).
- Link LGPD: substituir `[LINK_CONSENTIMENTO]` pela URL do Aviso de Privacidade (se já publicada; senão, deixar como está e sinalizar).

## 1. Atualizar a base de conhecimento (artigo 02)

1. Abrir **Base de conhecimento** na NeoAssist.
2. Localizar o artigo **"Regras gerais de atendimento"** (conteúdo em `implantacao/passo-1-base-conhecimento/artigo-02-regras-de-atendimento.md`).
3. **Substituir o conteúdo** pelo texto atualizado do arquivo (contém a nova **Regra Anti-Espera** e o **Silêncio zero**).
4. Salvar. Confirmar que o artigo está **vinculado aos 11 agentes** (A1–A11).

## 2. Atualizar os 11 agentes (Núb.ia Resolve)

Para **cada** agente abaixo, abrir o agente na Núb.ia Resolve e **substituir as instruções** pelo conteúdo do arquivo correspondente (a partir da linha após o bloco `---`, que é o prompt em si; o cabeçalho `>` é orientação de configuração, não vai no campo de instruções):

| Agente na plataforma | Nome interno | Arquivo do prompt |
|----------------------|--------------|-------------------|
| A1 — Cliente Site FTW | `A1-cliente-site-ftw` | `implantacao/passo-3-agentes/prompt-A1.md` |
| A2 — Prospect Consumidor | `A2-prospect-consumidor` | `prompt-A2.md` |
| A3 — Cliente Marketplace | `A3-cliente-marketplace` | `prompt-A3.md` |
| A4 — Cliente PDV | `A4-cliente-pdv` | `prompt-A4.md` |
| A5 — B2B Farma SP | `A5-b2b-farma-sp` | `prompt-A5.md` |
| A6 — B2B Nacional | `A6-b2b-nacional` | `prompt-A6.md` |
| A7 — Representantes | `A7-representantes` | `prompt-A7.md` |
| A8 — Profissionais de Saúde | `A8-profissionais-saude` | `prompt-A8.md` |
| A9 — Creators | `A9-creators` | `prompt-A9.md` |
| A10 — Terceirização | `A10-terceirizacao` | `prompt-A10.md` |
| A11 — CX Caso Financeiro | `A11-cx-caso-financeiro` | `prompt-A11.md` |

Para cada agente, ao colar:
- Trocar `[Assistente de IA Fitoway]` pelo nome oficial da persona.
- Conferir que o agente está vinculado aos artigos 01–04 e 06 (A3 também ao 05).
- Conferir que a **fila de transbordo** do agente existe (ver passo 5 da implantação) e está vinculada. Se a fila não existir (ex.: **CX Casos**, **JL Educa**, **Trade Marketing**, **Terceirização**), criar antes de publicar.
- Salvar.

## 3. Teste-chave (rodar em TODOS os 11 agentes) — SEM integração conectada

Como as integrações estão como placeholder, este é o teste central.

Para cada agente, iniciar uma conversa de teste e provocar uma necessidade de dado de sistema, por exemplo:
- A1: "qual o status do meu pedido?"
- A5/A6: "meu boleto está em aberto?" / "status do meu pedido"
- A7: "consultar minha carteira"
- A11: "quero um estorno do pedido X"
- (demais: qualquer pergunta que exija consulta)

**Resultado esperado (PASSA)**:
- O agente **não** responde "vou consultar / já te retorno" e some.
- Ele **pede o dado** à pessoa (ex.: nº do pedido + CPF) **ou** informa que vai transferir e **faz o transbordo na hora**, com um resumo.
- A conversa **nunca fica sem resposta**.

**FALHA**: o agente prometeu uma consulta e não voltou, ou ficou em silêncio. → Reportar ao usuário qual agente e a conversa.

## 4. Testes de transbordo (por agente)

- Pedir "quero falar com um atendente" → deve transferir na primeira vez para a fila correta.
- Fornecer dado inexistente / dizer "não tenho o número" → 1 tentativa de ajuda e transbordo com o que foi coletado.
- Fora do horário (seg–sex 9h–18h) → registra protocolo + mensagem de retorno.

## 5. Checklist específico por agente

Seguir a lista de `implantacao/roteiro-de-testes-agentes.md` (seção "Checklist por agente").

## 6. Reportar

Ao final, reportar ao usuário:
- Quais agentes **passaram** no teste-chave e nos de transbordo.
- Qualquer agente que **falhou**, com o print/trecho da conversa.
- Filas que faltavam e foram criadas.
- Placeholders ainda pendentes (`[LINK_CONSENTIMENTO]`, nome da persona) se não resolvidos.

## Observação sobre timeout técnico

A Regra Anti-Espera (prompt) já resolve o comportamento **sem** configuração de plataforma. Quando as integrações forem conectadas, configurar **também** o timeout do nó de integração (10 s + 1 retentativa + ramo de erro), conforme `docs/05-transbordo-humano-e-filas.md`, seção 2.2.
