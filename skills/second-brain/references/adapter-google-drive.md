# Google Drive adapter — V2

Google Drive is the active persistence adapter for Second Brain. The vault is stored as portable Markdown files and folders so it can also be opened in Obsidian. This adapter guides an authenticated Google Drive connector; it does not store credentials or hardcode private Drive IDs.

## Root discovery

1. Search for an accessible folder named `Segundo_Cerebro`.
2. If exactly one clear match exists, inspect it. If multiple plausible roots exist, ask the user which one is canonical.
3. Confirm the expected vault structure from direct children, `Dashboard.md`, and `99 - Sistema/Segundo Cerebro.md` when present.
4. Do not use a ZIP backup as the active source when unpacked Markdown files are available.
5. Do not use Notion as fallback.

For large folders, use a paginated folder-list operation when available. A folder fetch that returns only the first page is not proof of complete coverage.

## Mapping

| Logical operation | Google Drive / Markdown representation |
|---|---|
| discover | Search Drive for `Segundo_Cerebro`, inspect root metadata and direct children |
| inspect_schema | Read representative Markdown/YAML files and system documentation |
| search | Search Drive titles/content, then narrow to known vault folders; use folder listing for complete collection review |
| read | Fetch the raw Markdown text and metadata for the identified file |
| create | Upload a new `text/markdown` file into the correct vault folder |
| update | Replace raw Markdown bytes in place while preserving the same Drive file ID when possible |
| relocate | Move the existing file between vault folders by changing parent folders; do not copy/delete unless explicitly required |
| archive | Move/classify into `40 - Arquivo` reversibly; never map Archive to Drive trash |

## Canonical folders

Use the existing structure when present:

- `00 - Caixa de Entrada`
- `10 - Projetos`
- `20 - Areas`
- `30 - Recursos e Conhecimento`
- `40 - Arquivo`
- `50 - Tarefas`
- `90 - Anexos`
- `99 - Sistema`

`Dashboard.md` is the navigation entrypoint. These names are the current vault convention, not permission to create duplicates when equivalents already exist.

## Markdown and YAML rules

Raw Markdown is part of the data model. Never convert vault files to native Google Docs as a write shortcut.

When reading a file:

- Parse YAML frontmatter without discarding unknown keys.
- Preserve the original key names, accents, quoting style where practical, and wiki-link syntax.
- Treat the Markdown body as user-visible durable content.
- Treat Drive `modified_time` and the freshly read content as the observed revision snapshot.

When updating a file:

1. Fetch it immediately before writing.
2. Merge only the intended YAML/body changes.
3. Generate the complete replacement bytes as UTF-8 Markdown.
4. Use the connector's raw-file update operation with the same Drive file ID.
5. Fetch again and compare the intended fields/content.

If the runtime cannot create a replacement file reference for raw bytes, stop and return an unsaved draft. Do not create a Google Doc substitute.

## Task mapping

Each task is a Markdown file in `50 - Tarefas`. Common YAML keys include:

- `titulo` / `tarefa`
- `status`
- `tipo`
- `projeto`
- `área`
- `prioridade`
- `prazo` or `date:prazo:start`
- `responsável`
- `notas`
- `contexto`
- `criado_em`
- completion date only when explicitly present

Relations are represented with wiki links such as `[[10 - Projetos/Projeto X|Projeto X]]`. Preserve existing links rather than duplicating related content.

For newly created tasks, do not invent missing fields. Default status only when needed to Backlog, Em andamento, Impedido, or Concluído. Empty status is valid. Preserve existing status labels in migrated files.

If `50 - Tarefas/README.md` exists, update its item index after a successful create/rename. Do not treat an index update failure as failure of the canonical task write; report the partial result.

## Search coverage

Drive search can be metadata-oriented and body hydration may be best-effort. For Recall of a known item, direct file fetch is authoritative. For reviews that require all tasks/projects in a folder, enumerate the folder with pagination rather than relying on keyword search.

A zero-result search does not prove the vault or record is absent. Verify the intended folder and access first.

## Concurrency and verification

Raw Drive file replacement may not provide transactional compare-and-swap semantics. Reduce overwrite risk by:

- retaining the initial content and modified timestamp,
- fetching again immediately before write,
- reconciling if the content or modified timestamp changed,
- changing only intended sections,
- fetching after write to verify.

After timeout or unknown outcome, inspect the same Drive file ID or search the exact destination before retrying.

## Permissions and safety

The connector controls access. Never request tokens in chat. Do not change sharing permissions unless the user explicitly requests that action. Do not delete files when a reversible move/classification satisfies the request.

Google Calendar is not part of this adapter and should not be queried unless the user explicitly asks for it.
