# Story inventory index / Индекс рассказов

The private content repository should maintain a lightweight story index at:

```text
../knigi-content-private/inventory/story-index.md
```

The index is a navigation aid for humans and new ChatGPT sessions. It does not replace per-story `story-status.md`, `agent-queue.md`, canonical state, or handoffs.

The index covers story folders under `../knigi-content-private/stories/` only. Other private top-level workspaces may exist in the same repository; do not treat them as stories unless the user explicitly routes that material into `stories/<story-slug>/`.

## Purpose

Use the index to answer three questions quickly:

- which story folders exist;
- whether each folder is only captured input, an active pipeline workspace, blocked on the author, or a legacy/stale workspace;
- what the next safe action is before an agent reads or writes story material.

## Status vocabulary

- `capture_only` — raw idea or small note set; no active queue/status yet.
- `capture_rich` — multiple raw/development notes exist; triage is needed before specialist work.
- `structured_notes` — a dossier, canon-like notes, scenes, or fragments exist outside the current full pipeline structure.
- `seeded_concept` — a compact canonical seed exists, but no active queue/status exists yet.
- `active_pipeline` — current `story-status.md` and queue define the next role.
- `blocked_author` — current `story-status.md` or queue says author input is required.
- `legacy_pipeline_snapshot` — older full or partial workflow artifacts exist, but `story-status.md` is missing or stale.
- `manuscript_complete` — literary pipeline has reached the `150` completion boundary and final author checkpoint.

## Update rule

Update the private index when:

- a new story folder is created;
- capture-only material becomes an active pipeline workspace;
- `story-status.md` is created, removed, or materially changes current/next role;
- a story becomes blocked on the author;
- a legacy folder is reconciled with the current workflow;
- a manuscript reaches completion.

If the index conflicts with a story's own `story-status.md`, treat the index as stale and update it. Do not change canonical story state merely to match the index.

## Mobile use

When entering from ordinary ChatGPT/mobile with GitHub connector access:

1. Open the public workflow repo and read the runtime/manifest docs needed for the task.
2. Open `../knigi-content-private/inventory/story-index.md`.
3. Pick the exact `story-slug`.
4. If the row says `active_pipeline` or `blocked_author`, read that story's `06-agent-queue/story-status.md` next.
5. If the row says `capture_only`, `capture_rich`, `structured_notes`, or `seeded_concept`, route through `005-приёмщик` or `003-диспетчер` before specialist development.
6. Write all story outputs in the private repository.

The index should stay concise. It is a map, not another place to store story content.
