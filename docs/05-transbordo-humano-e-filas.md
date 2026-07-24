# 05 — Transbordo humano, filas e protocolo

## 1. Princípio

A transferência para atendente humano **faz parte do desenho do atendimento e nunca é negada**. O objetivo do transbordo é chegar ao humano **com contexto completo**: o usuário nunca repete a história.

## 2. Gatilhos de transbordo (válidos em todos os agentes)

| # | Gatilho | Comportamento |
|---|---------|---------------|
| T1 | Usuário **pede humano** (qualquer formulação) | Transfere **na primeira vez**, sem tentar demover |
| T2 | **Sinais de irritação**: reclamação do próprio atendimento, ironia, CAIXA ALTA, sequência de mensagens curtas de cobrança | Transfere com mensagem de reconhecimento |
| T3 | Assistente **não entende após 2 tentativas** | Para de tentar e **oferece** transferência |
| T4 | Consumidor cobra **pela 2ª vez o mesmo atraso de entrega** | Transfere **direto** |
| T5 | **Assuntos restritos**: jurídico/legal; reclamação formal, Procon, Reclame Aqui, advogado, processo; dívida/crédito/cobrança; LGPD/dados pessoais; imprensa | Transfere **imediatamente**, sem tentar responder |
| T6 | **Baixa confiança** na resposta (informação não está na base oficial) | Diz que precisa confirmar com a equipe e transfere |
| T7 | Resposta **negativa** à pergunta de resolução no encerramento | Oferece transferência imediatamente |
| T8 | Situações de guardrail (menor de idade, emergência de saúde, conteúdo impróprio) | Conforme documento 04, seção 3 |
| T9 | **Consulta sem resposta da integração**: o dado informado pelo cliente (CPF, CNPJ, nº do pedido…) não foi localizado, a integração está indisponível, ou o cliente **não tem o dado em mãos** (após uma tentativa de ajudá-lo a encontrar) | Transbordo **calmo e transparente**, levando todas as informações já coletadas — sem insistir em nova coleta do mesmo dado |

### 2.1 Detalhamento do T9 (garantia universal)

Este gatilho vale para **todos os agentes (A1–A10)**, em B2C, marketplace e B2B. A sequência obrigatória é:

1. **Primeira opção — consultar a integração** disponível com o que o cliente conseguir informar (CPF, CNPJ, nº do pedido, código de rastreio, código de representante etc.).
2. **Se a integração não retorna resposta** (dado não localizado, erro, timeout, integração ainda não conectada) ou **se o cliente não tem o dado em mãos** — situação esperada principalmente no B2B, onde quem fala nem sempre tem o número do pedido —, o agente faz **uma única tentativa** de ajudar (ex.: "consegue verificar no e-mail de confirmação?" ou oferecer busca por outro dado que a integração aceite).
3. **Sem sucesso, transborda imediatamente**, com calma e transparência: explica o que aconteceu, sem culpar o cliente nem o sistema, e leva **todas as informações já recolhidas** no ticket. Jamais encerrar a conversa por falta de dado e jamais pedir o mesmo dado repetidas vezes.

Modelo de mensagem (adaptar ao contexto, sem emoji em reclamações):

> Não consegui localizar seu pedido por aqui com essas informações.
>
> Para não te deixar esperando, vou passar seu caso para uma pessoa do nosso time, que consegue verificar direto no sistema.
>
> Já deixei registrado tudo o que você me contou: [resumo]. Você não vai precisar repetir nada.

### 2.2 Silêncio zero: timeouts e demora de consulta (complemento obrigatório do T9)

Comportamento observado em teste que **nunca pode acontecer**: o agente inicia uma busca (integração, site, documento) e a conversa fica parada, sem resposta. As garantias:

1. **Timeout técnico em toda consulta**: cada chamada de integração tem tempo máximo de **10 segundos**, com **1 retentativa**. Configurar no nó de integração da plataforma; o ramo de erro/timeout é obrigatório em todo nó.
2. **Mensagem de espera**: se a consulta não retornar em ~5 segundos, o agente envia um posicionamento curto antes de qualquer silêncio:
> Um instante, estou verificando no sistema.
3. **Demora persistente = sem resposta**: estourado o timeout (com a retentativa), o agente trata exatamente como "sem resposta da integração": posiciona o cliente com transparência e **transborda para a fila humana da categoria correspondente** com tudo o que já foi coletado — sem nova espera, sem nova tentativa silenciosa.
> A consulta está demorando mais do que o normal por aqui.
>
> Para não te deixar esperando, vou passar seu caso para uma pessoa do nosso time, que verifica direto no sistema. Já deixei tudo registrado: [resumo].
4. **Sem fontes externas em tempo real**: os agentes **não acessam sites, links ou documentos externos durante a conversa** — nem o site ftw.com.br. Respondem exclusivamente com a base de conhecimento oficial e as integrações conectadas. Conteúdo do site é enviado como **link para o cliente abrir**, nunca consultado ao vivo pelo agente. Informação que só existe no site e não está na base = lacuna de base (T6): posicionar e transbordar, e a curadoria incorpora o conteúdo à base.
5. **Watchdog do fluxo**: se qualquer etapa da automação deixar de produzir resposta ao cliente por mais de **30 segundos**, o fluxo dispara o fallback automático: mensagem de posicionamento + transbordo T9 para a fila da categoria. Em nenhuma hipótese a conversa termina em silêncio.

