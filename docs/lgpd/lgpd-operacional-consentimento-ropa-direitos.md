# LGPD na Núb.ia por WhatsApp: consentimento, ROPA e direitos do titular

**Versão:** 1.0 (uso interno, base para governança e configuração)
**Data:** 【CONFIRMAR: data】
**Escopo:** canal de WhatsApp de atendimento ao consumidor da FTW, número (11) 2388-3360, operado no NeoAssist com atendimento inicial por IA (Núb.ia)

> Documento de trabalho da área de CX. A base legal, os prazos de retenção e o texto público do Aviso de Privacidade precisam de validação do jurídico e do encarregado de dados (DPO). Pontos marcados com 【CONFIRMAR】 dependem de dado interno. Este material não é parecer jurídico.

---

## Parte A: Fluxo de consentimento no WhatsApp

### A.1. O ponto de partida correto

Existe um erro comum que vale evitar desde já: tratar o atendimento como se dependesse de consentimento. Não depende. Quando o cliente inicia a conversa no WhatsApp, o tratamento dos dados para atender e resolver a solicitação se apoia na execução do contrato e de procedimentos preliminares (art. 7, V da LGPD) e no legítimo interesse (art. 7, IX). Ou seja, a gente não precisa pedir "você autoriza tratar seus dados?" para poder atender.

O que a LGPD exige nesse cenário é transparência: informar, de forma clara e acessível, como os dados são tratados. Isso se resolve com um aviso no início da conversa e um link para o Aviso de Privacidade.

O consentimento (art. 7, I) entra só para o que é opcional, em especial o marketing. Para enviar novidades e ofertas de forma proativa, aí sim é preciso um opt-in claro, separado, e com opção fácil de sair.

Resumo da régua:

| Situação | O que fazer | Base legal |
|---|---|---|
| Atender a solicitação que o cliente iniciou | Informar (aviso + link), não pedir consentimento | Execução de contrato (art. 7, V) e legítimo interesse (art. 7, IX) |
| Enviar marketing e novidades de forma proativa | Pedir opt-in claro e registrar o aceite | Consentimento (art. 7, I) |

### A.2. Mensagem de transparência na abertura (obrigatória)

Inserir logo no início do fluxo, junto da saudação, antes ou junto do menu principal. Texto sugerido, sem pedir consentimento, apenas informando:

> Antes de começarmos, um aviso rápido: para te atender, a gente trata os seus dados conforme a nossa Política de Privacidade, que você lê aqui: [LINK_CONSENTIMENTO]. Você pode falar com um atendente humano quando quiser e pedir informações sobre os seus dados a qualquer momento.

O marcador [LINK_CONSENTIMENTO] deve apontar para o Aviso de Privacidade publicado (o documento público que acompanha este pacote). Enquanto o mkt e o jurídico não definirem a URL final, o marcador fica reservado.

### A.3. Opt-in de marketing (opcional, recomendado separar)

Se e quando a FTW quiser usar o WhatsApp para marketing, o aceite precisa ser coletado à parte, sem se misturar com o atendimento. Sugestão de abordagem, oferecida no fim de um atendimento bem resolvido, nunca como pedágio para atender:

> Quer receber por aqui novidades e ofertas exclusivas da FTW? É opcional e você pode sair quando quiser. Responda: 1 para Sim, quero receber; 2 para Não, obrigado.

Regras do opt-in:

- É sempre opcional e nunca condiciona o atendimento.
- Toda mensagem de marketing traz uma forma simples de sair (por exemplo, responder SAIR).
- O opt-out é respeitado de imediato.

### A.4. Como registrar e provar no NeoAssist

O que registrar, para que a empresa tenha prova do tratamento e do aceite:

