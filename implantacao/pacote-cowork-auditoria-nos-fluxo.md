# Pacote Cowork — Auditar os nós nativos do NeoAssist e reconstruir o fluxo com eles

> **Para o Claude Cowork** (roda no ambiente do usuário, com acesso ao navegador/Chrome, à plataforma NeoAssist já aberta e logada, e ao guia oficial `https://neoassist.getoutline.com/s/6acb61f6-9250-406a-a50f-2d74e0011738/doc/fluxo-de-automacoes-4ogIYYglMW`, que exige login e por isso o Claude Code não consegue ler). Repositório de referência: `rrfernandes01-ops/fluxosneoassist` (branch `claude/fitoway-whatsapp-ai-x5uzrt`).
>
> **Motivo desta rodada**: a documentação atual do fluxo mestre (`implantacao/passo-2-fluxo-mestre/construcao-no-a-no.md`) foi escrita em prosa lógica (nós N01–N22, mensagens e condições) pensada para ser configurada manualmente, mas **pode não estar usando todos os recursos nativos do construtor de fluxo do NeoAssist**. O Head já identificou pelo menos estes recursos nativos que a documentação atual não referencia explicitamente:
> - **Entrada dinâmica** com **validação de CPF/CNPJ** embutida, que transforma o valor digitado direto em variável.
> - Uma ação/nó que **salva variável** (a confirmar o nome e comportamento exatos).
> - Um nó de **vincular consumidor** (associa a conversa a um cadastro existente).
> - Um nó de **atribuir categoria** (marca a conversa/contato com uma categoria).
>
> O objetivo desta rodada é **descobrir todos os nós disponíveis**, mapear cada um contra o que a documentação hoje descreve de forma genérica, e **reconstruir o fluxo usando os nós nativos** onde isso for mais robusto do que a lógica manual atual — sem perder nenhuma regra já validada (Regra Anti-Espera, Silêncio Zero, N22, transbordo em 3 partes, regra de filas 611808).

## 0. Cautela antes de mexer em produção

O Head reportou insatisfação com os testes em produção. **Antes de alterar o fluxo publicado**:
- Verificar se o NeoAssist permite **duplicar/clonar o fluxo** para um ambiente de rascunho/teste. Se permitir, fazer todas as mudanças estruturais nessa cópia primeiro, validar, e só then publicar substituindo o fluxo em produção.
- Se não houver como duplicar, ao menos **exportar/documentar o estado atual do fluxo publicado** (prints ou export, se disponível) antes de editar, para haver como reverter.

## 1. Inventariar os nós disponíveis (descoberta completa)

Abrir o construtor de fluxo de automações (Automações → novo fluxo ou editar o fluxo existente do canal WhatsApp) e listar **todos** os tipos de nó/bloco disponíveis na paleta. Consultar também o guia oficial linkado acima (a sessão do Cowork está autenticada e consegue abrir). Para cada tipo de nó, documentar:
- Nome exato do nó como aparece na plataforma.
- O que ele faz.
- Campos de configuração disponíveis.
- Se ele **gera uma variável** (e como essa variável é referenciada depois no fluxo) ou **consome** uma variável existente.
- Se tem **validação embutida** (formato, regex, tipo de dado — ex.: CPF, CNPJ, e-mail, telefone).

Nós a confirmar com prioridade (citados pelo Head como possivelmente subutilizados):
1. **Entrada dinâmica de dados com validação de CPF/CNPJ** → transforma o valor digitado em variável validada.
2. **Salvar variável** (ação/nó dedicado, fora do próprio nó de entrada).
3. **Vincular consumidor** — associar a conversa a um cadastro existente no NeoAssist (isso substituiria a lógica de `[[INT_CONSUMIDOR]]` do nosso N04 por um recurso nativo?).
4. **Atribuir categoria** — marcar a conversa/contato com uma categoria (isso substituiria o nosso N18 "gravar `{{perfil}}` no cadastro" por um recurso nativo?).
5. Qualquer nó de **condição/branch**, **menu interativo (lista)**, **atraso/delay**, **chamada de integração/webhook**, **transferência para fila humana**, **encerramento de conversa** — documentar todos, mesmo que já estejamos usando algo parecido.

