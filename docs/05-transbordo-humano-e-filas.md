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
| **Parcerias Profissionais** | A8 | Médicos, nutricionistas, nutrólogos |
| **Marketing / Afiliados** | A9 | Influenciadores, programa de afiliados |
| **Privacidade / DPO** | Todos | LGPD e direitos de titular |
| **Jurídico / Ouvidoria** | Todos | Procon, Reclame Aqui, advogado, processo, imprensa |
| **Cobrança / Crédito** | A5, A6 | Negociação de dívida, crédito, cobrança |

Cada agente indica no próprio documento a fila-padrão de destino. O resumo estruturado (perfil, protocolos, variáveis coletadas) acompanha o ticket na NeoAssist.

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
- O protocolo carrega: perfil (A1–A9), variáveis coletadas, integrações consultadas, resumo da conversa e motivo do transbordo (T1–T8) — insumo para as métricas do documento 06.
