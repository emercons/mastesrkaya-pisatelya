# Story project structure

This repository is public infrastructure for AI-assisted storytelling. Concrete stories are private by default and live in a separate private repository.

Canonical role/runtime inventory: `docs/workflow-manifest.md`.

## Real story location

Every real story lives under:

```text
../knigi-content-private/stories/<story-slug>/
```

The public `stories/_template/` folder is only a copy source.

## Required/private story structure

## Capture-only vs active pipeline workspaces

Mobile/voice capture may initially create a minimal story folder with only `00-input/`, `00-raw-idea/`, or a few note files. That state is allowed and should not be treated as a broken full workspace.

Before specialist development begins, route the story through `005-приёмщик` or `003-диспетчер` as appropriate and converge the folder toward the active pipeline structure below. At that point, create or update `06-agent-queue/story-status.md` so the next ordinary ChatGPT session can resume from files instead of chat memory.

Do not bulk-create empty canonical/handoff/draft/review files for a captured idea just to satisfy the template. Create operational files when they have real state to preserve.

```text
../knigi-content-private/stories/<story-slug>/00-input/raw-idea-from-chat.md
../knigi-content-private/stories/<story-slug>/00-input/author-notes.md

../knigi-content-private/stories/<story-slug>/01-canonical/canonical-story-state.md
../knigi-content-private/stories/<story-slug>/01-canonical/decisions-log.md

../knigi-content-private/stories/<story-slug>/02-handoffs/003-диспетчер--revision-router--маршрутизатор-правок.md
../knigi-content-private/stories/<story-slug>/02-handoffs/005-приёмщик--idea-receiver--приёмщик-идеи.md
../knigi-content-private/stories/<story-slug>/02-handoffs/010-архитектор--idea-architect--архитектор-идеи.md
../knigi-content-private/stories/<story-slug>/02-handoffs/015-тестер--criterion-stress-tester--тестер-критериев.md
../knigi-content-private/stories/<story-slug>/02-handoffs/020-критик--brutal-critic--жестокий-критик.md
../knigi-content-private/stories/<story-slug>/02-handoffs/030-сюжетник--story-engineer--инженер-сюжета.md
../knigi-content-private/stories/<story-slug>/02-handoffs/040-психолог--character-psychologist--психолог-персонажей.md
../knigi-content-private/stories/<story-slug>/02-handoffs/050-мировик--worldlogic-auditor--аудитор-логики-мира.md
../knigi-content-private/stories/<story-slug>/02-handoffs/060-тематик--thematic-analyst--тематический-аналитик.md
../knigi-content-private/stories/<story-slug>/02-handoffs/070-черновик--draft-writer--писатель-черновика.md
../knigi-content-private/stories/<story-slug>/02-handoffs/080-структурщик--structural-editor--структурный-редактор.md
../knigi-content-private/stories/<story-slug>/02-handoffs/090-стилист--style-editor--стилевой-редактор.md
../knigi-content-private/stories/<story-slug>/02-handoffs/100-читатель--reader-simulator--симулятор-читателя.md
../knigi-content-private/stories/<story-slug>/02-handoffs/110-финалист--ending-analyst--аналитик-концовки.md
../knigi-content-private/stories/<story-slug>/02-handoffs/120-идеолог--ideology-stress-tester--идеологический-стресс-тестер.md
../knigi-content-private/stories/<story-slug>/02-handoffs/130-предсказатель--predictability-analyst--аналитик-предсказуемости.md
../knigi-content-private/stories/<story-slug>/02-handoffs/135-оригинальность--similarity-ip-auditor--аудитор-сходства-и-ip.md
../knigi-content-private/stories/<story-slug>/02-handoffs/140-сверщик--continuity-auditor--аудитор-непрерывности.md
../knigi-content-private/stories/<story-slug>/02-handoffs/150-финред--final-editor--финальный-редактор.md

../knigi-content-private/stories/<story-slug>/03-drafts/draft-v0-raw.md
../knigi-content-private/stories/<story-slug>/03-drafts/draft-v1-after-070-черновик.md
../knigi-content-private/stories/<story-slug>/03-drafts/draft-v2-after-080-структурщик.md
../knigi-content-private/stories/<story-slug>/03-drafts/draft-v3-after-090-стилист.md
../knigi-content-private/stories/<story-slug>/03-drafts/draft-v4-final-candidate.md
../knigi-content-private/stories/<story-slug>/03-drafts/draft-v5-after-150-финред.md

../knigi-content-private/stories/<story-slug>/04-reviews/reader-notes.md
../knigi-content-private/stories/<story-slug>/04-reviews/ending-analysis.md
../knigi-content-private/stories/<story-slug>/04-reviews/ideology-stress-test.md
../knigi-content-private/stories/<story-slug>/04-reviews/predictability-analysis.md
../knigi-content-private/stories/<story-slug>/04-reviews/similarity-ip-audit.md
../knigi-content-private/stories/<story-slug>/04-reviews/continuity-audit.md
../knigi-content-private/stories/<story-slug>/04-reviews/author-retrospective.md

../knigi-content-private/stories/<story-slug>/05-exports/full-draft-v<N>-MM-DD.md

../knigi-content-private/stories/<story-slug>/06-agent-queue/agent-queue.md
../knigi-content-private/stories/<story-slug>/06-agent-queue/story-status.md
```

