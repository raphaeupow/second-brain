# Capture and consolidation

## Capture

For an explicit quick save, preserve the essential idea with a useful title, short body, available attribution/source, and placement. Search the verified Google Drive vault first.

If a clear matching Markdown record exists, update the relevant file without duplicating it. Otherwise create a raw `.md` file in the appropriate canonical folder; use `00 - Caixa de Entrada` only when classification is unclear.

Never convert a Markdown vault item into a native Google Doc as a write shortcut.

## Consolidate

Read the current Drive file and relevant supplied conversation before drafting. If older context is unavailable, describe that boundary; do not imply the whole history was consolidated.

Use applicable sections, omitting empty ones:

- Objective and success criteria.
- Context and constraints.
- Current state: distinguish observed facts from reported status.
- Decisions and rationale, with dates/sources when available.
- Approach or architecture when relevant.
- Next actions, with owners/deadlines only if established.
- Open questions, assumptions, and unresolved alternatives.
- Short change history and source references.

Merge durable knowledge, remove conversational repetition, and preserve nuance. An idea is not a decision; a proposal is not a commitment; a discussed action is not completed work. When a new explicit decision supersedes an old one, update current state and preserve a brief dated history. If sources conflict without resolution, retain both as an open question.

Repeated consolidation with no new information should be a no-op with an explanation, not another identical appended summary.

## Task records

Creating separate tasks requires an instruction to do so. Suggested next actions may remain in the saved summary.

When task creation is authorized:

1. Inspect `50 - Tarefas` and search for an existing matching task.
2. Create one raw Markdown file per task with YAML frontmatter.
3. Link Project and Area with the vault's existing wiki-link conventions when clear.
4. Do not invent deadline, owner, priority, completion, or urgency.
5. Preserve existing task status labels. For new tasks, default only when needed to Backlog, Em andamento, Impedido, or Concluído; empty status is acceptable.
6. If `50 - Tarefas/README.md` exists, update its index after the canonical task write and report partial failure separately.

## Safe raw-file update

Re-fetch the target immediately before replacement. Build the complete UTF-8 Markdown content from that fresh version, preserving unknown YAML keys and unrelated body sections. Replace the raw file in place using the same Drive file ID when possible, then fetch again to verify.

If raw replacement cannot be performed with available host capabilities, return a draft marked **not saved**. Do not silently change file format.

## Receipt

Return the Drive destination link and a concise change summary. Distinguish verified saved, reported write with verification unavailable, not saved, and partial completion. Never claim durable memory from a draft displayed only in chat.
