# Bootstrap and adopt

Run when the selected backend, root, or mapping is missing or stale.

The canonical Second Brain has exactly one database each for Areas, Projects, Tasks, Knowledge, Resources, and Files. Area and Project pages expose filtered linked views of those canonical databases. They never own duplicate project-specific or area-specific databases.

1. Discover the connector's capabilities and accessible scope. If disconnected, explain that the user must connect storage using their host's integration flow. Never request secrets in chat. Prepare drafts while access is unavailable.
2. Inspect any user-supplied root first. Search for existing Areas, Projects, Tasks, Knowledge, Resources, Files, archive, inbox, and equivalent collections using the workspace language and aliases. Inspect representative content and schemas; names alone do not establish purpose.
3. **Adopt first:** map equivalent canonical databases without renaming or migrating them. A system need not look like PARA physically. Several logical locations may share one collection with a category field, but do not create duplicate databases per Area or Project. Preserve relations and user conventions unless they conflict with the canonical rules below.
4. If multiple roots are plausible, report the concrete candidates and ask which to use. If discovery is incomplete or denied, request the relevant locator/access; do not conclude the system is empty.
5. **Before creating any canonical database:** search for and inspect an existing equivalent for Projects, Tasks, Knowledge, Resources, or Files. A partial search, missing access, or stale index is not absence; ask for a locator or access when needed.
6. **Bootstrap if genuinely missing:** propose a minimal root with the six canonical databases: Areas, Projects, Tasks, Knowledge, Resources, and Files. Optional Inbox and Archive may be pages or classifications, not replacements for the canonical databases. Explain the concrete structure before writing if the setup request did not specify it. An explicit instruction to create that structure already authorizes it.
7. Configure the logical hierarchy: Area -> Project -> Tasks/Knowledge/Resources/Files. Projects relate to Areas. Tasks, Knowledge, Resources, and Files relate to Projects when applicable and to Areas directly when they are area-level.
8. The canonical Tasks database should include Tarefa, Status, Tipo, Projeto, Área, Prioridade, Prazo, Responsável, Notas, and Contexto if Contexto already exists or is already part of the user's schema. Do not add conflicting legacy properties merely to satisfy older instructions.
9. Status defaults are only Backlog, Em andamento, Impedido, and Concluído. Empty Status is valid and means no status. Do not create Inbox, Próxima, or Aguardando as default Status options.
10. Tipo defaults are Produção, E-commerce, Financeiro, Estrutura, Orçamentos, Marketing/Comercial, and Administrativo. Empty Tipo is valid.
11. When supported, create two views on the same canonical Tasks database: `Por Status`, a board grouped by Status, and `Por Tipo`, a board grouped by Tipo. These are views of the Tasks database, not additional databases.
12. For each Area page, add or describe filtered linked views into the canonical Projects, Tasks, Knowledge, Resources, and Files databases. For each Project page, add or describe filtered linked views into the canonical Tasks, Knowledge, Resources, and Files databases. Never create project-owned or area-owned databases for those records.
13. Search before each container creation. Verify new containers individually. On partial failure, keep verified IDs and resume only missing steps after reconciliation. Do not delete successful steps as automatic rollback.
14. Read back locations and mappings. Return the adopted/created destinations and limitations. If asked to save the mapping, use the user's private config location or a designated private storage note, never the distributed skill directory.

Only structure creation and requested configuration are covered by setup authorization. Setup does not authorize importing the conversation or saving every future exchange.
