# Components

## Proportion

When a button is shorter than the input next to it, something feels wrong even if the reason can't be named. Keep interactive elements on a form at the same height.

- Input height: `h-10` (40px)
- CTA button height: `h-10` (40px) — same as input, always
- All heights in multiples of 8px

## Component State Checklist

Elegant UI isn't one clever animation — it's every interactive component accounting for every state it can be in. Before shipping a component, confirm each state that applies to it:

| State | Standard |
|---|---|
| Default | Follows Color, Typography, Grid & Spacing above |
| Hover | Background or border opacity shift only — never a hue change, never a scale change |
| Active / pressed | Optional slight scale-down (`active:scale-[0.98]`) at `duration-100` — confirms the tap, nothing more |
| Focus-visible | See the focus-ring rule below — never skipped, this is accessibility, not decoration |
| Disabled | ~50% opacity, `cursor-not-allowed`, no pointer events — the element keeps its layout space, it doesn't disappear |
| Loading | Skeleton or spinner, transform-based (never a repainting shimmer), always with a reduced-motion fallback |
| Error | Border and text shift to the error token instantly — no animation delay on something the user needs to read right now |
| Empty | A designed state with one line of guidance and an action if one applies — never a blank container |

A component that only handles "default" and "hover" isn't done.

## Focus ring

The brand accent is reserved for CTAs — never for focus states. Border shift on elements that have a border; ring on elements that don't:

| Element | Focus class | Why |
|---|---|---|
| `input`, `input-otp`, `select` | `focus-visible:border-foreground/30` | Full-width border is perceptible |
| `button` | `focus-visible:ring-2 focus-visible:ring-foreground/20` | Default/ghost/secondary variants have no border — ring is the only reliable indicator |
| `checkbox` | `focus-visible:ring-2 focus-visible:ring-foreground/20 focus-visible:ring-offset-1` | 16px element — border color shift is imperceptible at that size |


## Common mistakes — recognize these, don't build them

**The two-state component**
Shipping a component that only handles `default` and `hover`, with loading/error/empty/disabled left undesigned. The tell: the first bug report is "what does this look like when it fails / is empty / is loading," and nobody knows.

**Height mismatch**
A CTA button that's shorter or taller than the input it sits next to in the same form. The tell: the form row looks slightly uneven even though nothing is individually broken.

**Hue-change hover**
A hover state that shifts an element's color or hue instead of only an opacity/background-strength shift. The tell: a button or row looks like a completely different color on hover rather than a slightly stronger version of itself.

**Silent disabled**
A disabled element that changes layout or disappears instead of staying in place at reduced opacity with `cursor-not-allowed`. The tell: content shifts around it when it becomes disabled.

**The missing ring**
An interactive element with no `focus-visible` indicator at all. The tell: tabbing through a form with the mouse untouched, some controls give no visible sign they're focused.
