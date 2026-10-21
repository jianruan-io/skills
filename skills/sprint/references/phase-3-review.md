# Phase 3: Review the Proof (Steps 8 to 10)

**What:** Show the result meets the spec, get it merged, and raise the standard for next time.

**Why:** Mapping each action item to the result turns review into a checklist instead of a full read.

**How:** Two human decisions: approve the result, and merge.

---

## Step 8: Show where the result meets each action item in the spec

**Does:** Gives the human everything they need to say yes or no.

**How:** Read the locked spec and the full diff. Mark each action item as present, partial (say what's missing), or absent. Then check the reverse: flag any change no action item asked for as extra.

```
━━━ Sprint: [Issue Title] ━━━━━━━━━━━━━━━━━━━━━━━━━

Action items and where they are done
  ✅  [item] in [file:line]
  ⚠️  [item] in [file:line], missing [what]
  ❌  [item] not found

Changes the spec did not ask for
  ⚠️  [file:line]: [description]
  (none)

Evals from eval.md
  ✓  [test or measurement], from [domain skill]: [result]
  ☐  [checklist question], for the human to tick

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Approve? Jump to any flagged row, or ask for the full diff.
```

🛑 Wait for the human to tick every checklist item and approve. If rejected, ask what's wrong and go back to step 7. The spec only reopens as a new sprint.

---

## Step 9: Open a PR and answer every automated review comment

**Does:** Puts the work up for merge and makes sure no review comment goes unanswered.

**How:** Open the PR with the title from the Linear issue, a body from the spec's What and Why plus the result map, and the issue linked. If the project has an automated reviewer, wait for it, then decide each comment:

- A real bug or security issue gets a **Fix**.
- A conflict with a decision in the spec gets a **Dismiss** that cites the spec.
- A style preference gets a **Dismiss** that says why.
- A valid point outside the spec gets a **Dismiss** and a note as a future issue.

🛑 Show the fix and dismiss table and wait for approval. Apply the fixes, re-run the checks, and commit. Post the table on the PR as a record.

🛑 The human merges. Never merge yourself.

---

## Step 10: Run a retro that turns every miss into a new standard in a domain skill

**Does:** Makes the next sprint start from a higher standard than this one.

**How:** Run `references/retro.md`. Then mark the Linear issue done with `Verified. PR: [url]`, remove the worktree, delete the branch locally and on the remote, switch back to main, and pull. Ask: "Next issue, or new intent?"

---

## Exit condition

| | |
|---|---|
| Result map | every action item present, approved by the human |
| Automated review | every comment fixed or dismissed, record posted |
| PR | merged by the human |
| Retro | run; Linear issue done; worktree and branch removed |
