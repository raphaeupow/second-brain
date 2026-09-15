# Storage adapter contract — V1

This is a behavioral interface interpreted by an agent, not an SDK, API server, or executable dispatcher. Core behaviors use logical records. Only adapters interpret provider-specific identifiers, property types, query semantics, and write mechanisms.

## Logical record

Use these concepts where applicable, without requiring a matching database column for each:

| Field | Meaning |
|---|---|
| id, locator | Stable provider identity and retrievable link; opaque to the core |
| title, aliases | Human name and alternative search terms |
| kind | project, area, resource, task, or note |
| placement | projects, areas, resources, archive, or inbox |
| body | Useful content, preferably readable without this skill |
| state | Existing workflow state, mapped without inventing options |
| sources | Available source links, dates, and attribution; no invented chat URLs |
| updated_at, revision | Provider revision or observed timestamp/content snapshot |
| relations | Related record identifiers, when supported |

Archive is placement, preserving the original kind. Tasks belong to a project or area where possible; Inbox is a staging queue outside the four PARA categories. Absent fields remain absent or unknown, not fabricated. Body headings can represent metadata when structured fields are unavailable.

## Operations

All operations accept the selected workspace/root scope. Names below are conceptual.

| Operation | Input | Output / fallback |
|---|---|---|
| discover | user-selected scope | identity, capabilities, roots, access limits |
| inspect_schema | container locator | actual fields, types, options, revision |
| search | query, scope, cursor | candidates, next cursor, coverage and limitations |
| read | locator | record, revision, completeness; fetch nested/paginated content |
| create | destination, logical record | locator and outcome; reconcile uncertain results |
| update | locator, patch, observed revision | outcome and revision; protect concurrent changes |
| relocate | locator, logical placement | reversible classification or unsupported |

Represent outcomes as success, not_found, permission_denied, unsupported, conflict, retryable_failure, or unknown_outcome. A connector may use different labels; interpret its actual result rather than manufacturing this envelope. Search coverage is scoped_complete, partial, or unknown; scoped_complete means the declared query scope only.

Read-only use needs discovery/search/read, or direct read of a supplied locator with the narrower coverage disclosed. Persistent capture needs create or update plus read-back. Schema inspection can be ordinary page inspection. Optional capabilities include relations, structured queries, conditional updates, and relocation. Emulate missing structured metadata with readable body sections only when it preserves semantics. No silent write-through to another provider.

## Duplicate and conflict handling

Compare identity, objective, scope, aliases, and sources, not title alone. Search the destination and related active/archive locations when accessible. An inaccessible known record is a blocker to duplicating that record, not a not_found result. When search coverage remains insufficient, ask for a locator or scope decision and keep the draft in conversation.

Before a write, retain the intended target, change, and any provider operation ID in working memory. If a create times out, inspect its returned ID or search the exact destination and compare the attempted content. Never assume retries are idempotent. If reconciliation cannot determine whether it committed, report unknown_outcome and stop retrying that mutation. No persistent retry journal is required in V1.

Use conditional updates when available. Otherwise compare a fresh read with the observed snapshot and patch only intended sections. This reduces risk but cannot eliminate the race between read and write; V1 offers no transaction or concurrency guarantee. Verify each step of multi-record operations and report committed and pending parts separately.

## Optional private config

The JSON example has config_version=1, a locale hint, the explicit persistence policy, logical locations, and provider details under adapters. Null means undiscovered, never a usable identifier. capabilities_observed is informational and must be checked against live tools; config cannot grant capabilities or permission. Record actual provider locators and actual field mappings after discovery.

Read a config only from a user-provided path or an agreed project location. Suggested private name: second-brain.config.local.json. Do not search the whole computer for configs. A copy is optional; the agent can rediscover mappings each session. Saving a config itself requires a requested setup/save location. Store no tokens. Reject unsupported config versions with a clear explanation, preserving the file.

## Adding an adapter

Write one adapter reference mapping these operations to available tools, documenting search coverage, schema inspection, identity, concurrency, and verification. Run the behavioral cases in the repository's acceptance guide. Extend the adapter routing in SKILL.md and add a config example only as needed. Core logical fields and behaviors should stay unchanged. Future backends are extension points, not supported integrations.
