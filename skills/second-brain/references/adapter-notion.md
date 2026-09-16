# Notion adapter — V1

Notion is the first documented storage adapter. This document guides an existing authenticated connector; it does not install one or provide a standalone client. Discover the host's tool names, input schemas, and exposed API semantics at runtime.

## Mapping

| Logical operation | Notion representation |
|---|---|
| discover | Inspect shared roots and connector access; search accessible page/collection titles |
| inspect_schema | Retrieve the relevant database/data source schema or inspect ordinary pages |
| search | Title search, then queries of mapped data sources and inspection of known pages |
| read | Retrieve page properties and its blocks, including nested children and pagination |
| create | Create a page under the inspected parent or a row using the actual data source schema |
| update | Patch appropriate properties/blocks; preserve unrelated content |
| relocate | Update an existing category/status mapping or use a supported move operation |

A logical record is typically a page; its body is block content. Collections may be pages or database data sources. Keep page, database, and data source IDs distinct. Inspect the connector's current parent requirements rather than assuming they are interchangeable. Do not hardcode API versions or obsolete argument shapes in the core.

For structured collections, inspect property types and allowed values before writing. Map title, state, category, deadline, and relations only where real equivalents exist. Do not add an option merely to fit this skill. Use body sections for optional information or explain the unsupported mapping. Leave deadlines unset when not established by the user.

## Canonical databases and views

For the canonical Second Brain, map Notion databases as one canonical database each for Areas, Projects, Tasks, Knowledge, Resources, and Files. Never create one Tasks, Knowledge, Resources, Files, or Projects database inside every Area or Project page. Use Notion linked database views filtered by relation instead.

Before creating a Notion database for Projects, Tasks, Knowledge, Resources, or Files, search the selected root and accessible shared pages for an existing canonical database or equivalent data source. Inspect schema and representative rows before deciding it is missing.

The logical relations are:

- Projects -> Areas.
- Tasks -> Projects and Areas.
- Knowledge -> Projects and Areas where applicable.
- Resources -> Projects and Areas where applicable.
- Files -> Projects and Areas where applicable.

When a task is created from a Project page or request scoped to one Project, set the Project relation and derive the Area from the Project relation when Notion properties or automation support it. If Area is a rollup/formula relation derived from Project, do not overwrite it manually. If derivation is unsupported or ambiguous, leave Area unset and report the limitation instead of guessing.

The canonical Tasks database should expose these properties when supported by the live schema: Tarefa as title, Status as status/select, Tipo as select, Projeto as relation, Área as relation/rollup/formula, Prioridade, Prazo, Responsável, Notas, and Contexto if Contexto already exists. The only default Status options are Backlog, Em andamento, Impedido, and Concluído; empty is allowed. Do not add Inbox, Próxima, or Aguardando as default statuses. The default Tipo options are Produção, E-commerce, Financeiro, Estrutura, Orçamentos, Marketing/Comercial, and Administrativo; empty is allowed.

If the connector supports database view creation, bootstrap two views on the canonical Tasks database: `Por Status` as a board grouped by Status and `Por Tipo` as a board grouped by Tipo. If it cannot create views, create the database and properties, then clearly report the exact views the user should add manually.

Area pages should contain linked views filtered to that Area across canonical Projects, Tasks, Knowledge, Resources, and Files. Project pages should contain linked views filtered to that Project across canonical Tasks, Knowledge, Resources, and Files. If the connector cannot create linked views, add a concise section describing the intended filtered views rather than creating duplicate databases.

## Search and permissions

The official API search is title-oriented; it does not promise full body search or exhaustive workspace enumeration. Search can lag behind newly shared content. Use mapped data source queries for records within those collections, and direct retrieval for known locators. Follow tool pagination. If a host exposes richer search, establish its scope rather than assuming API limitations or guarantees transfer unchanged. [Official search reference](https://developers.notion.com/reference/post-search), [search limitations](https://developers.notion.com/reference/search-optimizations-and-limitations).

Empty search results do not prove the workspace is empty. A missing shared root may mean lack of access. Ask for a page link or for sharing through the host's connection flow when discovery cannot establish the target. Never substitute a new private copy for an inaccessible known project.

## Writes and archive

Read current blocks before merging. Update the relevant summary section or append a clearly dated consolidation if safe replacement is unavailable. Do not replace the whole page to update one section. Preserve formatting/content that the tool cannot round-trip.

PARA Archive is a logical location or classification. Do not map it to a Notion trash/archive API flag that removes normal access. If no reversible mapping or move exists, propose the change and report unsupported; do not simulate a move by copying and deleting.

Read the resulting page and changed properties to verify. A create response without body verification is a reported write, not a verified consolidation. Reconcile timeouts through known IDs and destination queries before repeating writes. Follow connector retry guidance for rate limits, with bounded retries; authorization failures require resolving access, not repeated calls.

Private workspace IDs and property maps belong in user configuration under adapters.notion. The distributed package contains no live workspace identifiers.
