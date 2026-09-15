# Formato e fontes

Conferido em 15 de setembro de 2026 em fontes oficiais. As regras de distribuição podem mudar; revisar antes de releases futuros.

## Agent Skills

O pacote usa `SKILL.md` com frontmatter YAML, campos obrigatórios `name` e `description`, e referências locais. O slug corresponde à pasta. A configuração própria permanece em assets e não altera o formato padrão. [Especificação](https://agentskills.io/specification).

## skills.sh e CLI

A CLI aceita repositórios e caminhos locais, seleção com `--skill` e descoberta sem instalação com `--list`. O diretório `skills/` é uma localização de descoberta suportada. [CLI oficial](https://github.com/vercel-labs/skills).

As skills são hospedadas em repositórios GitHub; a FAQ descreve o ranking a partir de instalações registradas. Este pacote publica em `raphaeupow/second-brain` e não precisa de um manifest de aplicação adicional. [Documentação](https://www.skills.sh/docs), [FAQ](https://www.skills.sh/docs/faq).

## Primeiro adapter

A busca oficial do Notion é orientada a títulos e possui limitações de cobertura e atualização do índice. Por isso o adapter combina descoberta com leitura direta e consultas da coleção mapeada, conforme capacidades reais. [Busca](https://developers.notion.com/reference/post-search), [limitações](https://developers.notion.com/reference/search-optimizations-and-limitations).

## Método

PARA é atribuído a Tiago Forte; os comportamentos da skill, o contrato de adapter e os limites de persistência são decisões deste projeto. [Fonte do método](https://fortelabs.com/blog/para/).
