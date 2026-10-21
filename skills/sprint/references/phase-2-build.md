# Phase 2: Build the How (Steps 5 to 7)

**What:** Turn each action item into an eval, prove the evals fail, then build until they pass.

> **Scope:** Only create or change files named or implied by an action item. Everything else is read-only.

**Why:** Evals written before the build test the intent. Evals written after test whatever got built.

**How:** One human decision at the start, eval.md. Everything after runs on its own.

---

## Step 5: Write eval.md with one eval for every action item in the spec

**Does:** Decides how each action item will be proven, before any work starts. The spec says what done means; eval.md says how each part of it is proven, in every domain.

**How:** Confirm you are inside the sprint's worktree on its branch. Derive eval.md from the spec only, without reading existing code first. For every action item, name one eval and what it asserts, using the verify method of the domain skill the spec references. Pick the strongest kind that works, and group eval.md by kind:

```markdown
# Evals: [Issue Title]

## Tests (automated)
- [ ] [action item]: [test name] asserts [what]

## Measured
- [ ] [action item]: [metric] is [threshold], e.g. Lighthouse 90 or more, screenshot diff 1% or less, render 60 seconds

## Checklist (human)
- [ ] [action item]: [question the human answers yes or no at review], only when nothing above works
```

Leave out any section with no items. Write eval.md next to spec.md. If every action item is config or wiring with no logic, skip eval.md and use the spec's verify commands as the evals.

🛑 Print `Review: <absolute path>` and wait for approval. If rejected, revise and show it again.

---

## Step 6: Write the evals and confirm they all fail

**Does:** Proves each eval actually checks something.

**How:** Write every test and measurement script from the approved eval.md and run them. Every new one must fail. Checklist items fail by definition until the artifact exists. If one passes before the build, it asserts the wrong thing, so fix it first.

---

## Step 7: Build until every eval passes

**Does:** Does the minimum work that makes every eval pass.

**How:** Build, then run every test and measurement plus the project's own checks (test, build, lint, from `CLAUDE.md`). Repeat until all pass. Clean up without changing behaviour, re-running the evals after every change. Mark each action item `[x]` in the spec. Commit with the Linear issue ID in the message, then go straight to Phase 3.

---

## Exit condition

| | |
|---|---|
| eval.md | approved by the human before the build |
| Tests and measurements | failed first, now all pass |
| Checklist | ready for the human at review |
| Project checks | all pass |
| Spec | every action item marked `[x]` |
