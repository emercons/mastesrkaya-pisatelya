# Workflow integrity check / Проверка целостности workflow

This is a **ChatGPT-runnable validator**. It is intentionally usable from the mobile app and does not require a shell, CI runner, or local clone.

## Launch

In a normal ChatGPT conversation with GitHub access:

```text
Проверь целостность Мастерской писателя по docs/workflow-integrity-check.md. Ничего не меняй до отчёта.
```

After reviewing the report, the author may ask the same chat to repair the public infrastructure.

## Source of truth

Start with:

```text
docs/workflow-manifest.md
```

Do not infer the canonical role list from README or from directory order.

## Checks

### 1. Prompt existence

For every role in the manifest:

- exact prompt file exists;
- numeric ID, alias, English name, and Russian name are consistent enough to resolve unambiguously;
- no referenced canonical prompt is missing.

### 2. Role inventory drift

Compare manifest against:

- `README.md`;
- `docs/role-map.md`;
- `prompts/00-workflow.md`;
- `docs/story-project-structure.md`;
- `docs/feedback-and-session-boundaries.md`;
- `docs/agent-queue.md`.

Report missing roles, stale roles, different ordering, conditionality mismatches, checkpoint mismatches, and isolation/runtime mismatches.

### 3. Mobile-runtime safety

Search public workflow text for assumptions that make ordinary ChatGPT unusable, especially claims that:

- a real child agent is mandatory;
- parallel execution is mandatory;
- a VM/terminal/local filesystem is required;
- background execution is required.

`fresh_chat_required` may be mandatory. `child_agent` may only be optional enhancement.

### 4. Story template coverage

Check `stories/_template/` against `docs/story-project-structure.md` and manifest expectations.

At minimum verify:

- canonical state files;
- handoff namespace includes current conditional roles `015` and `135` where placeholders are enumerated;
- `06-agent-queue/agent-queue.md` exists;
- `06-agent-queue/story-status.md` exists;
- README references current runtime/status docs.

### 5. Quality-gate wiring

Verify `docs/quality-gates.md` exists and that roles `015`, `020`, `050`, `100`, `135`, and `140` either embed or explicitly reference their gate.

Verify `150` does not equate literary final edit with route-specific publication readiness.

### 6. Story isolation wiring

Verify public operating rules and router reference `docs/story-isolation-contract.md` or enforce equivalent repository/slug/path checks.

### 7. Story inventory index wiring

Verify `docs/story-inventory-index.md` exists and that public operating rules explain when to read the private `../knigi-content-private/inventory/story-index.md`.

The private index is derived navigation state. It must not be treated as canonical story state when it conflicts with a story's own `story-status.md`.

### 8. Post-manuscript boundary

Verify the writing workflow ends at `manuscript_complete` candidate/final literary checkpoint and points to `docs/post-manuscript-frameworks.md` rather than inventing publishing/SMM roles inside the writing chain.

### 9. Internal links

Check that all newly canonical docs referenced by README/AGENTS/workflow actually exist.

## Severity

Classify findings:

- `FATAL` — could write private story content to public repo, mix stories, lose/overwrite canon, or route using a nonexistent/wrong role.
- `MAJOR` — different public sources give materially different workflow/runtime instructions.
- `MINOR` — stale explanatory text with low operational risk.
- `COSMETIC` — wording/format only.

## Output

```markdown
# Workflow integrity report

## Verdict
PASS | PASS_WITH_WARNINGS | FAIL

## Findings

| id | severity | file(s) | mismatch | source-of-truth answer | repair |
| --- | --- | --- | --- | --- | --- |

## Role inventory comparison

## Mobile-runtime findings

## Template findings

## Quality-gate findings

## Isolation/privacy findings

## Story inventory index findings

## Post-manuscript-boundary findings

## Recommended patch order

## Files that should NOT be changed
```

## Repair rule

When repairs are authorized:

1. update manifest first only if the intended canonical semantics themselves changed;
2. otherwise treat manifest as fixed and repair downstream docs/templates/prompts;
3. do not touch private story content unless explicitly asked;
4. rerun this check after repairs;
5. report residual warnings instead of claiming success without verification.
