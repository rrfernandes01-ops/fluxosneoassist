# Passo 2 — Fluxo mestre: construção nó a nó

> Montar em: **Automações → novo fluxo → canal WhatsApp (11) 2388-3360 → gatilho: nova conversa**. Os textos abaixo são finais — colar como estão (substituindo os placeholders). Variáveis do fluxo estão em `{{chaves}}`.

## Variáveis do fluxo

| Variável | Origem | Uso |
|----------|--------|-----|
| `{{telefone}}` | Canal | Consulta I-01 |
| `{{nome}}` | I-01 ou coleta | Saudação, transbordo |
| `{{documento}}` | I-01 (mascarado) ou coleta | Validação de identidade |
| `{{perfil}}` | I-01 (se já classificado) ou triagem | Roteamento A1–A9 |
| `{{protocolo_recente}}`, `{{assunto_recente}}` | I-02 | Oferta de continuidade |
| `{{consentimento_ok}}` | I-01/I-03 | Bloqueio de prosseguimento |

## Sequência de nós

### N01 — Mensagem: boas-vindas (bloco 1)
> Oi! Eu sou a [Assistente de IA Fitoway], assistente virtual do Grupo Fitoway. 😊

### N02 — Atraso: 2 segundos

### N03 — Mensagem: boas-vindas (bloco 2)
> Estou aqui para te ajudar de forma rápida.

### N03-B — Mensagem: transparência LGPD (obrigatória, sem pedir aceite)
Enviada na abertura, junto da saudação, **para todos** (cadastrados ou não). Registrar a exibição em log (data/hora) via `[[INT_CONSENTIMENTO]]`.
> Antes de começarmos, um aviso rápido: para te atender, a gente trata os seus dados conforme a nossa Política de Privacidade, que você lê aqui: [LINK_CONSENTIMENTO]. Você pode falar com um atendente humano quando quiser e pedir informações sobre os seus dados a qualquer momento.

**Importante**: o atendimento **não depende de consentimento** (base legal: execução de contrato, art. 7, V + legítimo interesse, art. 7, IX). Este nó apenas informa — **não há pergunta de aceite nem bloqueio**. Consentimento só existe para marketing (N21).

### N04 — Integração `[[INT_CONSUMIDOR]]` (I-01): consumidor por telefone
Entrada `{{telefone}}` → preenche `{{nome}}`, `{{documento}}`, `{{perfil}}`, `{{consentimento_ok}}`, `{{identidade_validada}}`.
**Contingência (integração indisponível/erro)**: seguir pelo ramo "não encontrado".

### N05 — Condição: consumidor encontrado?
- **Sim** → N06. **Não** → N10.

### N06 — Integrações `[[INT_HISTORICO]]` + `[[INT_PROTOCOLOS]]` (I-02): histórico e protocolos abertos/fechados
**Contingência**: pular para N09.
**Identificação persistente**: se `{{identidade_validada}}` = sim, nenhum nó posterior (nem os agentes) pede identificação de novo — o atendimento segue mostrando que já sabe com quem fala. Se o histórico trouxer solução disponível (ex.: pedido saiu para entrega, troca aprovada), o agente a apresenta proativamente na abertura.

### N07 — Condição: existe protocolo em aberto?
- **Sim** → N08. **Não** → N09.

### N08 — Mensagem: continuidade
> Que bom te ver de novo, {{nome}}!
>
> Vi aqui que seu último contato foi sobre {{assunto_recente}}. Quer continuar esse assunto ou falar de algo novo?

- "Continuar" → hand-off direto ao agente do `{{perfil}}` com o protocolo carregado (N20).
- "Algo novo" → N09.

### N09 — Condição: perfil já gravado?
- **Sim** → N16 (pergunta aberta). **Não** → N15 (menu de triagem).

### N10 — Mensagem + entrada: coleta de nome
> Para eu conseguir te atender direitinho, pode me dizer seu nome completo?

Salvar em `{{nome}}`.

### N11 — Mensagem + entrada: coleta de CPF
> Obrigada, {{nome}}! Agora preciso do seu CPF, só para criar seu cadastro com segurança.

Validar formato de CPF (revalidar 1 vez em caso de erro; na 2ª falha, oferecer transbordo — T3). Se a pessoa disser que é empresa/revenda, aceitar CNPJ e marcar pré-perfil B2B.

### N12 — Mensagem: conveniência do cadastro
> Prontinho. Sempre que você falar com a gente por este mesmo número, não vai precisar confirmar seus dados de novo.

