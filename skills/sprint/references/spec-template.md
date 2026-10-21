# Spec Template

Write the spec in plain English. The Overview must make sense to someone who knows the goal but not the tools — no file paths, library names, or config in it.

## Rules

- **Done check before Action Items.** The artifact and how it will be judged come first; the Action Items exist to meet it.
- **Every done check cites its source.** The domain skill it came from, or `(no domain skill)`.
- **Core Logic is specific.** Every node names something in this piece of work; group nodes into labeled subgraphs; state what must always and never hold.
- **Action Items say what, never how.** One sentence each: the file or artifact, and what it contributes. No code, no signatures.
- **Every Verify is runnable.** A pasteable command with an exact expected result. If one command can't verify an item, split the item.

## Template

````markdown
# [Title]

## Overview

**What:**
[what someone can do after this that they couldn't before]

**Why:**
[the problem or gap this closes]

**How:**
[the approach in one or two sentences]

---

## Done Check

**Artifact:** [what exists at the end — a PR, a rendered video, a decision record, a page]

| Check | Proof | Source |
|---|---|---|
| [what good means, stated so someone else can judge it] | [automated test, measured comparison, or human checklist] | [domain skill, or (no domain skill)] |

---

## Core Logic

```mermaid
[flowchart grouped into labeled subgraphs; graspable in 30 seconds]
```

- Always: [what holds at every step]
- Never: [what means something went wrong]

---

## File Tree

```
[every file created or changed — one comment per line on its role]
```

---

## Action Items

**[ ] [one-line title]**

Implement: [one sentence — the file or artifact, and what it contributes]

Verify:
```
[exact command]
```
→ [exact expected result]
````