Conditional role handoff/review files may remain absent until the role is actually invoked. Do not create meaningless empty outputs solely for completeness.

## Why filenames do not include story title

The story title/slug belongs to the parent folder. Stable internal filenames keep routing simple and reduce accidental cross-story reads.

## Story isolation

Before reading/writing story files, follow `docs/story-isolation-contract.md`.

Do not read sibling story folders unless the current task explicitly requires cross-story work.

## Export naming

Full-draft exports must be versioned:

```text
../knigi-content-private/stories/<story-slug>/05-exports/full-draft-v<N>-MM-DD.md
```

Do not use unversioned `final.md` as the source of truth.

Author-review exports should use stable paragraph IDs according to `docs/stable-paragraph-ids.md`. Never globally renumber without explicit author consent.

## Core role rule

Each specialist receives only focused inputs:

- current specialist prompt;
- story status/queue when needed;
- current canonical story state;
- latest relevant handoff;
- relevant draft/review fragment;
- explicit author decisions;
- external research only when the role requires it.

The role writes a compact handoff before the next role begins.

## Canonical story state

The canonical story state contains durable story decisions, not every brainstormed option.

Update it when durable decisions change premise, protagonist, conflict, setting/world rules, ending direction, or draft-level facts.

## Agent queue

`06-agent-queue/agent-queue.md` stores the durable route across chats.

It should use mobile-safe chat isolation semantics from `docs/agent-queue.md` and `docs/workflow-manifest.md`.

## Story status

`06-agent-queue/story-status.md` is a concise derived snapshot for the author/new chat:

- current phase;
- last/current/next role;
- block/checkpoint state;
- author action required;
- latest draft/review/handoff;
- fresh-chat requirement;
- next launch command;
- manuscript/publication state.

See `docs/story-status.md`.

## Quality gates

Roles with defined quality gates must satisfy `docs/quality-gates.md` before being treated as safely complete.

Blocking findings reroute or stop the pipeline.

## Handoffs

Handoffs prevent role contamination. They are compact summaries, not transcripts.

A good handoff contains:

- what the role did;
- durable decisions/findings;
- unresolved questions;
- gate verdict when applicable;
- next-role instructions;
- warnings about what not to flatten/normalize.

## Mobile/new-chat continuation

A new ordinary ChatGPT conversation is sufficient for required clean-context roles. The new chat reads story status, queue, canonical state, current prompt, and focused handoff/draft inputs rather than the old transcript.

See `docs/mobile-chatgpt-runtime.md` and `docs/feedback-and-session-boundaries.md`.

## Manuscript completion

After `150` plus final author checkpoint and resolved blocking gates, the story may be marked `manuscript_complete`.

Publication/submission/promotion readiness is separate. See `docs/post-manuscript-frameworks.md`.

## Prompt naming

Use only canonical bilingual prompt filenames registered by `docs/workflow-manifest.md`:

```text
prompts/NNN-короткое--english--русский.md
```
