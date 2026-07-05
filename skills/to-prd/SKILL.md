---
name: to-prd
description: 'Use depois do grill-me, quando ja existe entendimento compartilhado e falta transformar em documento (Fase 1 do Agent-Driven Protocol). SEM entrevista nova: sintetiza o brief afiado + o codebase num PRD com user stories, decisoes de arquitetura e de teste. Casos-gatilho: "vira isso num PRD", "escreve a spec", "documenta o que a gente decidiu", "monta o documento da feature", "to-prd". Termina no gate de PRD aprovado, salvo em docs/adp/<slug>-prd.md, pronto para o to-issues.'
version: 0.1.0
---

# to-prd

Fase 1 do ADP (Especificar). Pega o brief afiado da `grill-me` e o entendimento do codebase e produz um PRD claro: o contrato que o `to-issues` fatia e o `tech-lead` executa. Reescrita da `to-prd` do Matt Pocock, adaptada ao ADP.

**SUB-SKILLS RECOMENDADAS (opcionais — sem elas, siga o processo descrito nesta própria skill):** `superpowers:writing-plans` (use o RIGOR e o zero-ambiguidade dele, NAO o seu formato-com-paths: o nivel de detalhe aqui e o da secao "Nivel de detalhe" abaixo) + `feature-dev:code-architect` (blueprint arquitetural: quais modulos criar/mudar, interfaces, fluxo de dados, sequencia). Esta skill orquestra os dois no formato PRD do ADP.

## Input
O brief afiado em `docs/adp/<slug>-brief.md` (saida da `grill-me`). Se ele nao existir, rode a `grill-me` antes: nao pule a Fase 0. O PRD sintetiza o brief, nao o substitui.

## Regra de ouro: sintetizar, nao re-entrevistar
A `grill-me` ja grelhou você. Aqui e sintese do que ja foi decidido, nao uma nova rodada de perguntas. Se ao escrever aparecer um buraco REAL (uma decisao que nunca foi tomada), volte pontualmente a grelhar so aquele ponto, nao refaca a descoberta inteira.

## Processo
1. **Ancorar no codebase.** Despache o `feature-dev:code-architect` para mapear o estado real e desenhar onde a feature encaixa. Se ele nao se aplicar ou falhar (projeto estatico/estrutura atipica), NAO re-tente igual: mapeie via subagente Explore com Glob/Read (caminho absoluto) e siga. Use o glossario e as decisoes do brief. Respeite as convencoes `backend-layers` / `frontend-react` (ou a variante `vanilla-static`) e ADRs da area tocada.
2. **Definir os seams (costuras de teste).** Onde a feature sera testada? Prefira seams existentes; use o seam mais ALTO possivel; minimize o numero (o ideal e um). Confirme com o usuário que os seams batem com a expectativa. Isso ja prepara a Fase 4 (QA) e evita teste acoplado a implementacao.
3. **Escrever o PRD** com o template abaixo, em `docs/adp/<slug>-prd.md`.
4. **Gate humano (PRD).**

## Template do PRD
### Problema
O problema, do ponto de vista de quem sofre com ele.
### Solucao
A solucao, do ponto de vista do usuario.
### User Stories
Lista LONGA e numerada, formato "Como <papel>, quero <acao>, para <beneficio>". Extensiva: cobre todos os aspectos da feature, inclusive erro e borda.
### Criterios de aceitacao
Criterios testaveis por feature, herdados e refinados dos criterios de sucesso do brief e ligados aos seams. Sao exatamente o que a Fase 4 (qa) vai verificar: mantem a rastreabilidade brief -> prd -> qa.
### Decisoes de arquitetura
Modulos a criar/modificar, interfaces/portas, camadas tocadas (nomeando router/controller/service/repository ou api/hook/container/presentational conforme a convencao), mudancas de schema, contratos de API, interacoes-chave, e as dependencias grosseiras entre modulos/stories (para o to-issues ordenar o fatiamento). **Sem caminhos de arquivo exatos nem code snippets** — envelhecem rapido; os paths exatos vao nas issues e no `tech-lead`. Excecao: um snippet que codifica melhor uma decisao que a prosa (state machine, schema, tipo) pode ser embutido, com nota.
### Novas adocoes (ferramentas / libs / MCPs / plugins / stack novos)
Liste TODA ferramenta, biblioteca, MCP, plugin, stack ou tecnica NOVA que este PRD adota (vinda da `web-buscador` no grill-me), cada uma com: o que e, por que ganhou (o trade-off decisivo), a VERSAO alvo e a fonte. Isso mantem a criatividade do fluxo rastreavel do grill ao codigo e avisa o `tech-lead` a consultar a doc atual antes de implementar. Deixe vazio se a feature so usa o que o projeto ja tem.
### Decisoes de teste
O que e um bom teste aqui (so comportamento externo, nunca detalhe de implementacao); quais modulos serao testados; os seams definidos no passo 2; prior art (testes parecidos que ja existem no codebase).
### Fora de escopo
A fronteira explicita (herdada e refinada do brief).
### Riscos e questoes em aberto
Herdados do brief (inclusive o que a web-buscador marcou como "nao consta"), para nao perder informacao entre fases.
### Notas
Qualquer coisa relevante que nao coube acima.

## Nivel de detalhe: PRD x plano de execucao
O PRD e nivel-modulo e nivel-decisao (o QUE e o PORQUE). Os caminhos exatos, a ordem de arquivos e o codigo passo a passo sao do `to-issues` (que fatia) e do `tech-lead` (que executa via `superpowers:writing-plans`). Nao antecipe paths no PRD; nao deixe decisao arquitetural de fora dele.

## Saida
O PRD completo em `docs/adp/<slug>-prd.md`, com TODAS as secoes do template preenchidas e frontmatter `status: rascunho` (vira `aprovado` quando o gate 2 passar — retomada do `/adp`). E o input unico do `to-issues` (que pode assumir que cada secao existe).

## Gate humano (PRD)
Segundo gate do ADP. Nao siga para o `to-issues` sem você aprovar o PRD. Use AskUserQuestion se houver escolha em aberto. Rejeitou parte? Ajuste o PRD; se for buraco de descoberta, volte pontualmente a `grill-me`.

## Anti-padroes
- Re-entrevistar o usuario (o grill ja fez).
- PRD recheado de paths/snippets que envelhecem.
- User stories rasas que nao cobrem erro e borda.
- Pular os seams (a Fase 4 sofre depois).
- PRD que ignora as convencoes de camada do projeto.
