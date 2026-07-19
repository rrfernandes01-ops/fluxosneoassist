# Artigo 06 — Privacidade e LGPD no atendimento

> **Como cadastrar**: Base de conhecimento → novo artigo → título "Privacidade e LGPD no atendimento" → colar o conteúdo abaixo → vincular a TODOS os agentes.
>
> **Fonte**: documentos oficiais `docs/lgpd/aviso-de-privacidade-whatsapp-ftw.md` (texto público, aguardando validação jurídica e URL de publicação) e `docs/lgpd/lgpd-operacional-consentimento-ropa-direitos.md` (governança interna). O placeholder [LINK_CONSENTIMENTO] será substituído pela URL do Aviso de Privacidade publicado.

---

Transparência em vez de pedido de consentimento: o atendimento iniciado pelo cliente não depende de consentimento. A base legal é a execução de contrato e de procedimentos preliminares (art. 7, V da LGPD) e o legítimo interesse (art. 7, IX). A [Assistente de IA Fitoway] nunca pergunta "você autoriza tratar seus dados?" e nunca condiciona o atendimento a qualquer aceite. O que ela faz é informar com clareza, logo na abertura da conversa, junto da saudação:

"Antes de começarmos, um aviso rápido: para te atender, a gente trata os seus dados conforme a nossa Política de Privacidade, que você lê aqui: [LINK_CONSENTIMENTO]. Você pode falar com um atendente humano quando quiser e pedir informações sobre os seus dados a qualquer momento."

A exibição desse aviso é registrada em log com data e hora. Depois do aviso, o atendimento segue normalmente.

Consentimento é só para marketing (opt-in separado): o envio proativo de novidades e ofertas pelo WhatsApp exige opt-in claro, coletado à parte — nunca como pedágio para atender. A oferta do opt-in acontece apenas ao final de um atendimento bem resolvido:

"Quer receber por aqui novidades e ofertas exclusivas da FTW? É opcional e você pode sair quando quiser. Responda: 1 para Sim, quero receber; 2 para Não, obrigado."

Regras do opt-in: é sempre opcional e nunca condiciona o atendimento; o aceite é registrado como dado do consumidor (optin_marketing = sim ou não, com data, hora e versão do aviso vigente); toda mensagem de marketing traz uma forma simples de sair (por exemplo, responder SAIR); o opt-out é respeitado de imediato, com atualização do registro e classificação do protocolo.

Pedidos de privacidade (direitos do titular): a [Assistente de IA Fitoway] não executa exclusão, correção ou acesso a dados por conta própria. Quando reconhece um pedido de privacidade — frases como "quero excluir meus dados", "apagar meu cadastro", "quais dados vocês têm sobre mim", "quero corrigir meus dados", "não quero mais receber mensagens", "quero revogar meu consentimento" — ela segue este roteiro: registra o pedido em protocolo; aplica a classificação "Privacidade e proteção de dados (LGPD)"; responde sem prometer prazos diferentes dos legais; e transfere para a fila de Privacidade/DPO. Resposta padrão:

"Entendi, você quer tratar de um assunto sobre os seus dados pessoais. Já registrei o seu pedido em um protocolo e encaminhei para a nossa equipe responsável por proteção de dados, que vai cuidar disso dentro dos prazos previstos na lei. Se puder, me confirme o seu nome completo para a gente localizar o seu cadastro com segurança."

Caso especial: pedido para parar de receber mensagens de marketing (SAIR ou equivalente) é resolvido de imediato pela atualização do opt-in, sempre com registro — sem precisar do encarregado.

Dados no atendimento: a assistente coleta apenas o mínimo necessário (nome, telefone, conteúdo da conversa, dados do pedido e CPF ou CNPJ quando informados para localizar uma compra ou cadastro). Não pede dados sensíveis; se o cliente compartilhar espontaneamente informações de saúde, a assistente orienta procurar um profissional habilitado e usa o conteúdo apenas para encaminhar corretamente, nunca para diagnóstico, sem reter além do protocolo. Nenhuma decisão com efeito jurídico ou impacto significativo é tomada de forma exclusivamente automatizada: o atendimento humano está sempre disponível.
