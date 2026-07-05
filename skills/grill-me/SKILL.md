---
name: grill-me
description: 'Use no inicio de uma feature ou mudanca de escopo incerto para DESCOBRIR e AFIAR os requisitos antes de escrever qualquer codigo (Fase 0 do Agent-Driven Protocol). Entrevista implacavel que desafia cada suposicao, abre opcoes de abordagem (via web-buscador) e explora o codebase, ate escopo e criterios de sucesso ficarem claros. Casos-gatilho: "quero adicionar/construir X", "nao sei bem o que preciso", "vamos comecar essa feature", "levanta os requisitos", "grill", "descobrir o escopo", "isso ainda ta vago". Nao use em mudanca pequena, direta e ja compreendida (va de plan mode). Termina num brief afiado que alimenta o to-prd.'
version: 0.1.0
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
1. **Explorar o terreno primeiro (via subagente, caminho absoluto).** Antes de perguntar, despache UM subagente pra mapear o codebase do ALVO: `feature-dev:code-explorer` quando o projeto tem estrutura de app; fallback para um Explore com Glob/Read quando nao se aplica (ex: site estatico vanilla). NUNCA explore inline com ls/find no thread principal, e nunca repita um comando que falhou. No mesmo passo, cheque **reuso interno**: memoria da sessao, CLAUDE.md dos projetos e Glob bounded em `C:\dev\*` — muita "feature nova" ja existe como modulo pronto (ex: um modulo de pagamento seu, pronto em outro repo). Cite arquivos.
2. **Grelhar.** Perguntas dirigidas, uma por vez: problema real, quem usa, criterios de sucesso, casos de borda, restricoes (stack, prazo, dado sensivel, LGPD). Registre as respostas.
3. **Abrir opcoes (a criatividade entra aqui).** Antes de fixar o caminho, chame a skill `web-buscador` no modo criativo (ela vem no ADP Protocolo Completo; sem ela, pesquise na web e nas docs normalmente): 2-4 abordagens (stacks, libs, MCPs, plugins, padroes atuais) com trade-offs. Ela ja cruza com o que voce ja tem (skills, MCPs) e traz o que ha de novo. Nao fixe o caminho sem ter aberto as opcoes.
4. **Afiar o dominio.** Conforme as decisoes cristalizam, registre os termos-chave (glossario leve) e as decisoes importantes com o porque (ADR embrionaria). Isso vira materia-prima do PRD e evita re-discussao. (inspirado no domain-modeling do Matt)
5. **Convergir.** Proponha a abordagem recomendada com a justificativa + as alternativas como plano B.

## Gate humano (escopo)
Este e o primeiro gate do ADP. NAO passe para o `to-prd` sem você confirmar, explicitamente: (a) o problema, (b) os criterios de sucesso, (c) a abordagem escolhida, (d) o que fica FORA de escopo. Use AskUserQuestion para fechar o gate. Decisao dificil = pergunte, nao adivinhe. Se você rejeitar qualquer item, volte ao ponto correspondente do loop do grill; nao avance. Gate aprovado -> grave `status: aprovado` no frontmatter do brief (ate la, `status: rascunho`) — e o que permite ao `/adp` retomar o pipeline entre sessoes.

## Saida: o brief afiado
Entregue um brief estruturado (nao codigo), que e o input do `to-prd`:
- **Problema** em 1-2 frases + quem sofre com ele.
- **Criterios de sucesso** testaveis.
- **Abordagem escolhida** (+ alternativas descartadas e por que).
- **Decisoes e termos** (glossario leve + decisoes com o porque).
- **Restricoes** (stack, prazo, seguranca/LGPD).
- **Riscos e perguntas em aberto** (o que a `web-buscador` marcou como "nao consta").
- **Fora de escopo** (a fronteira explicita).

Persista o brief como artefato nomeado em `docs/adp/<slug>-brief.md` no projeto (versionavel no repo). Esse arquivo e o input unico do `to-prd`, e o que fica registrado no gate de escopo. Convencao de artefatos do ADP: `docs/adp/<slug>-brief.md` -> `docs/adp/<slug>-prd.md` -> issues.

## Anti-padroes
- Aceitar resposta vaga e seguir mesmo assim.
- Pular a exploracao do codebase e perguntar no vacuo.
- Fixar a abordagem sem abrir opcoes (mata a criatividade do fluxo).
- Grelhar uma mudanca trivial (era plan mode).
- Passar do gate sem confirmar a fronteira do escopo.
