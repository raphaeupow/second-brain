# Second Brain

[![skills.sh](https://skills.sh/b/raphaeupow/second-brain)](https://skills.sh/raphaeupow/second-brain)

**Conversa é memória de trabalho. Storage externo é memória persistente.**

Second Brain transforma conversas em contexto reutilizável, com organização PARA e persistência explícita. O núcleo é independente da ferramenta; Notion é o primeiro adapter.

O mesmo repositório agora possui duas formas de distribuição:

- **Agent Skill / skills.sh** — continua instalável como `second-brain`.
- **ChatGPT plugin** — empacota a mesma skill e declara o app Notion como dependência de persistência.

Não existe servidor MCP próprio nesta arquitetura. O plugin reutiliza o app Notion autenticado pelo ChatGPT e mantém a lógica de storage atrás de um contrato de adapter.

## Instalação da Agent Skill

```sh
npx skills add raphaeupow/second-brain --skill second-brain
```

Para conferir a descoberta local, na raiz deste projeto:

```sh
npx skills add . --list
```

A instalação da skill isolada não conecta o Notion nem concede acesso. Sem uma conexão autenticada, a skill produz rascunhos identificados como não salvos.

## ChatGPT plugin

O pacote do plugin é descrito por `.codex-plugin/plugin.json`. A dependência do Notion fica em `.app.json`, usando o app oficial do Notion. Autenticação continua sendo responsabilidade da conexão do usuário no ChatGPT; nenhum token é armazenado neste repositório.

A skill operacional continua em `skills/second-brain/`. Assim, o comportamento não é duplicado entre a distribuição do skills.sh e a do plugin.

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
        Storage Adapter
            |
            +-- Notion (V1) -> app Notion autenticado
            +-- Google Drive (futuro)
            +-- Obsidian / arquivos (futuro)
            +-- outros providers (futuro)
```

O contrato evita espalhar IDs, propriedades e semântica de API pelo núcleo. Trocar ou adicionar um provider deve exigir um novo adapter, não uma reescrita dos comportamentos.

## Conteúdo do repositório

```text
second-brain/
├── .codex-plugin/
│   └── plugin.json
├── .app.json
├── adapters/
│   └── README.md
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

Todas as dependências operacionais da skill ficam dentro de sua pasta, para sobreviver à instalação isolada. A documentação de arquitetura e empacotamento fica fora do contexto operacional.

## Configuração opcional

O arquivo `skills/second-brain/assets/second-brain.config.example.json` é uma convenção do Second Brain, não um manifesto reconhecido pelo skills.sh nem um arquivo executado automaticamente. O agente pode ler uma cópia privada indicada pelo usuário. Valores `null` significam não descoberto. Nunca coloque tokens, IDs reais ou dados pessoais no pacote público.

## Publicação

O código está em `raphaeupow/second-brain`. A distribuição pelo skills.sh continua independente do empacotamento como plugin. Confira descoberta com `npx skills add raphaeupow/second-brain --list` e execute os casos de aceitação com o conector escolhido em um espaço de testes.

## Validação e limites

Veja `docs/SPEC.md`, `docs/ACCEPTANCE.md` e `docs/FORMAT-SOURCES.md`. Instruções dependem do cumprimento pelo agente; não há enforcement por código, transações, sincronização ou execução em segundo plano. Teste real de escrita no Notion permanece um smoke test de release.

PARA é um método de Tiago Forte. Este projeto é independente, sem afiliação com Forte Labs, Notion ou Vercel.
