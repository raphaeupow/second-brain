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

## Search and permissions

The official API search is title-oriented; it does not promise full body search or exhaustive workspace enumeration. Search can lag behind newly shared content. Use mapped data source queries for records within those collections, and direct retrieval for known locators. Follow tool pagination. If a host exposes richer search, establish its scope rather than assuming API limitations or guarantees transfer unchanged. [Official search reference](https://developers.notion.com/reference/post-search), [search limitations](https://developers.notion.com/reference/search-optimizations-and-limitations).

Empty search results do not prove the workspace is empty. A missing shared root may mean lack of access. Ask for a page link or for sharing through the host's connection flow when discovery cannot establish the target. Never substitute a new private copy for an inaccessible known project.

## Writes and archive

Read current blocks before merging. Update the relevant summary section or append a clearly dated consolidation if safe replacement is unavailable. Do not replace the whole page to update one section. Preserve formatting/content that the tool cannot round-trip.

PARA Archive is a logical location or classification. Do not map it to a Notion trash/archive API flag that removes normal access. If no reversible mapping or move exists, propose the change and report unsupported; do not simulate a move by copying and deleting.

Read the resulting page and changed properties to verify. A create response without body verification is a reported write, not a verified consolidation. Reconcile timeouts through known IDs and destination queries before repeating writes. Follow connector retry guidance for rate limits, with bounded retries; authorization failures require resolving access, not repeated calls.

Private workspace IDs and property maps belong in user configuration under adapters.notion. The distributed package contains no live workspace identifiers.
