# ADP · Fase Zero

Método para transformar uma ideia de software em requisitos verificáveis antes de iniciar a implementação. A Fase Zero reúne uma entrevista de escopo e a elaboração de um documento de requisitos do produto, o PRD.

## O que está neste repositório

| Recurso | Função |
| --- | --- |
| `grill-me` | Levantar objetivo, contexto, restrições e decisões pendentes |
| `to-prd` | Organizar as decisões em requisitos, histórias de uso e critérios de aceitação |
| Arquivo de convenções | Adaptar o processo ao projeto e à stack |

As duas etapas usam agentes de IA, com revisão humana das decisões e do documento resultante. O pacote não implementa automaticamente o produto descrito no PRD.

## Instalação

O pacote é um plugin para Claude Code. No terminal interativo dessa ferramenta:

```text
/plugin marketplace add murilomn58/Claude-Spec-Driven-Fase-Zero
/plugin install adp-fase-zero@adp
```

Inicie a entrevista com a skill `grill-me`. Depois de revisar o escopo, use `to-prd` para produzir o documento.

Para instalação manual, copie `skills/grill-me` e `skills/to-prd` para o diretório de skills do projeto. O arquivo `CLAUDE.coringa.md` contém as convenções de integração e deve ser adaptado ao projeto de destino.

## Exemplo de aplicação

Uma solicitação como “criar uma agenda de atendimentos” precisa definir quem agenda, como os horários são disponibilizados, quais conflitos devem ser impedidos e como o resultado será verificado. A entrevista registra essas decisões; o PRD as transforma em trabalho revisável.

## Escopo

Este repositório contém a Fase Zero. As etapas adicionais do protocolo são apresentadas no [site do ADP](https://claudespecdriven.com.br).

## Autoria e licença

Projeto de [Murilo Narciso](https://www.linkedin.com/in/murilonarciso/). Licença [MIT](LICENSE).
