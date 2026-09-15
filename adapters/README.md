# Storage adapters

Second Brain keeps its behavioral core independent from the persistence provider.

The core works with conceptual storage operations defined in [`skills/second-brain/references/adapter-contract.md`](../skills/second-brain/references/adapter-contract.md). An adapter maps those operations to tools exposed by an authenticated app available to the agent.

## V1 — Notion

The first adapter is Notion. The ChatGPT plugin binds the official Notion app through the repository-level `.app.json`. Runtime mapping and safety rules live in [`skills/second-brain/references/adapter-notion.md`](../skills/second-brain/references/adapter-notion.md).

The adapter is intentionally declarative: there is no custom MCP server and no Notion token in this repository. Authentication and concrete tool schemas are provided by the connected app at runtime.

## Future adapters

A future provider (Google Drive, Obsidian, local files, another knowledge store) should implement the same conceptual contract rather than changing Capture, Consolidate, Recall, Organize, Plan, or Review behavior.

When adding an adapter:

1. Keep provider-specific IDs, schemas, authentication, and API semantics outside the behavioral core.
2. Add a provider reference beside `adapter-notion.md` describing capability mapping and limitations.
3. Bind the required app in `.app.json` when the provider is supplied as a ChatGPT app.
4. Preserve the persistence boundary: no write occurs without explicit user intent.
5. Never silently migrate or switch storage providers.
