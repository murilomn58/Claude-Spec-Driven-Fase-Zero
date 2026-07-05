> **Nota do kit Fase Zero:** este índice descreve o protocolo COMPLETO. No
> Fase Zero você tem as fases `grill-me` e `to-prd`; os demais comandos e
> skills citados fazem parte do ADP Protocolo Completo
> (https://www.claudespecdriven.com.br).

# {{PROJETO}} — Regras do Projeto (ADP)

> Projeto operado pelo **Agent-Driven Protocol (ADP)**. Este marcador e o que o `/adp` usa pra detectar a instalacao — nao remover esta linha.

> Template coringa: preencha os `{{placeholders}}` (o que nao se aplica vira "n/a"), ajuste as convencoes ao stack real e apague ESTA linha de instrucao. Este arquivo e curto de proposito: aponta para as skills, nao repete o conteudo delas (economia de token).

## Stack
- Backend: {{STACK_BACKEND}}  (ex: Node 20 + Fastify + TypeScript ESM | Python 3.12 + FastAPI)
- Frontend: {{STACK_FRONTEND}}  (ex: Next.js 16 + Tailwind 4 + TanStack Query | React Native + Expo + Firebase)
- Infra/banco: {{INFRA}}  (ex: Railway + Postgres | Firestore | Vercel)

## Como o Claude trabalha aqui (protocolo ADP)
- Feature grande ou requisito ainda incerto: rode `/adp-spec`.
- Mudanca pequena, direta e ja compreendida: plan mode direto, sem spec.

Ponteiros de skill (abra a skill so quando a tarefa pedir):
- Descobrir/refinar requisitos -> skill `grill-me`
- Virar requisito em PRD -> `to-prd`  ·  Fatiar PRD em issues -> `to-issues`
- Executar as issues em sequencia -> `tech-lead`  ·  Verificar e testar -> `qa`
- Mexeu no BACKEND -> SEMPRE abra `backend-layers` (variante {{VARIANTE_BACKEND}}) antes de codar. [apague esta linha se o projeto NAO tem backend proprio]
- Mexeu no FRONTEND/UI -> SEMPRE abra `frontend-react` (variante {{VARIANTE_FRONTEND}}: next | react-native | vanilla-static) antes de codar
- Precisa de dado externo / preco / OSINT / doc de lib -> `web-buscador` (+ context7 para docs)

## Convencoes deste projeto (do / don't)
- Backend em camadas: `router -> controller -> service -> repository`. Dependencia so aponta para baixo, via porta (interface), nunca pulando camada. Regra dura: arquivo passou de ~500 linhas, quebre por responsabilidade.
- Frontend: `api -> hook de dominio -> container -> presentational`. Estado de servidor via TanStack Query, nunca espelhado em `useState`. Container e presentational separados.
- TypeScript estrito, sem `any`. Commits pequenos e focados, um assunto por commit.
- NUNCA commitar `.env`, chave ou segredo (o hook do ADP barra automaticamente).
- {{CONVENCAO_EXTRA_1}}

## Rodar e testar localmente
- Instalar: `{{CMD_INSTALL}}`
- Dev: `{{CMD_DEV}}`
- Testes: `{{CMD_TEST}}`  ·  Lint: `{{CMD_LINT}}`  ·  Typecheck: `{{CMD_TYPECHECK}}`

## Gate humano (nao pular)
Nada entra no `main` sem QA verde + revisao do PR por você. Mesmo com tech-lead e QA atuando, o PR e revisado antes do merge. Sem revisar = voltou pro vibe coding.

## Mapa vivo
Este projeto tem um `arquitetura_{{SLUG_PROJETO}}.html` na raiz (canvas estilo n8n): features e dependencias, endpoints, webhooks e integracoes externas, cada no linkando o arquivo real (`C:\...`). Atualizado pela skill `gerar-mapa` no fim de cada `/adp-spec`. Use-o pra achar rapido qual arquivo mexer.
