# Retro — Raise the Standard

**What:** Turn what this sprint learned into a stronger standard for the next one.

**Why:** The what has no ceiling. A miss that isn't written down will happen again; one written into a domain skill gets caught every time after.

**How:** Answer three questions. Each answer that produces a change becomes its own issue — the shipped sprint is never edited.

---

## Questions

1. **What got past the done check?** Anything the human flagged in review, or the automated reviewer caught, that the spec's checks missed. → Add it as a standard or check to the domain skill that should have caught it.
2. **Which done checks had no domain skill?** Every item marked `(no domain skill)` in the spec. → Propose a new domain skill, or a new section in an existing one, holding that standard and how to verify it.
3. **What did the human do by hand more than once?** A manual step repeated across sprints. → Turn it into a skill with `/new-skill`.

---

## Output

```
━━━ Retro: [Issue Title] ━━━━━━━━━━━━━━━━━━━━━━━━━━

Missed by done check
  [miss] → [domain skill] · [new standard or check]
  (none)

Standards with no domain skill
  [standard] → [new skill | section in existing skill]
  (none)

Repeated by hand
  [step] → /new-skill [name]
  (none)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

If all three are `(none)`, say so — then ask whether the done check was strict enough to catch anything at all.
