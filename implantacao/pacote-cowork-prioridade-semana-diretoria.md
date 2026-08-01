# Pacote Cowork — Prioridade da semana: fluxo pronto para apresentação à Diretoria

> **Para o Claude Cowork.** Este pacote existe porque a semana é curta e a apresentação à Diretoria é o objetivo imediato — **não** é para rodar tudo que está no índice de pacotes de uma vez. Siga **exatamente esta ordem, sem pular passos e sem improvisar** fora do que está escrito aqui. Se travar em algum passo, pare, registre o que travou e siga para o próximo — não invente solução.
>
> **O que fica de fora desta rodada (fazer depois, sem pressa)**: `pacote-cowork-auditoria-nos-fluxo.md` (auditoria profunda dos nós nativos) e a integração fina de field_ids do Pipefy (`pacote-cowork-build-pipefy-cx.md`, seção 7). Essas duas tarefas são de qualidade/robustez de longo prazo — não aparecem numa demonstração e consomem tempo desproporcional. O pipe do CX **já existe e já está referenciado** nos prompts, isso basta para a narrativa da demo ("o CX já tem o pipe pronto, a integração fina termina no próximo sprint").

## Objetivo desta rodada

Ter, até o fim da semana, um fluxo que na demonstração:
1. **Nunca fica mudo** quando o cliente muda de assunto.
2. **Sempre avisa explicitamente** quando está transferindo para um humano.
3. Mostra o **representante (A7) direcionando bem** um caso de cancelamento/estorno/prorrogação para o CX.
4. Tem um **tom de voz solícito**, sem soar corretivo ou frio.

## 1. Base de conhecimento (10 min)

- Abrir **Base de conhecimento** → artigo "Regras gerais de atendimento".
- Substituir o conteúdo pelo arquivo `implantacao/passo-1-base-conhecimento/artigo-02-regras-de-atendimento.md` (a partir da linha após o `---`).
- Confirmar que está vinculado aos 11 agentes.

## 2. Os 11 agentes (30–45 min)

Para cada agente, abrir na Núb.ia Resolve e **substituir as instruções** pelo conteúdo do arquivo correspondente (tabela abaixo — a partir da linha após o `---`, não o cabeçalho `>`):

| Agente | Arquivo |
|--------|---------|
| A1 | `implantacao/passo-3-agentes/prompt-A1.md` |
| A2 | `prompt-A2.md` |
| A3 | `prompt-A3.md` |
| A4 | `prompt-A4.md` |
| A5 | `prompt-A5.md` |
| A6 | `prompt-A6.md` |
| A7 | `prompt-A7.md` |
| A8 | `prompt-A8.md` |
| A9 | `prompt-A9.md` |
| A10 | `prompt-A10.md` |
| A11 | `prompt-A11.md` |

Trocar `[Assistente de IA Fitoway]` pelo nome oficial da persona (perguntar ao usuário se ainda não tiver). Salvar cada um.

## 3. Fluxo mestre — só o essencial para a demo (o passo mais importante desta rodada)

Abrir o fluxo de automação do canal WhatsApp (11) 2388-3360 no construtor. **Não é preciso auditar todos os nós nativos disponíveis nesta rodada** — use os nós que a plataforma já oferece para o que está descrito abaixo, da forma mais direta possível. O texto de referência completo é `implantacao/passo-2-fluxo-mestre/construcao-no-a-no.md`; aqui vai o recorte mínimo:

1. **Nó de reinício após o agente (N22)**: depois que um agente conclui o atendimento (ou quando detectar que o cliente quer outro assunto), o fluxo precisa perguntar:
   > "Ótimo! Já resolvi esse assunto para você. 😊 Quer falar de algo novo ou tem mais alguma dúvida?"
   - Opção "Sim, tenho outro assunto" → volta para o menu de triagem (a lista com as 10 opções de perfil).
   - Opção "Não, é só por agora" → pergunta se resolveu e encerra.
   - **Isso é o que resolve o bug que o Head reportou** (bot mudo ao trocar de assunto) — é o item mais crítico desta rodada.

2. **Mensagem de transbordo em 3 partes**, sempre que transferir para humano:
   - Reconhecimento: "Entendi, {{nome}}. Sinto muito por essa situação."
   - Ação: "Vou te passar agora para uma pessoa da nossa equipe que vai continuar ajudando você."
   - Contexto: "Já deixei tudo registrado aqui: [resumo]. Você não vai precisar contar tudo de novo."
   - Configurar isso como o texto padrão de todo nó de transferência para fila humana.

3. **Checagem de filas**: confirmar que toda fila de transbordo usada no fluxo está criada como subcategoria de **611808 FTW > Whatsapp**. Se alguma fila não existir ainda, criar antes de publicar (ver `implantacao/passo-5-filas/checklist-filas.md`).

## 4. Testar antes da demo (não pular — é o que evita vexame na frente da Diretoria)

Rodar estas 3 conversas de teste, exatamente como estão, e conferir o resultado esperado:

**Teste 1 — mudança de assunto (o bug original)**
- Iniciar como cliente do site perguntando sobre um pedido (perfil A1).
- Depois de uma resposta do agente, dizer: "na verdade, quero falar sobre outra coisa".
- **Esperado**: o bot pergunta se quer continuar ou tratar de algo novo, e **nunca fica em silêncio**.

**Teste 2 — transferência explícita**
- Em qualquer agente, escrever "quero falar com um atendente".
- **Esperado**: mensagem clara nas 3 partes (reconhecimento, ação, contexto), citando que está transferindo — não pode sumir da conversa sem avisar.

**Teste 3 — representante roteando para o CX**
- Simular um representante (A7) pedindo "o cliente quer cancelar o pedido dele".
- **Esperado**: o agente reconhece a intenção, coleta os dados (CNPJ, pedido, NF/boleto) e informa que vai encaminhar para o CX — nunca tenta resolver ou aprovar ele mesmo.

Se qualquer um dos 3 testes falhar, **não seguir para a apresentação** sem corrigir — esses são os que mais aparecem numa demo ao vivo.

## 5. Reportar

Ao final, reportar:
- Os 11 agentes atualizados (confirmar cada um).
- O nó de reinício (N22) e a mensagem de transbordo configurados no fluxo — como ficaram no construtor (nomes dos nós usados).
- Resultado dos 3 testes do passo 4 (passou/falhou, com prints se possível).
- Filas que faltavam e foram criadas.
- Qualquer coisa que não deu tempo de fazer, para entrar na próxima rodada (auditoria de nós + Pipefy fino).
