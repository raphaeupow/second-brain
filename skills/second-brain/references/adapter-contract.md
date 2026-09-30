# Storage adapter contract — V2

This is a behavioral interface interpreted by an agent, not an SDK, API server, or executable dispatcher. Core behaviors use logical records. Only adapters interpret provider-specific identifiers, file/container types, query semantics, and write mechanisms.

## Logical record

Use these concepts where applicable, without requiring a matching structured field for each:

| Field | Meaning |
|---|---|
| id, locator | Stable provider identity and retrievable link; opaque to the core |
| title, aliases | Human name and alternative search terms |
| kind | area, project, task, knowledge, resource, file, or note |
| placement | areas, projects, tasks, knowledge/resources, archive, inbox, attachments, or system |
| body | Useful durable content, readable without this skill |
| state | Existing workflow state, mapped without inventing options |
| sources | Available source links, dates, and attribution |
| updated_at, revision | Provider revision or observed timestamp/content snapshot |
| relations | Related record identifiers/links when supported |

Archive is placement, preserving the original kind. Inbox is a staging queue outside the four PARA categories. Absent fields remain absent or unknown, not fabricated.

A backend may represent a canonical collection as a database, folder, label, or another durable grouping. Do not force database semantics onto a file/folder backend.

## Operations

All operations accept the selected root/scope. Names below are conceptual.

| Operation | Input | Output / fallback |
|---|---|---|
| discover | user-selected scope | identity, capabilities, roots, access limits |
| inspect_schema | collection/container locator | actual fields/file conventions, types, options, revision |
| search | query, scope, cursor | candidates, next cursor, coverage and limitations |
| read | locator | record, revision, completeness |
| create | destination, logical record | locator and outcome; reconcile uncertain results |
| update | locator, patch/replacement, observed revision | outcome and revision; protect concurrent changes |
| relocate | locator, logical placement | reversible classification/move or unsupported |

Represent outcomes as success, not_found, permission_denied, unsupported, conflict, retryable_failure, or unknown_outcome. Search coverage is scoped_complete, partial, or unknown; scoped_complete means only the declared query scope.

Read-only use needs discovery/search/read, or direct read of a supplied locator with narrower coverage disclosed. Persistent capture needs create/update plus read-back verification. No silent write-through to another provider.

## Duplicate and conflict handling

Compare identity, objective, scope, aliases, and sources, not title alone. Search the destination and related active/archive locations when accessible. An inaccessible known record is a blocker to duplicating that record, not a not_found result.

Before a write, retain the intended target and observed revision/content snapshot. If a create/update times out, inspect the same target or exact destination before retrying. Never assume retries are idempotent.

Use conditional updates when available. Otherwise re-read immediately before the write, reconcile changes, and modify only intended sections. This reduces but does not eliminate race conditions.

## Optional private config

The JSON example contains an adapter selection, locale hint, persistence policy, logical locations, and provider details. Null means undiscovered, never a usable identifier. Capabilities in config are informational and must be checked against live tools.

Read a config only from a user-provided path or an agreed project location. Store no tokens. Private Drive IDs belong in private config, not the distributed skill.

## Adding an adapter

Write one adapter reference mapping these operations to available tools, documenting search coverage, identity, concurrency, verification, and format-preservation rules. Extend routing in `SKILL.md` without changing the six core behaviors.
