# Workflow manifest / Канонический манифест workflow

This file is the **single human-and-agent source of truth** for the canonical story-role inventory and runtime semantics.

Other documents may explain the workflow, but they must not contradict this manifest. When a role is added, removed, renamed, or changes runtime/checkpoint semantics, update this file first, then run `docs/workflow-integrity-check.md`.

## Runtime vocabulary

- `same_chat` — may run in the current ChatGPT conversation when context is clean enough.
- `fresh_chat_recommended` — prefer a new ChatGPT conversation if the current one is long or biased by a conflicting role.
- `fresh_chat_required` — the default mobile-safe isolation boundary; start a new ChatGPT conversation for the role.
- `optional_child_agent` — environments that support real child agents may use one instead of a fresh manual chat, but this is never required.
- `diagnosis` — may inspect and recommend, must not directly rewrite literary prose.
- `sequential_prose` — edits prose and therefore must see the previous prose version.

## Canonical story roles

| id | alias | prompt | conditional | isolation | mode | author checkpoint |
| --- | --- | --- | --- | --- | --- | --- |
| 003 | `диспетчер` | `prompts/003-диспетчер--revision-router--маршрутизатор-правок.md` | route/start/resume | same_chat | routing | when choices block route |
| 005 | `приёмщик` | `prompts/005-приёмщик--idea-receiver--приёмщик-идеи.md` | normal new-story intake | same_chat | diagnosis/intake | yes |
| 010 | `архитектор` | `prompts/010-архитектор--idea-architect--архитектор-идеи.md` | normal | same_chat | diagnosis/design | no default |
| 015 | `тестер` | `prompts/015-тестер--criterion-stress-tester--тестер-критериев.md` | premise-defining rule/criterion | fresh_chat_recommended | diagnosis | when materially different survivors remain |
| 020 | `критик` | `prompts/020-критик--brutal-critic--жестокий-критик.md` | normal first-pass pressure | fresh_chat_required | diagnosis/opposition | yes |
| 030 | `сюжетник` | `prompts/030-сюжетник--story-engineer--инженер-сюжета.md` | normal | same_chat | design | no default |
| 040 | `психолог` | `prompts/040-психолог--character-psychologist--психолог-персонажей.md` | normal | same_chat | diagnosis/design | no default |
| 050 | `мировик` | `prompts/050-мировик--worldlogic-auditor--аудитор-логики-мира.md` | normal | fresh_chat_recommended | diagnosis/audit | no default |
| 060 | `тематик` | `prompts/060-тематик--thematic-analyst--тематический-аналитик.md` | normal | same_chat | diagnosis | yes |
| 070 | `черновик` | `prompts/070-черновик--draft-writer--писатель-черновика.md` | after pre-draft checkpoint | same_chat | sequential_prose | no default |
| 080 | `структурщик` | `prompts/080-структурщик--structural-editor--структурный-редактор.md` | normal after draft | same_chat | sequential_prose | no default |
| 090 | `стилист` | `prompts/090-стилист--style-editor--стилевой-редактор.md` | normal after structure | fresh_chat_recommended | sequential_prose | no default |
| 100 | `читатель` | `prompts/100-читатель--reader-simulator--симулятор-читателя.md` | normal review | fresh_chat_recommended | diagnosis | yes |
| 110 | `финалист` | `prompts/110-финалист--ending-analyst--аналитик-концовки.md` | advanced review | fresh_chat_recommended | diagnosis | no default |
| 120 | `идеолог` | `prompts/120-идеолог--ideology-stress-tester--идеологический-стресс-тестер.md` | advanced review | fresh_chat_required | diagnosis/opposition | no default |
| 130 | `предсказатель` | `prompts/130-предсказатель--predictability-analyst--аналитик-предсказуемости.md` | advanced review | fresh_chat_required | diagnosis/opposition | no default |
| 135 | `оригинальность` | `prompts/135-оригинальность--similarity-ip-auditor--аудитор-сходства-и-ip.md` | named/likely external-work overlap or pre-publication risk check | fresh_chat_required | diagnosis/web | conditional on high-risk findings |
| 140 | `сверщик` | `prompts/140-сверщик--continuity-auditor--аудитор-непрерывности.md` | normal late audit | fresh_chat_recommended | diagnosis/audit | yes |
| 150 | `финред` | `prompts/150-финред--final-editor--финальный-редактор.md` | normal final literary edit | same_chat | sequential_prose | yes |

## Normal pipeline

```text
003
-> 005
-> 010
-> 015? (conditional)
-> 020
-> 030
-> 040
-> 050
-> 060
-> 070
-> 080
-> 090
-> 100
-> 110
-> 120
-> 130
-> 135? (conditional/late)
-> 140
-> 150
-> manuscript_complete candidate
```

`003` may route around unnecessary roles during revisions. `015` and `135` are conditional and must not be mechanically forced into every story.

## Canonical checkpoints

Default human checkpoints exist after:

- `005`;
- `020`;
- `060`;
- `100`;
- `140`;
- `150`.

Conditional checkpoints:

- `015`: when materially different surviving rules require author selection;
- `135`: when high-risk overlap requires premise-, structure-, or ending-level changes;
- `003`: when route choices conflict or author preference is required.

## Story workspace operational files

Every active story should converge toward:

```text
00-input/
01-canonical/
02-handoffs/
03-drafts/
04-reviews/
05-exports/
06-agent-queue/agent-queue.md
06-agent-queue/story-status.md
```

The status file is a derived operational snapshot, not a replacement for canonical state or the queue.

## Integrity rule

The following must agree with this manifest:

- `README.md` role inventory;
- `docs/role-map.md`;
- `prompts/00-workflow.md`;
- `docs/story-project-structure.md`;
- `docs/story-inventory-index.md`;
- `docs/feedback-and-session-boundaries.md`;
- `docs/agent-queue.md`;
- `stories/_template/`;
- every canonical prompt filename referenced here.

Use `docs/workflow-integrity-check.md` after changes.