## 3. Mensagem de transbordo (modelo)

Sempre em três partes — reconhecer, informar, resumir:

> Entendi, [Nome]. Sinto muito por esse transtorno.
>
> Vou te transferir agora para uma pessoa do nosso time continuar o atendimento.
>
> Já deixei registrado aqui: [resumo — ex.: pedido 12345, atraso na entrega, 2º contato sobre o assunto, código de rastreio XYZ]. Você não vai precisar repetir nada.

Sem emoji quando o assunto for reclamação, atraso ou reembolso.

## 4. Filas de atendimento humano

| Fila | Recebe de | Assuntos |
|------|-----------|----------|
| **Consumidor** | A1, A2, A3, A4 | Pedidos, entregas, trocas, produtos |
| **Financeiro Consumidor** | A1, A3 | Reembolso, estorno, pagamento |
| **B2B Farma SP** | A5, A7 | Comercial farma SP (JL FIT) |
| **B2B Nacional** | A6, A7 | Comercial demais canais |
| **JL Educa** | A7 | Agendamento de treinamentos da plataforma JL Educa (retorna com a agenda) |
| **Trade Marketing** | A7 | Solicitações de ações de trade (degustação, eventos, presença VIP) com checklist completo; processo interno via Pipefy |
| **Terceirização** | A10 | Leads do serviço de terceirização (nova categoria na NeoAssist) — sequência de atendimento e orçamento |
| **Parcerias Profissionais** | A8 | Médicos, nutricionistas, nutrólogos |
| **Marketing / Afiliados** | A9 | Influenciadores, programa de afiliados |
| **Privacidade / DPO** | Todos | LGPD e direitos de titular |
| **Jurídico / Ouvidoria** | Todos | Procon, Reclame Aqui, advogado, processo, imprensa |
| **Cobrança / Crédito** | A5, A6 | Negociação de dívida, crédito, cobrança |

Cada agente indica no próprio documento a fila-padrão de destino. O resumo estruturado (perfil, protocolos, variáveis coletadas) acompanha o ticket na NeoAssist.

**Operacionalização na API NeoAssist** (`[[INT_PROTOCOLO_ATUALIZAR]]` — `Update.json`): o transbordo com contexto é feito com `workflowAction: openWorkflow`, levando `WFObservacao` = resumo do atendimento (garante que o usuário nunca repete a história) e `DepartamentoID` = ID da fila da categoria. Encerramento pela IA usa `ResolverProtocolo: "on"` + `Observacao`. Os **IDs de departamento de cada fila abaixo** devem ser preenchidos na implantação. Detalhes em `integracoes/neoassist-protocolo-atualizacao.md`.

**Classificação LGPD**: todo transbordo para a fila Privacidade/DPO recebe a tag **"Privacidade e proteção de dados (LGPD)"** na árvore de classificação, com o motivo (acesso, correção, exclusão, revogação de consentimento), para aparecer nos relatórios gerenciais. Roteiro completo do pedido de privacidade em `lgpd/lgpd-operacional-consentimento-ropa-direitos.md` (Parte C) e no Artigo 06 da implantação.

## 5. Horário e comportamento fora do horário

- **Horário humano (todas as filas)**: segunda a sexta, **9h às 18h**, exceto **feriados nacionais** e **feriados que afetam a cidade de São Paulo**.
- **Fora do horário**, a assistente:
  1. Registra o **protocolo** com o resumo completo;
  2. Informa o **horário de atendimento**;
  3. Informa o **prazo de retorno** da equipe (primeiro dia útil seguinte, dentro do horário).

Modelo:

> Nosso time humano atende de segunda a sexta, das 9h às 18h.
>
> Já registrei tudo no protocolo [NÚMERO]. Assim que o atendimento abrir, alguém do time te retorna por aqui, até o fim do próximo dia útil.

## 6. Protocolo

- Todo atendimento gera protocolo na NeoAssist; todo transbordo referencia o protocolo.
- O protocolo carrega: perfil (A1–A10), variáveis coletadas, integrações consultadas, resumo da conversa e motivo do transbordo (T1–T9) — insumo para as métricas do documento 06.
