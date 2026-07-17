# Guia de implantação na NeoAssist

Este diretório contém os artefatos **prontos para colar** na plataforma NeoAssist, na ordem de implantação. A documentação de referência (o "porquê" de cada regra) está em `../docs/`.

## Pré-requisitos

- [ ] Acesso administrador à conta NeoAssist do Grupo Fitoway.
- [ ] Canal WhatsApp **(11) 2388-3360** homologado e conectado à NeoAssist.
- [ ] Núb.ia Resolve habilitada no plano.
- [ ] URL do termo de consentimento LGPD publicada (substituir `[LINK_CONSENTIMENTO]` em todos os textos).
- [ ] Nome oficial da persona definido (substituir `[Assistente de IA Fitoway]` em todos os textos).

## Ordem de implantação

| Passo | O quê | Onde na NeoAssist | Artefatos |
|-------|-------|-------------------|-----------|
| 1 | Base de conhecimento | Base de conhecimento → novos artigos | `passo-1-base-conhecimento/` (4 artigos) |
| 2 | Fluxo mestre | Automações → novo fluxo no canal WhatsApp | `passo-2-fluxo-mestre/construcao-no-a-no.md` |
| 3 | Agentes de IA | Núb.ia Resolve → criar 9 agentes | `passo-3-agentes/prompt-A1.md` … `prompt-A9.md` |
| 4 | Integrações | Integrações / APIs | `passo-4-integracoes/checklist-integracoes.md` |
| 5 | Filas humanas | Filas / times de atendimento | `passo-5-filas/checklist-filas.md` |

## Regras de montagem

1. **Passo 1 antes do 3**: os agentes só respondem com base nos artigos oficiais; sem base publicada, os agentes devem ficar em rascunho.
2. **Vincule os 4 artigos do passo 1 a TODOS os 9 agentes** (são a camada comum de comportamento).
3. **Passo 2 e 3 juntos**: o fluxo mestre termina em hand-off para os agentes; crie os agentes primeiro em rascunho, monte o fluxo, depois publique tudo junto.
4. **Passo 4 é incremental**: cada integração conectada substitui o comportamento de contingência correspondente (documentado em `../docs/03-integracoes.md`). O fluxo funciona desde o dia 1 apenas com I-01/I-02/I-03; sem elas, use as contingências.
5. **Passo 5 antes de publicar**: nenhum agente publicado sem sua fila de transbordo criada — transferência nunca é negada.

## Checklist de publicação (go-live)

- [ ] 4 artigos publicados e vinculados aos 9 agentes.
- [ ] 9 agentes criados com os prompts do passo 3, cada um com fila de transbordo vinculada.
- [ ] Fluxo mestre publicado no canal WhatsApp com os 9 hand-offs + ramo de exceção.
- [ ] Placeholders substituídos: `[Assistente de IA Fitoway]`, `[LINK_CONSENTIMENTO]`.
- [ ] Horário de atendimento configurado: seg–sex 9h–18h, calendário de feriados nacionais + cidade de São Paulo.
- [ ] Teste ponta a ponta com número não cadastrado (fluxo de cadastro + LGPD) e com número cadastrado (continuidade de protocolo).
- [ ] Teste de cada gatilho de transbordo (T1–T8 — `../docs/05-transbordo-humano-e-filas.md`).
- [ ] Piloto com % reduzida de tráfego antes de 100%.