### N13 — Prosseguir após cadastro (sem gate de consentimento)
A transparência LGPD já foi dada no N03-B — **não repetir aviso nem pedir aceite aqui**. Seguir direto para N15. Se o cliente fizer qualquer pergunta sobre os dados dele, aplicar o roteiro de privacidade do Artigo 06 (registro + classificação LGPD + fila Privacidade/DPO).

### N14 — (reservado para reentrada de subfluxos)

### N15 — Menu interativo: triagem de perfil
> Para te direcionar para o time certo, me conta: qual dessas opções combina com você?

Lista interativa WhatsApp:
1. Comprei no site ftw.com.br
2. Quero comprar / conhecer os produtos
3. Comprei em marketplace (Mercado Livre, Amazon, Shopee, Magalu, TikTok Shop…)
4. Comprei em loja física
5. Sou farmácia ou drogaria
6. Sou lojista / revenda (outros canais)
7. Sou representante comercial
8. Sou profissional de saúde e quero parceria
9. Sou influenciador(a) e quero ser afiliado(a)

### N16 — Entrada: pergunta aberta (clientes já conhecidos)
> Como posso te ajudar hoje?

→ Classificação de intenção pela Núb.ia. Confiança ≥ limiar → N17; senão → N15.

### N17 — Sub-roteamento farma por UF (apenas opção 5)
> Sua farmácia fica em qual cidade e estado?

- UF = **SP** → perfil A5. UF ≠ SP → perfil A6.

### N18 — Ação: gravar `{{perfil}}` no cadastro do contato (campo customizado)

### N19 — Condição: assunto restrito detectado em qualquer ponto?
(jurídico, Procon/Reclame Aqui/advogado/processo, dívida/cobrança, LGPD, imprensa)
→ **Transbordo imediato** para a fila correspondente (ver passo 5), com mensagem de transbordo do Artigo 04. Este ramo tem prioridade sobre qualquer outro.

### N20 — Hand-off: agente Núb.ia Resolve do perfil
Roteia para A1–A9 passando: `{{nome}}`, `{{documento}}`, `{{perfil}}`, `{{protocolo_recente}}`, resumo da triagem.

### N21 — Opt-in de marketing (opcional; somente após atendimento bem resolvido)
Oferecido **apenas** ao final de um atendimento resolvido (resposta positiva à pergunta de resolução), **nunca como pedágio** e no máximo 1 vez por período definido pela curadoria. Registrar via `[[INT_CONSENTIMENTO]]`: `optin_marketing = sim/nao` + data/hora + versão do aviso vigente.
> Quer receber por aqui novidades e ofertas exclusivas da FTW? É opcional e você pode sair quando quiser. Responda: 1 para Sim, quero receber; 2 para Não, obrigado.

Opt-out a qualquer momento (SAIR ou equivalente): atualizar `optin_marketing = nao` com data/hora e classificar o protocolo — efeito imediato.

## Regras transversais (configurar no fluxo/canal)

- **Áudio**: mensagem recebida em áudio → transcrever, processar e responder em áudio.
- **Mídia não suportada** (figurinha, imagem sem texto): pedir descrição em texto; na 2ª falha, oferecer transbordo.
- **Inatividade**: 30 min sem resposta → 1 lembrete gentil; +24h → encerrar com protocolo.
- **Encerramento**: todo fim de conversa sem transbordo dispara a pergunta de resolução:
> Antes de eu encerrar: a solução que te apresentei resolveu o problema?
  - Sim → agradecer e encerrar. Não → oferecer transbordo imediatamente.
- **Troca de perfil no meio da conversa**: hand-off interno ao agente correto levando o resumo — o usuário não repete nada.
- **Identificação persistente**: identidade validada uma única vez (telefone, CPF ou CNPJ) fica gravada no cadastro (`{{identidade_validada}}`); em qualquer retorno, nenhum agente repete a identificação positiva — cumprimenta pelo nome e conduz. Confirmação adicional só em divergência de identidade ou operação sensível (documento 01, seção 6.1).
- **Placeholders de integração**: os conectores são referenciados como `[[INT_*]]` (mapa no documento 03, seção 1.1); substituir pelos conectores reais conforme as documentações forem chegando.
- **Sem resposta de integração (T9 — vale em qualquer nó e em todos os agentes)**: dado não localizado, integração indisponível ou cliente sem o dado em mãos → uma única tentativa de ajudar (onde encontrar o dado / busca por dado alternativo) e, sem sucesso, transbordo calmo e transparente com tudo o que já foi coletado. Nunca encerrar por falta de dado nem repetir a mesma coleta.
