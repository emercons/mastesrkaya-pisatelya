# Agent operating rules

This repository is a sequential AI-assisted storytelling workshop.

## Repository pair

This repository is the public workflow repository. Treat it as process infrastructure: prompts, agent roles, templates, routing rules, handoff rules, and documentation.

The sibling private content repository is:

```text
../knigi-content-private/
```

That repository stores real story ideas, dictated notes, drafts, canonical state, handoffs, reviews, exports, and inventories.

When entering through GitHub or another connector, resolve the pair explicitly:

- `masterskaya-pisatelya` / `masterskaya-pisatelya-public/` = public workflow, agents-as-markdown, templates, docs.
- `knigi-content-private` = private story content.

Do not save story content in this repository. Use this repository to improve the workshop algorithm itself.

## Canonical sources

Role/runtime inventory source of truth:

```text
docs/workflow-manifest.md
```

Primary runtime contract:

```text
docs/mobile-chatgpt-runtime.md
```

Story isolation contract:

```text
docs/story-isolation-contract.md
```

Quality gates:

```text
docs/quality-gates.md
```

After changing role/runtime semantics, use:

```text
docs/workflow-integrity-check.md
```

## Primary runtime: ordinary ChatGPT

The workflow must be executable from a normal ChatGPT conversation, including the mobile app.

Assume no terminal, VM, local filesystem, background worker, real child-agent runtime, or parallel executor is available.

A new ChatGPT conversation is the baseline clean-context mechanism. Real child agents or parallel execution are optional enhancements only when the environment actually exposes them.

Never block normal mobile use merely because a richer runtime would be convenient.

## Main use cases

1. Capture a dictated idea: create or update files under `../knigi-content-private/stories/<story-slug>/`.
2. Develop a story: read prompts, role map, templates, and workflow docs here; write outputs in `../knigi-content-private/`.
3. Run specialist roles: use the current numbered prompt, current story state, queue/status, and focused inputs; write compact handoffs back to the story workspace.
4. Improve the algorithm: when author feedback reveals a recurring workflow, prompt, template, routing, state, or runtime problem, use `docs/framework-retrospective.md` and edit this public repository.
5. Apply updated workflow to private stories opportunistically when they are resumed.

Captured ideas may be lighter than a full story workspace. A private story folder with only `00-input/`, `00-raw-idea/`, or a few notes is an idea inbox, not a malformed pipeline run. Before specialist work, triage it through `005` or `003`, then add the needed canonical state, queue, handoffs, and `story-status.md`.

## Privacy boundary

This repository is public. Treat every concrete story idea, handoff, canonical state, draft, review, and export as private by default.

Real story workspaces live under:

```text
../knigi-content-private/stories/<story-slug>/
```

Do not create real story instances under public `stories/`. The public `stories/_template/` folder is only a template.

## Story isolation preflight

Before substantive specialist work, resolve:

- workflow repository;
- content repository;
- exact `story-slug`;
- exact specialist role/prompt.

Then verify queue/status/canonical state refer to the same workspace and keep story-specific reads/writes inside it unless the task explicitly requires cross-story work.

If project context already resolves these facts, do not make the author repeat them.

See `docs/story-isolation-contract.md`.

## Repository layout and migration safety

The public repository root should contain only local tool artifacts plus the public infrastructure subtree:

```text
masterskaya-pisatelya-public/
```

Private content is stored in a separate sibling repository:

```text
../knigi-content-private/
```

When working from the repository root, public infrastructure files live under `masterskaya-pisatelya-public/`. Interpret public workflow paths relative to that subtree. Interpret story-content paths under `../knigi-content-private/` unless the task explicitly names another content repository.

Do not assume private content belongs in this public repository because old paths/history mention `private/`, `masterskaya-pisatelya-PRIVATE/`, or concrete `stories/<story-slug>/` paths.

Before large moves or cleanup in a runtime that actually has git/local-shell capabilities:

- inspect git status/tracked paths/remotes;
- preserve history for tracked files;
- verify staged paths and diffs;
- preserve GitHub email privacy.

These shell checks are optional-runtime safety advice, not requirements for mobile ChatGPT operation.

## Priority order

1. `docs/workflow-manifest.md`
2. `prompts/00-workflow.md`
3. `prompts/003-диспетчер--revision-router--маршрутизатор-правок.md` when starting, resuming, or revising a route
4. `docs/role-map.md` when resolving a short agent alias
5. Current numbered bilingual specialist prompt in `prompts/`
6. Current story's `../knigi-content-private/stories/<story-slug>/06-agent-queue/story-status.md`, if present
7. Current story's `../knigi-content-private/stories/<story-slug>/06-agent-queue/agent-queue.md`, if present
8. Current story's `../knigi-content-private/stories/<story-slug>/01-canonical/canonical-story-state.md`
9. Latest relevant handoff in `02-handoffs/`
10. Relevant draft/review fragment

