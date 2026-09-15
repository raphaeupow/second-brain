# Second Brain

[![skills.sh](https://skills.sh/b/raphaeupow/second-brain)](https://skills.sh/raphaeupow/second-brain)

**Conversa é memória de trabalho. Storage externo é memória persistente.**

Agent Skill pública para transformar conversas em contexto reutilizável, com organização PARA e persistência explícita. O nome instalável é `second-brain`; a identidade do projeto é **Second Brain**.

O núcleo independe da ferramenta. Notion é o primeiro adapter documentado. A V1 contém instruções e referências interpretadas pelo agente: não é um aplicativo, servidor MCP ou SDK, e não fornece um conector próprio.

## Instalação

```sh
npx skills add raphaeupow/second-brain --skill second-brain
```

Para conferir a descoberta local, na raiz deste projeto:

```sh
npx skills add . --list
```

O agente precisa suportar Agent Skills e ter uma conexão autenticada com o storage escolhido. Instalar a skill não conecta o Notion nem concede acesso. Sem conexão, a skill pode produzir rascunhos identificados como não salvos.

## Comece assim

> Use Second Brain para adotar minha estrutura existente no Notion. Primeiro identifique as páginas e os bancos equivalentes.

Se ainda não houver estrutura:

> Configure Second Brain sob a página que vou indicar. Crie Projects, Areas, Resources, Archive e Inbox, se não existirem.

Depois:

| Você pede | Comportamento |
|---|---|
| “Salve esta ideia” | Capture: grava uma captura curta após buscar correspondências |
| “Consolide este projeto” | Consolidate: sintetiza e persiste o contexto relevante |
| “Onde paramos?” | Recall: consulta fontes persistentes antes de responder |
| “Organize meu segundo cérebro” | Organize: inspeciona e propõe mudanças |
| “Planeje minha semana” | Plan: propõe ações com base no contexto |
| “Faça a revisão semanal” | Review: revisa e propõe próximos passos |

“Consolide aqui, sem salvar” permanece no chat. Planejar e revisar não gravam automaticamente. “Mova este projeto para Archive” autoriza essa alteração específica. Salvar uma nota não cria tarefas separadas sem solicitação.

## Arquitetura

```text
Conversa / intenção do usuário
            |
Núcleo: Capture · Consolidate · Recall · Organize · Plan · Review
            |
Método PARA + contrato lógico de storage
            |
Adapter Notion (V1) -> conector autenticado do agente -> storage
```

Adapters futuros podem representar arquivos locais, Obsidian ou outros serviços. Eles não estão implementados nesta versão. O contrato evita espalhar IDs, propriedades e semântica de API pelo núcleo.

## Conteúdo do repositório

```text
second-brain/
├── README.md
├── LICENSE
├── .gitignore
├── docs/
│   ├── SPEC.md
│   ├── ACCEPTANCE.md
│   └── FORMAT-SOURCES.md
└── skills/
    └── second-brain/
        ├── SKILL.md
        ├── LICENSE
        ├── assets/
        │   └── second-brain.config.example.json
        └── references/
            ├── adapter-contract.md
            ├── adapter-notion.md
            ├── bootstrap.md
            ├── para.md
            ├── persistence.md
            ├── review.md
            └── example.md
```

Todas as dependências de instrução da skill ficam dentro de sua pasta, para sobreviver à instalação isolada. A documentação do projeto fica fora do contexto operacional.

## Configuração opcional

[Exemplo JSON](skills/second-brain/assets/second-brain.config.example.json). Ele é uma convenção do Second Brain, não um manifest reconhecido pelo skills.sh ou um arquivo executado automaticamente. O agente pode ler uma cópia privada indicada pelo usuário. Valores `null` significam não descoberto. Nunca coloque tokens, IDs reais ou dados pessoais no pacote público.

## Publicação

O código está em [raphaeupow/second-brain](https://github.com/raphaeupow/second-brain). Instale com o comando acima para o skills.sh registrar a skill via telemetria da CLI. Não há formulário de submissão; ranking e prazo de aparição não são garantidos. [FAQ oficial](https://www.skills.sh/docs/faq).

Confira descoberta com `npx skills add raphaeupow/second-brain --list`. Execute os casos de aceitação com o conector escolhido em um espaço de testes.

## Validação e limites

Veja [especificação](docs/SPEC.md), [casos de aceitação](docs/ACCEPTANCE.md) e [fontes do formato](docs/FORMAT-SOURCES.md). Instruções dependem do cumprimento pelo agente; não há enforcement por código, transações, sincronização ou execução em segundo plano. Teste real de escrita no Notion permanece um smoke test de release, não um resultado desta distribuição.

PARA é um método de Tiago Forte. Este projeto é independente, sem afiliação com Forte Labs, Notion ou Vercel.
