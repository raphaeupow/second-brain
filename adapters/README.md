# Storage adapters

Second Brain keeps its behavioral core independent from the persistence provider.

The core works with conceptual storage operations defined in [`skills/second-brain/references/adapter-contract.md`](../skills/second-brain/references/adapter-contract.md). An adapter maps those operations to tools exposed by an authenticated app available to the agent.

## Current adapter — Google Drive

The active adapter is Google Drive. The ChatGPT plugin binds the Google Drive connector through the repository-level `.app.json`.

Runtime mapping, Markdown/YAML preservation, search coverage, concurrency rules, and verification live in [`skills/second-brain/references/adapter-google-drive.md`](../skills/second-brain/references/adapter-google-drive.md).

The persistent data model is a portable Markdown vault, normally rooted at `Segundo_Cerebro`. The adapter preserves raw Markdown and uses Drive files/folders instead of databases.

There is no custom MCP server and no Google token in this repository. Authentication and concrete tool schemas are provided by the connected app at runtime.

## Legacy Notion reference

`adapter-notion.md` may remain in repository history as a legacy implementation reference, but the current skill does not route to Notion and the current plugin does not declare Notion as a dependency.

## Adding another adapter

When adding an adapter:

1. Keep provider-specific IDs, schemas, authentication, and API semantics outside the behavioral core.
2. Add a provider reference beside `adapter-google-drive.md` describing capability mapping and limitations.
3. Bind the required app in `.app.json` when the provider is supplied as a ChatGPT app.
4. Preserve the persistence boundary: no write occurs without explicit user intent.
5. Never silently migrate or switch storage providers.
