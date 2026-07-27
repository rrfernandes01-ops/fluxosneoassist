# Índice dos pacotes Cowork

Pacotes de instruções para o **Claude Cowork** executar no ambiente do usuário (com acesso ao navegador e às plataformas), já que o Claude Code não acessa a NeoAssist nem sites bloqueados pela rede.

## Configuração e testes dos agentes

| Pacote | O que faz |
|--------|-----------|
| [pacote-cowork-atualizacao-agentes.md](pacote-cowork-atualizacao-agentes.md) | Atualiza os 11 agentes e o artigo 02 na NeoAssist e roda os testes (Regra Anti-Espera) |
| [roteiro-de-testes-agentes.md](roteiro-de-testes-agentes.md) | Roteiro de testes dos fluxos por agente |

## Integrações (ordem de prioridade sugerida)

| # | Pacote | Placeholders | Prioridade |
|---|--------|--------------|------------|
| 1 | [pacote-cowork-neoassist-nativas.md](pacote-cowork-neoassist-nativas.md) | I-01, I-02, I-03, I-19, I-20 + IDs da conta | **P0** — destrava protocolo/transbordo |
| 2 | [pacote-cowork-tray-a1.md](pacote-cowork-tray-a1.md) | I-04 (+ I-05/I-13) | **P0** — pós-compra A1 |
| 3 | [pacote-cowork-pipefy.md](pacote-cowork-pipefy.md) | I-17 (Trade), I-21 (CX Casos) | P1 |
| 4 | [pacote-cowork-representantes-parceiros.md](pacote-cowork-representantes-parceiros.md) | I-09 (A7), I-11 (A8) | P1 |
| 5 | [pacote-cowork-afiliados.md](pacote-cowork-afiliados.md) | I-10 (A9) | P1 |
| 6 | [pacote-cowork-jl-educa-calendario.md](pacote-cowork-jl-educa-calendario.md) | I-16 (A7 — JL Educa) | P2 |
| 7 | [pacote-cowork-utilitarias.md](pacote-cowork-utilitarias.md) | I-05, I-12, I-13, I-15 | P2 |

> Ainda **sem pacote** (dependem de definição interna, não de integração): ERP B2B (I-07) e CRM B2B (I-08) — quando o usuário indicar o sistema (JL FIT/FTW) e houver API, gerar um pacote específico, análogo ao de representantes/parceiros.

## Regras comuns a todos os pacotes

- **Nunca versionar credenciais** (tokens, chaves, secrets) no repositório.
- Entregável: **atualizar o repositório** (abrir PR na branch `claude/fitoway-whatsapp-ai-x5uzrt`) **ou** reportar ao usuário para o Claude Code finalizar.
- Ao conectar cada integração: configurar **timeout do nó** (10 s + 1 retentativa + ramo de erro — doc 05, seção 2.2); a **Regra Anti-Espera** cobre a contingência.
- Cada integração conectada **substitui a contingência** do placeholder correspondente (doc 03).
