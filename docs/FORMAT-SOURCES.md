# Formato e fontes

## Agent Skills

O pacote usa `SKILL.md` com frontmatter YAML, campos obrigatórios `name` e `description`, e referências locais. O slug corresponde à pasta. [Especificação](https://agentskills.io/specification).

## skills.sh e CLI

A CLI aceita repositórios e caminhos locais, seleção com `--skill` e descoberta sem instalação com `--list`. O diretório `skills/` é uma localização de descoberta suportada. [CLI oficial](https://github.com/vercel-labs/skills).

## Adapter atual

O backend atual é Google Drive. A skill usa pesquisa, listagem de pastas, leitura direta, metadados e substituição de arquivos raw expostas pelo conector disponível no host. Como busca pode não representar cobertura total de uma pasta, revisões completas devem preferir enumeração/listagem paginada do diretório canônico.

## Formato persistente

Os dados do Segundo Cérebro são arquivos Markdown com YAML e links wiki, preservados como arquivos raw para continuarem portáveis e utilizáveis no Obsidian. A skill não converte esses arquivos para Google Docs durante gravações.

## Método

PARA é atribuído a Tiago Forte; os comportamentos da skill, o contrato de adapter e os limites de persistência são decisões deste projeto. [Fonte do método](https://fortelabs.com/blog/para/).
