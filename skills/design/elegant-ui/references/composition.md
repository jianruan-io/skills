# Composition

This is the pillar that runs *before* Color/Typography/Grid/Iconography decide how anything looks. It's actually three distinct questions with three different owners — treating them as one blended "does this look right" judgment call is how they get conflated. Answer them in order.

## 1 — Does this feature need to exist at all?

**Not this skill's job.** This is Jobs-to-be-Done / essential-vs-accidental-complexity territory (Christensen, Fred Brooks' "No Silver Bullet") — finding the real job the user is hiring the product to do, before designing anything. Use `/first-principles` (lock the problem → question every belief → find bedrock facts → build only from those) — or any necessity check ("does this need to be built? already exists? simpler alternative?"). If this question hasn't been answered yet, answer it first — don't let a styling skill improvise a substitute.

## 2 — Which existing pattern answers this job?

This is the semantic layer, and it maps directly onto **Atomic Design** (Brad Frost): atoms are the smallest building blocks (Button, Input, Checkbox, Card, Select), organisms are complete patterns built from them and **named by job, not by shape** — `metric-card`, `status-badge`, `data-table`, `app-shell`, `onboarding-wizard`.

**The workflow:** before building anything new, check the component library (e.g. Storybook) for an organism that already answers the job. If one fits, use it. If nothing fits, that's a deliberate, documented decision — add the new organism to the library with its own story — not a silent one-off built for a single screen.

**A catalogue of AI-response shapes — reasoned from beautifului.dev's harness, for when an AI feature grows past plain text:**

| Job | Nearest named shape |
|---|---|
| Confirm the request registered, nothing to show yet | Loading state (a shimmer shaped like the eventual content) |
| Narrate the agent's intermediate steps before a final answer | Thinking (named steps, not a generic spinner) |
| Reveal a long answer as it's generated | Streaming text |
| Require human confirmation before a consequential action | Approval card |
| Show which tool/data source the agent used, without raw call JSON | Tool chip |
| Track one discrete unit of multi-step agent work | Task row |
| Suggest a next question when the input is empty | Recommendation card |
| Surface a real linked record relevant to the conversation | Context card |
| Show exactly what changed, field by field | Diff table |
| Answer a data question with data | Records table (already proven — the 60-row supplier grid) |
| Same, with narrowing controls for a large dataset | Filter table |
| Surface one notable finding the agent proactively noticed | Insight card (a conclusion, not raw retrieval) |
| Bulk-act on multiple selected rows | Selection actions |

Each row exists because it's the cheapest correct representation for one specific job — never a generic wrapper reused for five different jobs.

**Four of these rows are no longer just reasoned — they've been clicked through and measured live on `/harness`:**

- **Thinking** — a compact row above the response, icon + label + a live elapsed-time counter (`Thinking 0.0s`, ticking), not a generic spinner. It resolves in place into the actual response; it never blocks the composer.
- **Approval card** — a bordered card (`--shadow-sm`, `22px`-ish rounded corners scaled to the card's size) containing: a one-line question ("Want me to place this restock order?"), the proposed action with an inline entity chip (`Cone King`, colored avatar + name, no border — just colored text/icon) and a status pill (`7 days`, `bg-green-tint text-green`, full pill), a confidence indicator (a small bar-chart glyph + `High confidence` label), and two full-pill buttons side by side — a filled primary (`Accept`, `--accent` background, white text) and an outline secondary (`Alternatives`, white background, `--ink` text, hairline border). This is the concrete shape for "require human confirmation before a consequential action" — not a generic modal, an inline card the user can act on without leaving the message flow.
- **Task row** — lives in a persistent side panel (not inline in the chat), not the message thread. Each row is a short label with a leading status glyph: a filled green check for done, a numbered circle for not-yet-started, a spinning ring for in-progress. This is the concrete shape for "track one discrete unit of multi-step agent work" — plural task rows read as a checklist, and the panel stays visible across the whole conversation rather than scrolling away with old messages.
- **Tool chip / Context card** — the same side panel has an "All chunks" section below the task list: small cards, each with a colored file-type badge (e.g. a red "PDF" tag, a green "CSV" tag) plus a one-line label naming the source. This is the concrete shape for "show which tool/data source the agent used, without raw call JSON."

The remaining rows (Loading shimmer, Streaming text, Diff table, Records/Filter table, Insight card, Selection actions, Recommendation card) are still reasoned-not-measured — `/harness`'s own demo conversation didn't surface a case for them in this pass.

## 3 — Given the chosen pattern, does each individual element need to exist?

**This is this skill's actual job.** Once a pattern is chosen (existing or newly documented above), apply the necessity test to every element inside it: **would removing this lose information or capability the user needs right now?** If no, cut it. This is Dieter Rams' 10th principle — *"good design is as little design as possible"* — and it's checkable, not a taste call.

## How this connects to the rest of the skill

Question 1 decides whether to build anything. Question 2 decides which pattern. Question 3 prunes what's inside it. Only after all three does Color, Typography, Grid & Spacing, or Iconography become relevant — and Motion is the very last layer on top of all of it.

## Common mistakes — recognize these, don't build them

**The wrapper reflex**
Building a new one-off component for a job an existing pattern in the component library already answers, instead of checking it first. The tell: a PR adds a new component whose job matches one the library already has.

**The everything card**
One generic card or container reused for five different jobs (a metric, a warning, a recommendation, a task) instead of the cheapest correct representation for each specific job. The tell: every panel on a screen looks identical regardless of what it's communicating.

**The decorative element**
Keeping an element that fails the necessity test — nothing is lost if it's removed — because it fills space or "looks more finished." The tell: can't name what capability or information the user loses if it's deleted.

**Skipping the necessity gate**
Applying styling to a feature whose "should this exist at all" question was never actually answered by `/first-principles` or a necessity check. This skill styling something is not evidence the something should exist.
