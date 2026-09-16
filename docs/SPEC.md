# Second Brain — especificação V1

## Objetivo

Criar uma Agent Skill portátil que recupere contexto útil e persista apenas o conteúdo solicitado. O storage é a fonte durável; a conversa é um espaço temporário para pensar, decidir e preparar mudanças. Não se promete lembrança entre sessões sem gravação externa e posterior recuperação.

## Decisões de projeto

- Identidade pública: Second Brain. Slug instalável: `second-brain`, sem fornecedor.
- Distribuição: uma skill autocontida em `skills/second-brain/`, com Markdown e JSON ilustrativo.
- Idioma: instruções em inglês para distribuição ampla; documentação inicial em português. Respostas acompanham o idioma do usuário.
- Arquitetura: núcleo de comportamentos, contrato lógico e adapters de tradução para ferramentas já disponíveis.
- Arquitetura canônica do storage: bases únicas para Áreas, Projetos, Tarefas, Conhecimento, Recursos e Arquivos. Páginas de Área e Projeto exibem linked views filtradas dessas bases, sem bases duplicadas por Área ou Projeto.
- Notion: primeiro adapter documental; nenhuma credencial, conta ou estrutura pessoal embutida.
- Sem runtime na V1: um SDK executável adicionaria autenticação, dependências e manutenção sem ser necessário à entrega. Uma skill específica de Notion violaria a portabilidade pedida.

## Modelo e fluxo

O núcleo trabalha com registros, localização lógica PARA, conteúdo, origem, relações e revisão. O adapter converte essas intenções em operações reais. IDs permanecem opacos ao núcleo; propriedades e capacidades pertencem ao adapter.

A hierarquia lógica é Área → Projeto → Tarefas/Conhecimento/Recursos/Arquivos. Tarefas criadas no contexto de um Projeto devem ser relacionadas ao Projeto e herdar ou derivar a Área quando o schema permitir.

Fluxo de leitura: intenção → destino verificado → busca/leitura → síntese com fontes e limites.

Fluxo de escrita: intenção explícita → descoberta/schema → busca → correspondência → rascunho → conferência de revisão → mutação → leitura de verificação → recibo.

Os estados observáveis são rascunho, destino resolvido, mudança preparada, gravação reportada, gravação verificada ou falha/resultado desconhecido. Uma chamada iniciada não equivale a persistência.

## Escopo funcional

| Requisito | Critério de aceite |
|---|---|
| Capture | Salva o conteúdo delimitado; usa Inbox quando necessário |
| Consolidate | Mescla objetivo, decisões e ações sem converter hipóteses em fatos |
| Recall | Busca primeiro; informa fontes e contexto indisponível |
| Organize | Adota equivalências existentes e aplica só mudanças autorizadas |
| Plan | Mantém recomendações em conversa até pedido de persistência |
| Review | Revisa o período e separa diagnóstico de alterações |
| Search-before-create | Considera aliases, paginação, escopo e acesso |
| Canonical architecture | Usa bases únicas; páginas exibem views filtradas; não cria bases por Área/Projeto |
| Bootstrap/adopt | Prefere estrutura existente; cria seis bases canônicas só quando ausentes |
| Tasks schema | Status padrão só Backlog, Em andamento, Impedido e Concluído; Tipo padrão conforme arquitetura canônica; views Por Status e Por Tipo |
| Portabilidade | Nenhuma operação conceitual exige propriedade ou ID do Notion |
| Confiança na escrita | Lê o resultado e reconcilia timeouts antes de repetir |

## Não objetivos

Sincronização entre provedores, importação em massa, migração automática, busca vetorial própria, transcrição de voz, tarefas agendadas, envio de mensagens, API executável e criptografia própria. Nenhum adapter futuro é anunciado como funcional.

## Extensão

Um novo adapter deve documentar capacidades, busca e cobertura, leitura completa, mapeamento de schema, identidade, atualização, classificação reversível, erros e verificação. Deve passar os mesmos cenários sem mudar os seis comportamentos. V1 seleciona um backend por contexto; não faz fan-out nem replica dados.

## Limites e entrega

As regras são instruções ao agente. Segurança e permissões continuam dependendo do host e do storage. Busca pode ser incompleta e atualizações sem controle condicional têm uma janela de concorrência. O agente deve expor esses limites quando afetarem uma operação.

O pacote usa licença MIT. Distribuição: [raphaeupow/second-brain](https://github.com/raphaeupow/second-brain). Validação com credenciais de teste no storage continua sendo um smoke test de release, não um recurso omitido da skill.