## 2. Cruzar com a documentação atual (gap analysis)

Usando a tabela de nós do passo 1, revisar `implantacao/passo-2-fluxo-mestre/construcao-no-a-no.md` nó a nó (N01 a N22) e, para cada um, responder:
- Qual nó nativo do NeoAssist implementa esse passo hoje?
- Estamos usando o recurso nativo mais adequado, ou fizemos uma gambiarra manual onde existia um nó pronto?

Atenção especial a estes pontos, que o Head suspeita estarem subotimizados:
- **N10/N11 (coleta de nome e CPF)**: hoje descrito como "mensagem + entrada" com validação manual de formato. Se existe um nó nativo de **entrada dinâmica com validação de CPF/CNPJ**, ele deve substituir essa lógica manual — a validação e a geração da variável `{{documento}}` ficam a cargo do nó nativo, não de instrução de prompt.
- **N04 (integração `[[INT_CONSUMIDOR]]`)**: se existe um nó nativo de **vincular consumidor**, avaliar se ele resolve parte ou todo esse passo sem depender de uma integração customizada.
- **N18 (gravar `{{perfil}}` no cadastro)**: se existe um nó nativo de **atribuir categoria**, usá-lo aqui em vez de uma ação genérica "gravar campo customizado".
- Qualquer variável usada no fluxo (`{{nome}}`, `{{documento}}`, `{{perfil}}`, `{{protocolo_recente}}`, `{{assunto_recente}}`, `{{consentimento_ok}}`, `{{identidade_validada}}`) deve ser conferida contra o mecanismo real de variáveis da plataforma (como é declarada, como é lida por outros nós e pelos agentes Núb.ia Resolve).

Produzir uma tabela: **nó do nosso doc → nó(s) nativo(s) do NeoAssist → gap encontrado → ação recomendada**.

## 3. Reconstruir o fluxo com os nós nativos

Com o mapeamento do passo 2, reconstruir o fluxo (na cópia de teste, se houver — ver seção 0) usando os nós nativos onde for mais robusto, mantendo **todas** as regras já validadas:
- Regra Anti-Espera e Silêncio Zero (timeouts, ramos de erro).
- **N22** — reinício de triagem para nunca deixar o cliente sem resposta ao mudar de assunto.
- Mensagem de transbordo em 3 partes (reconhecimento, ação, contexto), sempre explícita.
- Transbordo apenas para filas filhas de **611808 FTW > Whatsapp**.
- Identificação persistente (`{{identidade_validada}}`) — não repetir identificação já validada.

## 4. Testar ponta a ponta na cópia de teste

Antes de publicar em produção, rodar pelo menos:
- Cliente novo (sem cadastro) → coleta de nome e CPF pelo nó de entrada dinâmica → variável validada corretamente → cadastro vinculado.
- Cliente recorrente (já identificado) → nó de vincular consumidor traz os dados sem repetir pergunta.
- Roteamento de perfil → nó de atribuir categoria grava corretamente e é lido pelos agentes.
- Mudança de assunto (N22) e transbordo (mensagem em 3 partes, fila correta) continuam funcionando depois da reconstrução.

## 5. Reportar ao Claude Code / Head

Ao final, reportar em detalhe (para o Claude Code atualizar `construcao-no-a-no.md` como fonte da verdade):
- **Inventário completo dos nós** encontrados (nome, função, configuração, variáveis).
- A **tabela de mapeamento** do passo 2 (nosso nó → nó nativo → gap → ação).
- O que foi **efetivamente reconstruído** nesta rodada e o que ficou pendente.
- Resultado dos testes do passo 4.
- Se foi possível duplicar o fluxo para teste ou se a alteração foi direto em produção (e por quê).
