# Projeto IA FTW no WhatsApp — Assistente de IA Fitoway (NeoAssist / Núb.ia Resolve)

Documentação completa do fluxo único de atendimento no canal oficial de WhatsApp do Grupo Fitoway — **Número Consumidor (11) 2388-3360** — construída para implantação na plataforma **NeoAssist**, com a IA generativa **Núb.ia Resolve**.

## Objetivo

Um único número de WhatsApp recebe todos os públicos do Grupo Fitoway. Um **fluxo mestre de triagem** identifica o perfil de quem entra em contato e roteia a conversa para o **agente de IA especializado** naquele perfil. Cada agente tem persona, escopo, integrações e **guardrails regulatórios próprios** (CDC, LGPD, ANVISA, CFM/CFN, CONAR e demais órgãos que assistem cada segmento).

## Públicos atendidos e agentes correspondentes

| # | Perfil | Agente | Documento |
|---|--------|--------|-----------|
| 1 | Clientes que compraram no site FTW (www.ftw.com.br) | A1 — Cliente Site FTW | [docs/agentes/A1-agente-cliente-site-ftw.md](docs/agentes/A1-agente-cliente-site-ftw.md) |
| 2 | Interessados que ainda não compraram (prospects) | A2 — Prospect Consumidor | [docs/agentes/A2-agente-prospect-consumidor.md](docs/agentes/A2-agente-prospect-consumidor.md) |
| 3 | Clientes de lojas oficiais em marketplaces | A3 — Cliente Marketplace | [docs/agentes/A3-agente-cliente-marketplace.md](docs/agentes/A3-agente-cliente-marketplace.md) |
| 4 | Clientes que compraram em PDVs (pontos de venda físicos) | A4 — Cliente PDV | [docs/agentes/A4-agente-cliente-pdv.md](docs/agentes/A4-agente-cliente-pdv.md) |
| 5 | Clientes B2B do canal Farma no estado de São Paulo (JL FIT) | A5 — B2B Farma SP | [docs/agentes/A5-agente-b2b-farma-sp.md](docs/agentes/A5-agente-b2b-farma-sp.md) |
| 6 | Clientes B2B nacionais: Alimentar, Varejo, BodyShop, Canal Verde e Farma fora de SP | A6 — B2B Nacional | [docs/agentes/A6-agente-b2b-nacional.md](docs/agentes/A6-agente-b2b-nacional.md) |
| 7 | Representantes comerciais (Farma SP e demais canais nacionais) | A7 — Representantes | [docs/agentes/A7-agente-representantes.md](docs/agentes/A7-agente-representantes.md) |
| 8 | Profissionais de saúde parceiros (médicos, nutricionistas, nutrólogos) | A8 — Profissionais de Saúde | [docs/agentes/A8-agente-profissionais-saude.md](docs/agentes/A8-agente-profissionais-saude.md) |
| 9 | Influenciadores digitais e celebridades (programa de afiliados) | A9 — Influenciadores e Afiliados | [docs/agentes/A9-agente-influenciadores-afiliados.md](docs/agentes/A9-agente-influenciadores-afiliados.md) |

## Estrutura da documentação

```
docs/
├── 01-identidade-e-regras-gerais.md      # Quem é a assistente, tom de voz, regras válidas para TODOS os agentes
├── 02-fluxo-mestre-triagem.md            # Fluxo de entrada, identificação, consentimento LGPD e roteamento
├── 03-integracoes.md                     # Integrações NeoAssist, e-commerce, ERP, logística, CRM, afiliados
├── 04-guardrails-globais-compliance.md   # CDC, LGPD, ANVISA e matriz regulatória por agente
├── 05-transbordo-humano-e-filas.md       # Regras de transferência, filas, horários e protocolo
├── 06-metricas-e-monitoramento.md        # KPIs, curadoria e melhoria contínua
├── lgpd/
│   ├── aviso-de-privacidade-whatsapp-ftw.md              # Texto público (minuta p/ validação jurídica/DPO)
│   └── lgpd-operacional-consentimento-ropa-direitos.md   # Consentimento, ROPA e direitos do titular
└── agentes/
    ├── A1-agente-cliente-site-ftw.md
    ├── A2-agente-prospect-consumidor.md
    ├── A3-agente-cliente-marketplace.md
    ├── A4-agente-cliente-pdv.md
    ├── A5-agente-b2b-farma-sp.md
    ├── A6-agente-b2b-nacional.md
    ├── A7-agente-representantes.md
    ├── A8-agente-profissionais-saude.md
    └── A9-agente-influenciadores-afiliados.md
```

## Como implantar na NeoAssist

> **Pacote pronto para colar**: o diretório [`implantacao/`](implantacao/00-guia-de-implantacao.md) contém os artefatos finais de cada passo — artigos da base de conhecimento redigidos, fluxo mestre nó a nó com os textos exatos, prompts de configuração dos 9 agentes e checklists de integrações e filas. Comece pelo [guia de implantação](implantacao/00-guia-de-implantacao.md).

1. **Base de conhecimento**: cadastre o conteúdo de `01-identidade-e-regras-gerais.md` e `04-guardrails-globais-compliance.md` como artigos-base vinculados a todos os agentes da Núb.ia Resolve.
2. **Fluxo de automação**: monte o fluxo de entrada do WhatsApp conforme `02-fluxo-mestre-triagem.md` (boas-vindas → identificação via integração de consumidor → consentimento LGPD → menu de triagem → roteamento).
3. **Agentes**: crie um agente Núb.ia Resolve por perfil (A1 a A9), copiando de cada documento as seções *Persona*, *Escopo*, *Proibições*, *Guardrails* e *Fluxo conversacional*.
4. **Integrações**: conecte as APIs listadas em `03-integracoes.md`. Enquanto uma integração não estiver disponível, o fluxo usa o comportamento de contingência descrito no mesmo documento.
5. **Filas de transbordo**: crie as filas humanas descritas em `05-transbordo-humano-e-filas.md` e vincule cada agente à sua fila.

## Documentos de referência do projeto

- Briefing "Projeto IA FTW no WhatsApp" (mensagem do solicitante).
- PDF "Orientações para construção de chatbot com Núb.ia Resolve" (NeoAssist).
- PDF "Definição da persona da Núb.ia".
- "Aviso de Privacidade — Atendimento por WhatsApp da FTW" e "LGPD Operacional — consentimento, ROPA e direitos do titular" (`docs/lgpd/`, minutas aguardando validação do jurídico/DPO).
- Guia NeoAssist — Fluxo de Automações: <https://neoassist.getoutline.com/s/6acb61f6-9250-406a-a50f-2d74e0011738/doc/fluxo-de-automacoes-4ogIYYglMW>
