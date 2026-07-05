---
name: grill-me
description: 'Use no inicio de uma feature ou mudanca de escopo incerto para DESCOBRIR e AFIAR os requisitos antes de escrever qualquer codigo (Fase 0 do Agent-Driven Protocol). Entrevista implacavel que desafia cada suposicao, levanta o estado da arte online ANTES do workflow (stacks, ideias, comunidades, concorrentes — via web-buscador), abre opcoes de abordagem, explora o codebase e FUNDE tudo isso com o escopo pedido e as respostas do usuario, ate escopo e criterios de sucesso ficarem claros. Casos-gatilho: "quero adicionar/construir X", "nao sei bem o que preciso", "vamos comecar essa feature", "levanta os requisitos", "grill", "descobrir o escopo", "isso ainda ta vago", "como o mundo resolve isso hoje?". Nao use em mudanca pequena, direta e ja compreendida (va de plan mode). Termina num brief afiado que alimenta o to-prd.'
version: 0.3.0
---

# grill-me

O maior modo de falha no desenvolvimento com agente e o desalinhamento: voce acha que pediu uma coisa, o agente entendeu outra, e isso so aparece no codigo pronto. O `grill-me` mata o desalinhamento na origem: uma entrevista implacavel que forca entendimento compartilhado ANTES de qualquer linha de codigo. E a Fase 0 do ADP, disparada pelo `/adp-spec` ou avulsa.

Reescrita da ideia do `grill-me` do Matt Pocock (mattpocock/skills), adaptada ao ADP com abertura criativa e integracao ao fluxo do protocolo.

**SUB-SKILL RECOMENDADA (opcional — sem ela, siga o processo descrito nesta própria skill):** `superpowers:brainstorming` e o motor de descoberta (uma pergunta por vez, propor abordagens, HARD-GATE: nada de implementar sem design aprovado). Esta skill adiciona por cima a POSTURA de grill, a abertura criativa e a saida estruturada do ADP.

## Quando usar e quando NAO usar
- **Usar:** feature nova, escopo grande, requisito ainda vago, mudanca que cruza varias areas.
- **Nao usar:** mudanca pequena, direta e ja compreendida. Nesse caso, va de plan mode direto (regra spec-driven vs plan mode da aula). Nao force spec no trivial.

## Postura: grill, nao brainstorm gentil
A diferenca do brainstorming puro esta na postura. Aqui:
- **Uma pergunta por vez.** Nunca despeje um questionario. Cada resposta abre a proxima pergunta.
- **Nao aceite resposta vaga.** "Sei la", "tipo um dashboard", "o de sempre" nao passam: puxe o "por que" ate o requisito ficar concreto e testavel.
- **Desafie cada suposicao,** inclusive as do próprio usuário. Se um pedido colide com o que ja existe no codebase ou com uma restricao real, aponte antes de seguir.
- **Prefira multipla escolha** quando der: e mais facil de responder e expoe as opcoes.
- **Pare quando afiado, nao antes.** Escopo, criterios de sucesso e fronteira (o que fica de fora) precisam estar claros.

## O loop do grill
1. **Dupla exploracao em paralelo (2 subagentes de uma vez, ANTES de perguntar).** Despache no MESMO turno:
   - **Terreno interno:** UM subagente pra mapear o codebase do ALVO (caminho absoluto): `feature-dev:code-explorer` quando o projeto tem estrutura de app; fallback para um Explore com Glob/Read quando nao se aplica (ex: site estatico vanilla). NUNCA explore inline com ls/find no thread principal, e nunca repita um comando que falhou. No mesmo passo, cheque **reuso interno**: memoria da sessao, CLAUDE.md dos projetos e Glob bounded em `C:\dev\*` — muita "feature nova" ja existe como modulo pronto (ex: um modulo de pagamento seu, pronto em outro repo). Cite arquivos.
   - **Radar largo (estado da arte, semeado so pelo prompt inicial):** UM subagente com a `web-buscador` no playbook "Radar estado-da-arte", passada larga: como o mundo resolve isso HOJE — abordagens dominantes e emergentes, stack de referencia (versoes atuais, o que a comunidade adotou/abandonou), templates e repos exemplares, concorrentes lancados, dores relatadas. Cobre o radar tecnico E o de comunidades (Reddit, dev.to, Product Hunt, TabNews, HN, GitHub).
   Voce entra na entrevista JA sabendo o terreno interno e o estado da arte — as perguntas nascem melhores.
2. **Grelhar (informado pelo radar).** Perguntas dirigidas, uma por vez: problema real, quem usa, criterios de sucesso, casos de borda, restricoes (stack, prazo, dado sensivel, LGPD). Use os achados como MUNICAO de pergunta, nao como palestra: "a maioria resolve isso com X; teu caso tem motivo pra diferir?", "o produto Y ja faz isso; o que te diferencia?", "a comunidade abandonou Z por causa de W; ainda quer seguir por ai?". Registre as respostas.
3. **Abrir opcoes (a criatividade entra aqui).** Antes de fixar o caminho, monte 2-4 abordagens (stacks, libs, MCPs, plugins, padroes atuais) com trade-offs, cruzando o radar largo com o que a sua equipe ja tem (skills, MCPs). Se as respostas do grill mudaram a direcao (nicho diferente, restricao nova), dispare uma **segunda passada FINA** da `web-buscador`, agora mirada na direcao escolhida — curta e cirurgica, nada de repetir a larga. Nao fixe o caminho sem ter aberto as opcoes.
4. **Fundir (achado vira decisao, nunca trivia).** Cruze CADA achado relevante do radar com o escopo pedido no prompt E com as respostas do brainstorm, e de a ele um unico destino (tabela na secao Fusao). Achado sem destino = corta. E aqui que o estado da arte vira escopo, e nao curiosidade.
5. **Afiar o dominio.** Conforme as decisoes cristalizam, registre os termos-chave (glossario leve) e as decisoes importantes com o porque (ADR embrionaria). Isso vira materia-prima do PRD e evita re-discussao. (inspirado no domain-modeling do Matt)
6. **Convergir.** Proponha a abordagem recomendada com a justificativa + as alternativas como plano B.