Do not read the whole repository unless the current task requires it.

For short commands such as `работай, критик`, resolve the alias through the manifest/role map and use only that prompt plus the focused story recovery set.

## Canonical prompt naming

Use only bilingual role prompt filenames registered in `docs/workflow-manifest.md`:

```text
prompts/NNN-короткое--english--русский.md
```

Do not use non-bilingual role prompt filenames or unregistered prompt files as canonical story-role definitions.

## Role reset

Before every specialist role:

```text
You are ONLY the currently assigned specialist.
Forget previous specialist roles.
Do not continue previous analytical styles.
Do not behave as a general assistant.
Your scope is intentionally narrow.
```

## Fresh-chat isolation

For roles marked `fresh_chat_required` by the manifest, a new ordinary ChatGPT conversation is the canonical baseline.

If such a role is next and the current session is not already a clean launch of that role, stop at the boundary and give the exact alias/filename for the next chat.

For `fresh_chat_recommended`, continue in the same chat only when context is short, focused, and not biased by a conflicting role.

A real child agent may substitute for a fresh manual chat only when the environment actually supports it.

## Optional enhanced execution

Environments with real child agents may use them for oppositional/diagnosis roles, especially `015`, `020`, `120`, `130`, and `135`, or for large isolated audits.

Diagnosis-only reviews may run in parallel only when they read the same stable draft and write disjoint outputs.

Prose-editing roles must remain sequential because each editor must see the previous edited draft.

None of this enhanced execution is required for normal ChatGPT/mobile use.

## Stable paragraph IDs for review drafts

When exporting or presenting a full draft for human author review, add stable paragraph IDs inline at the start of each numbered story paragraph:

```text
100: First paragraph text.

200: Second paragraph text.
```

Rules:

- Number paragraphs, not visual lines.
- Preserve existing IDs across agent passes.
- When a paragraph moves, keep its ID.
- When deleted, retire its ID.
- When merged, keep the semantic-core ID and retire absorbed IDs.
- When split, keep the original ID on the most continuous fragment and assign new IDs to new fragments.
- Insert within numeric gaps when possible (`150` between `100` and `200`).
- If gaps are exhausted, use textual dot suffixes such as `100.1`, `100.2`.
- Never globally renumber without explicit author consent.
- Clean publication/reader-facing copies without paragraph IDs must be separate files.

See `docs/stable-paragraph-ids.md`.

## Handoff and status rule

After every role:

- save compact output under `../knigi-content-private/stories/<story-slug>/02-handoffs/`;
- update canonical story state only when durable decisions changed;
- update draft/review files only when the role is allowed to;
- update the story-specific agent queue when running a queued route;
- update `06-agent-queue/story-status.md` when current/next role, checkpoint, latest draft/review, fresh-chat requirement, or manuscript state changed;
- carry forward a short summary, not the whole prior conversation.

## Quality gates

A role is not complete merely because it produced a handoff.

Use `docs/quality-gates.md`. Record `PASS`, `PASS_WITH_KNOWN_RISKS`, `BLOCKED`, or `AUTHOR_DECISION_REQUIRED` when the role has a defined gate.

Blocking failures must reroute or stop downstream work rather than being buried in prose.

## Human feedback and session boundaries

Use `docs/feedback-and-session-boundaries.md` for author checkpoints and high-conflict transitions.

After complex author feedback, or when resuming a partial pipeline in a new session, run `003` before rewriting. Skip it only for narrow obvious fixes.

## Framework feedback loop

When repeated story work exposes a workflow defect, use `docs/framework-retrospective.md` to distinguish story-specific repair from a public framework change.

Do not add a new permanent specialist merely because one story produced one unusual problem if an existing role can own it.

## Manuscript vs publication

`150` ends the literary editing pipeline. After author review and required quality gates, the result may be marked `manuscript_complete`.

Do not treat that as route-specific `publication_ready`.

Publishing/submission and promotion are future sibling frameworks described in `docs/post-manuscript-frameworks.md`. If those workflows later require literary change, route it back through `003`.

## Do not

- mix roles;
- perform general literary analysis instead of the current role;
- silently invent missing prompt files;
- rewrite artistic prose without role-specific need;
- place story content in public tracked folders;
- read sibling stories without a task-specific reason;
- flatten strange/conflicting elements into generic LLM prose;
- make AI jargon the whole story;
- require unavailable child agents/VMs/background execution for baseline operation;
- equate `manuscript_complete` with publication readiness.
