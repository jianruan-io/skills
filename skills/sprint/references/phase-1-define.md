# Phase 1: Define the What (Steps 1 to 4)

**What:** Turn a rough intent into an approved spec whose done check someone else could run.

**Why:** This is the hard part. The how is getting cheap; knowing what good looks like is not. Everything after this phase is only as good as the done check written here.

**How:** AI asks the hard questions and suggests answers; the human fills in only what AI can't resolve. Two human decisions: it is worth doing, and the spec is approved.

---

## Step 1: Check if this is worth doing at all

**Does:** Decides whether this needs to be built, before any time goes into it.

**How:** Use `/first-principles` on the intent. Ask whether it already exists, whether a simpler way works, and what blocks it. Present exactly:

One paragraph of two or three plain sentences saying what problem it solves and why nothing simpler works.

**Recommendation: YES, BUILD** (or **NO, SKIP** with a one-line reason)

🛑 Confirm to proceed

---

## Step 2: Create a Linear issue, then a worktree and branch for it

**Does:** Gives the work an issue ID and an isolated place to happen.

**How:** Confirm main is checked out, clean, and synced with the remote. If not, 🛑 stop and say what to fix. Create the Linear issue in the team named in the project's `CLAUDE.md`, filling every field it requires. Then create the worktree and branch named after the issue, using the project's worktree command. Every later step runs inside that worktree.

---

## Step 3: Write a spec that defines what done means

**Does:** Writes down what good looks like before anything is built, and points to the domain skills that already define it.

**How:**

1. Name the artifact: what exists at the end (a feature, a rendered video, a landing page, a decision record).
2. Name the journey goal it serves from `/user-journey`. That goal is the top-level done check: its end-to-end test proves the work.
3. Find the domain skills that cover it:

   | Work | Domain skills |
   |---|---|
   | Journey, goal, step | `/user-journey` |
   | Screens and components | `/frontend`, `/design-system` |
   | Domain logic, APIs, integrations, workflows | `/backend` |
   | Schemas, migrations, pipelines, data quality | `/data` |
   | Proving a goal end to end | `/e2e-verify` |
   | Launch video | `/animation-video` |
   | Landing page | `/landing-page`, `/design-system` |
   | Positioning, channels, campaigns | `/gtm` |

   A landing page needs landing-page, design-system and frontend; a backend change needs backend and e2e-verify.
4. Write the done checks, each referencing the domain skill that defines it. If no domain skill covers part of the work, write the standard yourself and mark it `(no domain skill)`.
5. Write the rest from `references/spec-template.md`: Core Logic, file tree, and action items, each with one verify command and an exact expected result.

Write it to the project's spec location (default `specs/<issue-id>-<slug>/spec.md`). If a group of action items could ship and be reviewed on its own, split it into its own Linear issue.

---

## Step 4: Use /delete-algorithm to cut the spec down to what really matters

**Does:** Removes every action item that doesn't need to exist, so only the work that matters gets built.

**How:** Use `/delete-algorithm` steps 1 to 3. Name who required each item, delete what nothing depends on and log the reason for each cut, then simplify what survives. If nothing was cut, say so.

🛑 Print `Review: <absolute path>` and wait for explicit approval. The spec is locked from here.

---

## Exit condition

| | |
|---|---|
| Spec | written and approved by the human |
| Done check | names the artifact; each check references a domain skill or says `(no domain skill)` |
| Action items | every verify command is runnable with an exact expected result |
| Linear issue, worktree, branch | all exist |