- **Aviso de transparência exibido:** registrar que a mensagem de abertura com o link foi apresentada, com data e hora. Como o aviso é passivo, basta o log do fluxo.
- **Opt-in de marketing:** gravar como dado do consumidor ou tag, por exemplo `optin_marketing = sim` ou `nao`, com data, hora e a versão do aviso vigente no momento do aceite. Usar a capacidade Salvar dados do consumidor dos agentes e, se necessário, uma tag gerencial dedicada.
- **Retirada do consentimento:** ao receber um SAIR ou pedido equivalente, atualizar para `optin_marketing = nao` com data e hora, e classificar o protocolo.

【LACUNA DE WORKFLOW: definir com o time de NeoAssist se o opt-in vira campo estruturado do consumidor, tag gerencial, ou os dois, e como esse dado sincroniza com a Tray e com a base de marketing.】

### A.5. Onde isso encaixa no fluxo mestre já montado

O fluxo mestre de triagem no WhatsApp já existe e está inativo. Para o consentimento e a transparência, os ajustes são:

1. Incluir a mensagem de transparência da seção A.2 na saudação inicial (hoje o texto de boas-vindas do menu principal já reserva o marcador do link).
2. Se for adotar marketing, criar um ramo de opt-in ao fim dos atendimentos resolvidos, com registro conforme A.4.

Posso implementar os dois no fluxo assim que você aprovar o texto e o mkt liberar a URL do Aviso.

---

## Parte B: Registro das Operações de Tratamento (ROPA)

Registro interno das operações de tratamento de dados neste canal, para governança e para demonstrar conformidade. Revisar e completar os campos 【CONFIRMAR】 com o jurídico e o DPO.

**Controlador:** 【CONFIRMAR: razão social e CNPJ do controlador FTW】
**Encarregado (DPO):** 【CONFIRMAR: nome e e-mail】
**Operador principal:** NeoAssist (plataforma de atendimento omnichannel)
**Canal:** WhatsApp consumidor FTW (11) 2388-3360

| Finalidade | Categorias de dados | Titulares | Base legal | Operadores e compartilhamento | Retenção | Segurança |
|---|---|---|---|---|---|---|
| Atender e resolver a solicitação do cliente | Nome, telefone, conteúdo da conversa, dados do pedido, CPF quando informado | Consumidores da FTW | Execução de contrato (art. 7, V) e legítimo interesse (art. 7, IX) | NeoAssist; 【CONFIRMAR: Tray, ERP, transportadoras】 | 【CONFIRMAR】 | Controle de acesso, plataforma com segurança, registro de protocolos |
| Registrar histórico e medir qualidade do atendimento | Protocolo, histórico, classificação, conteúdo | Consumidores da FTW | Legítimo interesse (art. 7, IX), com teste de proporcionalidade (LIA) | NeoAssist | 【CONFIRMAR】 | Idem acima |
| Cumprir obrigações legais e de defesa do consumidor | Dados do pedido e do atendimento | Consumidores da FTW | Obrigação legal (art. 7, II) | 【CONFIRMAR】 | Prazo legal aplicável | Idem acima |
| Enviar marketing pelo WhatsApp | Nome, telefone, registro do opt-in | Consumidores que deram opt-in | Consentimento (art. 7, I) | NeoAssist; 【CONFIRMAR: base de marketing】 | Enquanto ativo o consentimento, mais prazo de prova | Idem acima, com registro do aceite |
| Atender pedidos de direitos do titular | Dados de identificação e do pedido de direito | Titulares que exercem direitos | Obrigação legal (art. 7, II) e cumprimento da LGPD | Encarregado; NeoAssist | Prazo de comprovação | Verificação de identidade |

Observação sobre legítimo interesse: onde a base for legítimo interesse, a ANPD orienta que se documente um teste de proporcionalidade (a avaliação de legítimo interesse, ou LIA), demonstrando a finalidade legítima, a necessidade e o balanceamento com os direitos do titular, além de garantir o direito de oposição. 【CONFIRMAR com o jurídico: elaborar e arquivar a LIA das finalidades apoiadas em legítimo interesse.】

