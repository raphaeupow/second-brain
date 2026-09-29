# Bootstrap and adopt — Google Drive vault

Run when the Second Brain root or folder mapping is missing, stale, or ambiguous.

The preferred persistent structure is a portable Markdown vault in Google Drive. Adopt an existing vault before creating anything.

1. Discover Google Drive access. If disconnected, explain that the user must connect Google Drive through the host. Never request secrets in chat.
2. Search for the root folder `Segundo_Cerebro`. Inspect user-supplied roots first. If multiple plausible roots exist, ask which one is canonical.
3. Inspect direct children, `Dashboard.md`, and `99 - Sistema/Segundo Cerebro.md` when present.
4. **Adopt first:** preserve existing folder names, Markdown file names, YAML keys, wiki links, and status labels. Do not migrate or rename merely to match this document.
5. Map the logical PARA locations to existing folders. The current canonical convention is:
   - `00 - Caixa de Entrada`
   - `10 - Projetos`
   - `20 - Areas`
   - `30 - Recursos e Conhecimento`
   - `40 - Arquivo`
   - `50 - Tarefas`
   - `90 - Anexos`
   - `99 - Sistema`
6. A partial search, missing access, or truncated folder listing is not evidence that a location is absent. Follow pagination or ask for the specific root locator.
7. **Bootstrap only when genuinely missing and explicitly authorized:** propose/create the minimal missing folders, not a duplicate second structure. Do not bootstrap Notion databases.
8. `50 - Tarefas` is the canonical task collection. Tasks are standalone Markdown files with YAML frontmatter. Do not create task sub-databases or task stores inside individual projects.
9. Projects and Areas are linked through wiki links/YAML relationships. Related tasks/resources should link to their Project/Area when clear.
10. Preserve Markdown portability. Do not convert vault files into native Google Docs/Sheets/Slides during setup.
11. If an index such as `README.md` or `Dashboard.md` exists, preserve its role and update it only when the requested setup makes that necessary.
12. Verify every created folder/file by reading metadata/content back. On partial failure, keep verified successful steps and resume only missing steps after reconciliation.
13. If asked to save a private mapping, store no tokens. Prefer a user-designated private config location and avoid hardcoding Drive IDs in the public skill.

Setup authorization covers only structure creation/configuration. It does not authorize importing the conversation or automatically saving future exchanges.
