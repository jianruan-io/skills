# Accessibility & Enforcement

These aren't pillars — they're constraints that apply to every pillar above, not a step in a sequence.

## Accessibility bar

- Every focus state in `references/components.md` must be present — this is the accessibility floor (WCAG 2.4.7), not decoration, and it's never optional.
- Every animation in `references/motion.md` needs a `prefers-reduced-motion` fallback: keep opacity/color transitions, remove transform/position movement.
- Color contrast: text and interactive elements need to meet WCAG AA contrast against their background. The measured text values in `references/color.md` (near-black primary, a lighter step for muted) were not independently contrast-checked against your exact canvas value — verify before shipping a screen that relies heavily on the muted text color for anything load-bearing (not just decorative).
- Keyboard navigation and ARIA labeling are not covered anywhere in this skill yet — this is a real, undocumented gap, not an oversight to route around silently.

## Enforcement — the honest state of things

Nothing in this skill is automatically checked unless you add a check for it — a lint rule, a CI gate, or a visual/Storybook test. Until then, assume drift: a shared `Button` can quietly default to an off-grid height or ship with no focus ring, and nothing will flag it.

**How to apply:** before treating any existing component as a correct example to copy, check it against the relevant reference file first. "It's already in the codebase" is not evidence it's correct. Fixing the shared library itself is real production code affecting every app; it needs its own branch and review, not a silent fix bundled into unrelated work.

## Common mistakes — recognize these, don't build them

**"It's already in the codebase"**
Treating an existing component as a correct example to copy just because it ships today, without checking it against the relevant reference file. The tell: a new screen inherits `Button`'s missing focus ring or off-grid height because it copied an existing usage instead of the documented rule.

**Copy-paste contrast**
Assuming a text/background pairing passes WCAG AA because it's used elsewhere in the product, without checking the actual contrast ratio for the specific combination on the specific screen. The tell: muted text sits on a tinted or colored background it wasn't originally measured against.

**Mouse-only testing**
Shipping a component checked only by clicking through it with a mouse, with no keyboard-only pass. The tell: tabbing through the screen skips an interactive element, traps focus, or never shows where focus currently is.
