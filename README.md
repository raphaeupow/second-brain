# Second Brain

[![skills.sh](https://skills.sh/b/raphaeupow/second-brain)](https://skills.sh/raphaeupow/second-brain)

**Conversa é memória de trabalho. O Google Drive é a memória persistente.**

Second Brain transforma conversas em contexto reutilizável, com organização PARA e persistência explícita. A versão atual usa um vault portátil em Markdown no Google Drive, compatível com Obsidian.

## Backend atual

O armazenamento persistente oficial é o Google Drive. O vault esperado usa uma estrutura como:

```text
Segundo_Cerebro/
├── 00 - Caixa de Entrada/
├── 10 - Projetos/
├── 20 - Areas/
├── 30 - Recursos e Conhecimento/
├── 40 - Arquivo/
├── 50 - Tarefas/
├── 90 - Anexos/
├── 99 - Sistema/
└── Dashboard.md
```

As tarefas ficam como arquivos Markdown individuais em `50 - Tarefas`, com propriedades no cabeçalho YAML e contexto legível no corpo do arquivo.

O plugin não usa Notion nesta versão e não consulta Google Calendar, a menos que o usuário peça explicitamente para combinar o calendário com o Segundo Cérebro.

## Distribuição

- **Agent Skill / skills.sh** — instalável como `second-brain`.
- **ChatGPT plugin** — empacota a mesma skill e declara Google Drive como dependência de persistência.

Não existe servidor MCP próprio nesta arquitetura. O plugin reutiliza o conector autenticado do Google Drive disponível no ChatGPT.

## Instalação da Agent Skill

```sh
npx skills add raphaeupow/second-brain --skill second-brain
```

A instalação da skill isolada não concede acesso ao Drive. Sem uma conexão autenticada, a skill produz rascunhos identificados como não salvos.

## Comece assim

> Use Second Brain para retomar meu vault `Segundo_Cerebro` no Google Drive.

Depois:

| Você pede | Comportamento |
|---|---|
| “Salve esta ideia” | Capture: busca correspondências e grava um Markdown no destino correto |
| “Consolide este projeto” | Consolidate: lê o arquivo atual, sintetiza e persiste o contexto relevante |
| “Onde paramos?” | Recall: consulta os arquivos persistentes antes de responder |
| “Organize meu segundo cérebro” | Organize: inspeciona e propõe mudanças |
| “Planeje minha semana” | Plan: propõe ações com base no contexto |
| “Faça a revisão semanal” | Review: revisa e propõe próximos passos |

“Consolide aqui, sem salvar” permanece no chat. Planejar e revisar não gravam automaticamente. Salvar uma nota não cria tarefas separadas sem solicitação.

## Arquitetura

```text
Conversa / intenção do usuário
            |
Núcleo: Capture · Consolidate · Recall · Organize · Plan · Review
            |
Método PARA + contrato lógico de storage
            |
     Google Drive adapter
            |
      Segundo_Cerebro/
        Markdown + YAML
```

O adapter preserva IDs, arquivos, relações e semântica do Drive fora do núcleo. A skill mantém os arquivos Markdown portáveis e evita conversão para Google Docs durante gravações.

## Tarefas

`50 - Tarefas` é a coleção canônica. Cada tarefa é um arquivo `.md` com YAML. Campos comuns incluem `status`, `tipo`, `projeto`, `área`, `prioridade`, `prazo`, `notas`, `contexto` e `criado_em`.

A skill preserva os valores existentes. Para tarefas novas, quando um status padrão for necessário, usa somente Backlog, Em andamento, Impedido ou Concluído. Campos ausentes permanecem ausentes em vez de serem inventados.

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
            ├── adapter-google-drive.md
            ├── bootstrap.md
            ├── para.md
            ├── persistence.md
            ├── review.md
            └── example.md
```

## Configuração opcional

`skills/second-brain/assets/second-brain.config.example.json` é uma convenção do Second Brain, não um manifesto executado automaticamente. Valores `null` significam não descoberto. Nunca coloque tokens ou IDs privados reais no pacote público.

## Publicação

O código está em `raphaeupow/second-brain`. A distribuição pelo skills.sh continua independente do empacotamento como plugin.

## Validação e limites

As regras são instruções ao agente. Segurança e permissões continuam dependendo do host e do Google Drive. Busca pode ser incompleta, e substituição de arquivos raw deve ser verificada por leitura posterior.

PARA é um método de Tiago Forte. Este projeto é independente, sem afiliação com Forte Labs, Google ou Obsidian.
