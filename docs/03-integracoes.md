# 03 — Integrações do fluxo

> O fluxo deve utilizar **todo tipo de integração possível para trazer previsibilidade** ao atendimento. As integrações podem ser conectadas ao longo do projeto; o fluxo já nasce preparado para elas, com **comportamento de contingência** definido para quando ainda não estiverem disponíveis.

## 1. Catálogo de integrações

| ID | Integração | Uso no fluxo | Agentes | Prioridade |
|----|------------|--------------|---------|------------|
| I-01 | **NeoAssist — consumidor por telefone** | Identificar se o contato já existe na base ao entrar no canal | Fluxo mestre | P0 (essencial) |
| I-02 | **NeoAssist — histórico e protocolos** | Ler o histórico de atendimentos anteriores e os protocolos **abertos e fechados**, preparando o contexto e permitindo apresentar soluções proativamente | Fluxo mestre, todos | P0 |
| I-03 | **Transparência e opt-in LGPD** | Registrar em log a exibição do aviso de transparência (data/hora + versão) e gravar o opt-in/opt-out de marketing (`optin_marketing` com data/hora + versão do aviso) | Fluxo mestre | P0 |
| I-04 | **E-commerce FTW (plataforma do site)** | Status de pedido, itens, pagamento, NF, trocas/devoluções | A1, A2 | P0 |
| I-05 | **Logística / transportadora (rastreio)** | Rastreamento de entrega em tempo real e previsão de entrega | A1, A5, A6 | P0 |
| I-06 | **Catálogo de produtos** | Ficha técnica, composição, tabela nutricional, modo de uso, estoque, preço no site | A1, A2, A3, A4, A8 | P0 |
| I-07 | **ERP / faturamento JL FIT e FTW B2B** | Pedidos B2B, faturamento, títulos, prazos, tabela de preço por canal | A5, A6, A7 | P1 |
| I-08 | **CRM comercial B2B** | Cadastro de leads B2B, carteira por representante, status de crédito (somente sinalização — negociação é humana) | A5, A6, A7 | P1 |
| I-09 | **Base de representantes** | Validar vínculo do representante (código, canal, região) | A7 | P1 |
| I-10 | **Plataforma de afiliados** | Cadastro de afiliado, status de aprovação, link/cupom, painel de comissões | A9 | P1 |
| I-11 | **Base de profissionais parceiros** | Cadastro e validação de CRM/CRN, status da parceria | A8 | P1 |
| I-12 | **Consulta CNPJ (Receita/serviço externo)** | Validar CNPJ e UF na triagem B2B | Fluxo mestre, A5, A6 | P2 |
| I-13 | **Pagamentos (gateway do site)** | Status de pagamento, reenvio de boleto/Pix (sem dados de cartão) | A1 | P2 |
| I-14 | **Agenda de visitas / distribuidor** | Agendar visita de representante para lead B2B qualificado | A5, A6 | P2 |
| I-15 | **Localizador de PDVs / lojas oficiais** | Indicar ponto de venda ou loja oficial de marketplace mais próxima | A2, A4 | P2 |

## 1.1 Placeholders de integração (substituíveis)

As documentações técnicas das integrações ainda serão fornecidas. Até lá, **todo o projeto (documentos e prompts dos agentes) referencia cada integração por um placeholder padronizado**, no formato `[[INT_*]]`. Quando a documentação real de uma integração chegar: (1) atualizar o contrato dela neste documento; (2) substituir o placeholder pelo conector real na plataforma NeoAssist; (3) desativar a contingência correspondente; (4) rodar os testes de aceitação (incluindo o T9 com a integração desligada).

