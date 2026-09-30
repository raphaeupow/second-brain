# Second Brain — especificação V2

## Objetivo

Criar uma Agent Skill portátil que recupere contexto útil e persista apenas o conteúdo solicitado. A conversa é memória de trabalho; o vault no Google Drive é a memória durável.

## Decisões de projeto

- Identidade pública: Second Brain. Slug instalável: `second-brain`.
- Backend atual: Google Drive.
- Formato persistente: arquivos Markdown portáveis com YAML e links internos compatíveis com Obsidian.
- Root preferencial: pasta `Segundo_Cerebro`.
- Idioma: respostas acompanham o idioma do usuário.
- Arquitetura: núcleo de comportamentos, contrato lógico e adapter Google Drive.
- Não usar Notion nesta versão.
- Não consultar Google Calendar salvo pedido explícito do usuário.
- Sem runtime próprio: autenticação e ferramentas são fornecidas pelo host/conector.

## Modelo e fluxo

A estrutura persistente típica é:

- Caixa de Entrada → `00 - Caixa de Entrada`
- Projetos → `10 - Projetos`
- Áreas → `20 - Areas`
- Recursos/Conhecimento → `30 - Recursos e Conhecimento`
- Arquivo → `40 - Arquivo`
- Tarefas → `50 - Tarefas`
- Anexos → `90 - Anexos`
- Sistema → `99 - Sistema`

As tarefas são arquivos Markdown individuais com estado no YAML e contexto no corpo.

Fluxo de leitura: intenção → root verificado → busca/listagem → leitura direta → síntese com links e limites.

Fluxo de escrita: intenção explícita → destino verificado → busca → correspondência → leitura fresca → merge Markdown/YAML → substituição raw do arquivo → leitura de verificação → recibo.

## Escopo funcional

| Requisito | Critério de aceite |
|---|---|
| Capture | Salva o conteúdo delimitado no arquivo/local correto |
| Consolidate | Mescla objetivo, decisões e ações sem converter hipóteses em fatos |
| Recall | Busca/lê o Drive primeiro e informa fontes |
| Organize | Adota estrutura existente e aplica só mudanças autorizadas |
| Plan | Mantém recomendações no chat até pedido de persistência |
| Review | Revisa o período e separa diagnóstico de alterações |
| Search-before-create | Considera aliases, escopo, folder listing e acesso |
| Markdown preservation | Não converte arquivos do vault para Google Docs |
| Tasks | Usa `50 - Tarefas` como coleção canônica |
| Drive verification | Releitura obrigatória após gravação |
| Archive | Move/classifica reversivelmente; não usa lixeira |
| Backend boundary | Não cai silenciosamente para Notion ou outro storage |

## Não objetivos

Sincronização entre provedores, migração automática, busca vetorial própria, transcrição de voz, execução de calendário ou mensagens, API própria e criptografia própria.

## Extensão

Um novo adapter deve documentar capacidades, busca e cobertura, leitura completa, identidade, atualização, classificação reversível, erros e verificação. Deve passar os mesmos cenários sem mudar os seis comportamentos.

## Limites

As regras são instruções ao agente. Permissões dependem do host e do Google Drive. Busca pode ser incompleta. Atualização raw sem controle condicional tem janela de concorrência; por isso a skill relê imediatamente antes de gravar e verifica depois.