## Ritmo do radar: antecipado ou intercalado
Escolha o ritmo pelo grau de vagueza do prompt inicial:
- **Antecipado (padrao):** prompt razoavelmente claro -> radar largo completo em paralelo com o codebase (passo 1), grill depois, passada fina se a direcao mudar. Um ciclo, barato.
- **Intercalado (prompt muito vago ou area desconhecida):** alterne busca e pergunta em ciclos curtos de calibracao:
  1. **Sonda:** 1 fetch por fonte (um GitHub, um Reddit/HN, um Product Hunt, um dev.to/TabNews) — visao de 5 minutos, nao o radar inteiro.
  2. **Calibra:** 2-3 perguntas de grill usando a sonda como municao; as respostas estreitam a area.
  3. **Aprofunda:** proxima rodada de buscas JA mirada no que as respostas revelaram (agora sim top 3 por fonte, versoes, concorrentes).
  4. **Fecha:** ultima rodada de perguntas pra cravar escopo e fronteira. Repita 2-3 no maximo mais uma vez; mais que isso e enrolacao.
  O intercalado gasta menos busca no alvo errado: nao faca radar profundo de e-commerce se a terceira resposta revela que o problema real e logistica.

## Fusao: os 5 destinos de um achado
Todo achado do radar (larga ou fina) recebe UM destino — e o passo que transforma pesquisa em escopo:

| Destino | Quando | Vira o que |
|---|---|---|
| **Pergunta de grill** | o achado expoe uma decisao que so o usuario pode tomar | pergunta dirigida no passo 2 (com a fonte citada) |
| **Opcao de abordagem** | rota tecnica viavel pro escopo pedido | entrada nas 2-4 opcoes do passo 3, com trade-off |
| **Mesa (table stakes)** | todo produto da area ja faz; o usuario vai ESPERAR que exista | proposta de inclusao no escopo (ou corte explicito e consciente) |
| **Anti-escopo** | a comunidade tentou, abandonou ou considera armadilha | proposta de "fora de escopo" com o porque documentado |
| **Risco** | contradiz uma suposicao do prompt/respostas e nao da pra resolver agora | linha em "Riscos e perguntas em aberto" do brief |

Regras da fusao:
- Cada destino cita a fonte (link) — decisao rastreavel ate o achado.
- Achado que colide com resposta do usuario NAO decide sozinho: vira pergunta ("voce disse A, mas a comunidade converge pra B por causa de C — mantem A?"). O usuario e o dono do escopo; o radar e conselheiro.
- No maximo ~7 achados fundidos; o resto e ruido, corta. Radar bom termina magro.

## Gate humano (escopo)
Este e o primeiro gate do ADP. NAO passe para o `to-prd` sem você confirmar, explicitamente: (a) o problema, (b) os criterios de sucesso, (c) a abordagem escolhida, (d) o que fica FORA de escopo. Use AskUserQuestion para fechar o gate. Decisao dificil = pergunte, nao adivinhe. Se você rejeitar qualquer item, volte ao ponto correspondente do loop do grill; nao avance. Gate aprovado -> grave `status: aprovado` no frontmatter do brief (ate la, `status: rascunho`) — e o que permite ao `/adp` retomar o pipeline entre sessoes.

## Saida: o brief afiado
Entregue um brief estruturado (nao codigo), que e o input do `to-prd`:
- **Problema** em 1-2 frases + quem sofre com ele.
- **Criterios de sucesso** testaveis.
- **Abordagem escolhida** (+ alternativas descartadas e por que).
- **Decisoes e termos** (glossario leve + decisoes com o porque).
- **Restricoes** (stack, prazo, seguranca/LGPD).
- **Estado da arte** (a tabela de fusao: cada achado do radar com fonte + destino — pergunta, opcao, mesa, anti-escopo ou risco. So os que influenciaram decisao; maximo ~7).
- **Riscos e perguntas em aberto** (o que a `web-buscador` marcou como "nao consta").
- **Fora de escopo** (a fronteira explicita).

Persista o brief como artefato nomeado em `docs/adp/<slug>-brief.md` no projeto (versionavel no repo). Esse arquivo e o input unico do `to-prd`, e o que fica registrado no gate de escopo. Convencao de artefatos do ADP: `docs/adp/<slug>-brief.md` -> `docs/adp/<slug>-prd.md` -> issues.

## Anti-padroes
- Aceitar resposta vaga e seguir mesmo assim.
- Pular a exploracao do codebase e perguntar no vacuo.
- Fixar a abordagem sem abrir opcoes (mata a criatividade do fluxo).
- Grelhar uma mudanca trivial (era plan mode).
- Passar do gate sem confirmar a fronteira do escopo.
- Despejar os achados do radar no usuario como palestra (achado vira pergunta, opcao ou fronteira — ou e cortado).
- Rodar o radar DEPOIS de ja ter fixado a abordagem (radar e insumo da decisao, nao decoracao do brief).
- Deixar o estado da arte decidir sozinho por cima de uma resposta do usuario (o radar aconselha; o dono do escopo e o usuario).
- Radar profundo no alvo errado: prompt muito vago pede o ritmo intercalado (sonda -> calibra -> aprofunda), nao um mergulho cego.