| Placeholder | Integração (ID) | O que executa |
|-------------|-----------------|---------------|
| `[[INT_CONSUMIDOR]]` | I-01 | Integração de consumidores: busca do cadastro por telefone, CPF ou CNPJ |
| `[[INT_HISTORICO]]` | I-02 | Leitura do histórico de atendimentos anteriores |
| `[[INT_PROTOCOLOS]]` | I-02 | Leitura de protocolos anteriores, fechados ou abertos |
| `[[INT_CONSENTIMENTO]]` | I-03 | Log do aviso de transparência LGPD e registro do opt-in/opt-out de marketing |
| `[[INT_ERP_B2C]]` | I-04 | Leitura do ERP/e-commerce B2C: pedidos, pagamento, NF, trocas |
| `[[INT_RASTREIO]]` | I-05 | Rastreamento de entregas |
| `[[INT_CATALOGO]]` | I-06 | Catálogo, fichas oficiais, preço e estoque |
| `[[INT_ERP_B2B]]` | I-07 | Leitura do ERP B2B (JL FIT e FTW): pedidos, faturamento, títulos, tabelas |
| `[[INT_CRM_B2B]]` | I-08 | CRM comercial B2B: leads e carteiras |
| `[[INT_REPRESENTANTES]]` | I-09 | Base de representantes: validação de vínculo |
| `[[INT_PARCEIROS]]` | I-11 | Base de parceiros profissionais (CRM/CRN) |
| `[[INT_AFILIADOS]]` | I-10 | Base/plataforma de influenciadores e afiliados |
| `[[INT_CNPJ]]` | I-12 | Consulta e validação de CNPJ |
| `[[INT_PAGAMENTOS]]` | I-13 | Gateway de pagamentos B2C |
| `[[INT_AGENDA]]` | I-14 | Agenda de visitas de representantes |
| `[[INT_PDV]]` | I-15 | Localizador de PDVs e lojas oficiais |

## 2. Contratos esperados (resumo funcional)

### I-01 — Consumidor por telefone
- **Entrada**: telefone (E.164) do WhatsApp.
- **Saída**: `encontrado (bool)`, `id_cliente`, `nome`, `documento_mascarado`, `perfil` (A1–A9 se já classificado), `consentimento_lgpd (bool, versão, data)`.
- **Contingência**: tratar como cliente novo (subfluxo de cadastro do documento 02).

### I-02 — Histórico de protocolos
- **Entrada**: `id_cliente`.
- **Saída**: lista dos últimos N protocolos: `numero`, `assunto`, `status (aberto/resolvido)`, `data`, `resumo`.
- **Contingência**: seguir para pergunta aberta sem oferta de continuidade.

### I-04 — Pedidos e-commerce
- **Entrada**: CPF validado ou nº do pedido.
- **Saída**: pedidos com `status`, `itens`, `pagamento`, `nf`, `transportadora`, `codigo_rastreio`, `previsao_entrega`.
- **Contingência**: coletar nº do pedido + CPF, registrar protocolo e transbordar para a fila Consumidor.

### I-05 — Rastreio
- **Entrada**: `codigo_rastreio`.
- **Saída**: eventos de transporte + previsão.
- **Contingência**: informar o código e o link público da transportadora.

*(Demais integrações seguem o mesmo padrão: entrada mínima, saída estruturada e contingência que nunca deixa o usuário sem caminho — na dúvida, protocolo + transbordo.)*

## 3. Regras de uso das integrações

1. **Previsibilidade primeiro**: sempre que existir dado de sistema (status real do pedido, posição do rastreio, título em aberto), a assistente responde com o dado — nunca com estimativa genérica.
2. **Validação antes de exposição**: nenhuma integração que retorne dado pessoal ou financeiro é consultada antes da validação de identidade (documento 02, seção 3.3).
3. **Dados mascarados**: documentos e dados de contato retornados por integração são exibidos mascarados.
4. **Sem resposta da integração — transbordo garantido (T9)**: em erro, timeout, dado não localizado ou integração ainda não conectada, a assistente **não inventa** o dado e **não insiste na coleta**: após uma única tentativa de ajudar o cliente a encontrar a informação (ou buscar por dado alternativo), faz o transbordo calmo e transparente para o humano, levando tudo o que o cliente já forneceu. Vale igualmente para B2C, marketplace e B2B — onde é comum o cliente não ter o nº do pedido em mãos. Detalhamento no documento 05, seção 2.1.
5. **Escrita em sistemas** (criar lead, registrar aceite, agendar visita): toda escrita retorna confirmação ao usuário com número de protocolo/registro.
6. **Logs**: toda chamada de integração fica associada ao protocolo do atendimento para auditoria (LGPD — minimização e rastreabilidade).
