# Índice dos pacotes Cowork

Pacotes de instruções para o **Claude Cowork** executar no ambiente do usuário (com acesso ao navegador e às plataformas), já que o Claude Code não acessa a NeoAssist nem sites bloqueados pela rede.

## Configuração e testes dos agentes

| Pacote | O que faz |
|--------|-----------|
| [pacote-cowork-auditoria-nos-fluxo.md](pacote-cowork-auditoria-nos-fluxo.md) | **Fazer antes dos demais** — audita todos os nós nativos do construtor de fluxo do NeoAssist (entrada dinâmica com validação CPF/CNPJ, salvar variável, vincular consumidor, atribuir categoria etc.), cruza com o fluxo mestre documentado e reconstrói usando os recursos nativos |
| [pacote-cowork-atualizacao-agentes.md](pacote-cowork-atualizacao-agentes.md) | Atualiza os 11 agentes e o artigo 02 na NeoAssist e roda os testes (Regra Anti-Espera) |
| [roteiro-de-testes-agentes.md](roteiro-de-testes-agentes.md) | Roteiro de testes dos fluxos por agente |

## Integrações (ordem de prioridade sugerida)

| # | Pacote | Placeholders | Prioridade |
|---|--------|--------------|------------|
| 1 | [pacote-cowork-neoassist-nativas.md](pacote-cowork-neoassist-nativas.md) | I-01, I-02, I-03, I-19, I-20 + IDs da conta | **P0** — destrava protocolo/transbordo |
| 2 | [pacote-cowork-tray-a1.md](pacote-cowork-tray-a1.md) | I-04 (+ I-05/I-13) | **P0** — pós-compra A1 |
| 3 | [pacote-cowork-pipefy.md](pacote-cowork-pipefy.md) | I-17 (Trade), I-21 (CX Casos) | P1 |
| 3b | [pacote-cowork-build-pipefy-cx.md](pacote-cowork-build-pipefy-cx.md) — **construir o pipe financeiro do CX do zero** (blueprint em `../docs/processos/pipefy-cx-financeiro-blueprint.md`) | I-21 | P1 |
| 4 | [pacote-cowork-erp-crm-b2b.md](pacote-cowork-erp-crm-b2b.md) | I-07 (ERP), I-08 (CRM) — JL FIT/FTW | **P0/P1** — sustenta todo o B2B |
| 5 | [pacote-cowork-representantes-parceiros.md](pacote-cowork-representantes-parceiros.md) | I-09 (A7), I-11 (A8) | P1 |
| 6 | [pacote-cowork-afiliados.md](pacote-cowork-afiliados.md) | I-10 (A9) | P1 |
| 7 | [pacote-cowork-jl-educa-calendario.md](pacote-cowork-jl-educa-calendario.md) | I-16 (A7 — JL Educa) | P2 |
| 8 | [pacote-cowork-utilitarias.md](pacote-cowork-utilitarias.md) | I-05, I-12, I-13, I-15 | P2 |

> **Primeiro passo do pacote ERP/CRM**: descobrir com a TI da JL FIT/FTW qual é o ERP e o CRM e se há API — o pacote conduz essa descoberta antes do mapeamento.

## Regras comuns a todos os pacotes

- **Nunca versionar credenciais** (tokens, chaves, secrets) no repositório.
- Entregável: **atualizar o repositório** (abrir PR na branch `claude/fitoway-whatsapp-ai-x5uzrt`) **ou** reportar ao usuário para o Claude Code finalizar.
- Ao conectar cada integração: configurar **timeout do nó** (10 s + 1 retentativa + ramo de erro — doc 05, seção 2.2); a **Regra Anti-Espera** cobre a contingência.
- Cada integração conectada **substitui a contingência** do placeholder correspondente (doc 03).