---

## Parte C: Direitos do titular na Núb.ia

### C.1. Os direitos que precisam ser atendidos

A LGPD (art. 18 e seguintes) garante ao titular: confirmação da existência de tratamento; acesso aos dados; correção de dados incompletos, inexatos ou desatualizados; anonimização, bloqueio ou eliminação de dados desnecessários, excessivos ou tratados em desconformidade; portabilidade; eliminação dos dados tratados com base no consentimento; informação sobre compartilhamento; informação sobre a possibilidade de não consentir e as consequências; e revogação do consentimento.

### C.2. Canal e prazos

- **Canal de exercício:** e-mail do encarregado 【CONFIRMAR: e-mail】 e o próprio WhatsApp de atendimento, que reconhece o pedido e encaminha.
- **Verificação de identidade:** confirmar a identidade do titular antes de atender, para evitar entregar dados a terceiros.
- **Prazos:** confirmação e acesso podem ter resposta imediata em formato simplificado, ou resposta completa em até 15 dias. Os demais pedidos seguem os prazos aplicáveis. 【CONFIRMAR: a ANPD está finalizando regulamento específico de direitos do titular, que pode ajustar prazos e forma; revisar quando publicado.】

### C.3. Roteiro da Núb.ia quando o cliente pede algo de privacidade

A IA não executa exclusão, correção ou acesso por conta própria. Ela reconhece, registra e encaminha para o encarregado. Roteiro:

1. **Reconhecer o gatilho:** frases como quero excluir meus dados, apagar meu cadastro, quais dados vocês têm sobre mim, quero corrigir meus dados, não quero mais receber mensagens, quero revogar meu consentimento.
2. **Responder com clareza e sem prometer prazo que não é o legal:** confirmar que o pedido de privacidade foi registrado e será tratado pelo encarregado dentro dos prazos da LGPD.
3. **Classificar o protocolo:** abrir protocolo e aplicar a tag de assunto de privacidade e proteção de dados (LGPD) na árvore de classificação, para o pedido aparecer nos relatórios e não se perder.
4. **Transbordar para a fila certa:** encaminhar ao encarregado ou à fila responsável por privacidade. 【LACUNA DE WORKFLOW: definir se o pedido de direito vai para uma fila própria de privacidade ou para a fila geral com a tag; e como o encarregado recebe e responde.】
5. **Casos especiais:** pedido de revogação de consentimento de marketing pode ser resolvido de imediato pela atualização do opt-in (seção A.4), sempre com registro.

### C.4. Texto pronto para a Núb.ia usar

> Entendi, você quer tratar de um assunto sobre os seus dados pessoais. Já registrei o seu pedido em um protocolo e encaminhei para a nossa equipe responsável por proteção de dados, que vai cuidar disso dentro dos prazos previstos na lei. Se puder, me confirme o seu nome completo para a gente localizar o seu cadastro com segurança.

### C.5. Tag de classificação

Para o relatório gerencial enxergar esses acionamentos, criar na árvore de classificação do WhatsApp uma entrada dedicada, por exemplo Privacidade e proteção de dados (LGPD), com motivos como pedido de acesso, correção, exclusão e revogação de consentimento. 【CONFIRMAR: incluir essa área ou motivo na árvore de classificação do canal e no blueprint de relatórios.】

---

## Próximos passos sugeridos

1. Jurídico e DPO validam a base legal, os prazos de retenção e o texto do Aviso de Privacidade.
2. Mkt e jurídico definem e publicam a URL do Aviso, que substitui o marcador [LINK_CONSENTIMENTO].
3. CX implementa no fluxo a mensagem de transparência e, se aprovado, o opt-in de marketing.
4. NeoAssist estrutura o registro do opt-in e a tag de privacidade na árvore de classificação.
5. Elaborar e arquivar a LIA das finalidades de legítimo interesse.
