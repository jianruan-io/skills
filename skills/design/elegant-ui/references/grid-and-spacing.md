# Grid & Spacing

## The 8px grid

All spacing, margin, padding, and sizing values are multiples of 8 (8, 16, 24, 32, 64px). Neither reference site was measured to contradict this — it's a standard, load-bearing convention, keep it. Never use `h-9` (36px) or `h-11` (44px) — off-grid, and the accumulation of small mismatches like these is exactly what makes a product feel assembled instead of designed.

## The ownership rule — which class to use

| Situation | Class | Plain English |
|---|---|---|
| Consistent gap between a stack of siblings | `space-y-*` or `gap-*` on parent | The parent controls the rhythm |
| Whitespace inside a container | `p-*` on the element itself | Stays with the element wherever it goes |
| One element needs extra separation from its neighbor | `mt-*` on that element | Isolated nudge — doesn't affect others |

## Standard values for auth forms

| Gap | Tailwind |
|---|---|
| Heading → description | `space-y-2` (8px) |
| Text block → first input | `space-y-6` (24px) |
| Field → field | `space-y-4` (16px) |
| Last field → CTA button | `mt-6` (24px) |
| Card inset | `p-6` (24px) |

## Border-radius — a scale, not one number

This was the biggest correction from actually measuring both reference sites: there is no single "correct" radius. It scales with the size and role of the element:

| Element type | Measured radius | Example |
|---|---|---|
| Small controls (nav item, chip) | `8px` | beautifului.dev sidebar nav item |
| Buttons (Linear) | `10px` | Linear's real button radius |
| Large containers / cards | `20–24px` | beautifului.dev's composer card measured at `22px` |
| Pills / fully-round elements | `9999px` (full) | Linear's "Skills" chip, beautifului.dev badges |

Pick radius by the element's size and role, not a blanket 4–8px rule — a large card at an 8px radius looks under-designed next to reference-quality UI; a small chip at 22px looks broken.

**A genuine per-context split, not a contradiction:** beautifului.dev's own buttons don't all use one radius either. Measured directly on the harness's inline "Accept" / "Alternatives" buttons (the pair on an approval card, inside a chat message) — full pill (`border-radius: 2.68435e+07px`, i.e. `9999px`), not Linear's `10px`. Read this as: a standalone toolbar/form button follows the `10px` row above, but a compact action button embedded inside a card of its own (an approval card, a suggestion chip) goes full pill instead — the pill reads as "attached to this card's content," a rectangular button reads as "part of the page chrome around it." Pick by that distinction, not by guessing between the two.

## Common mistakes — recognize these, don't build them

**The off-grid height**
`h-9` (36px) or `h-11` (44px) instead of a real 8px-grid value. The tell: nothing about the screen is individually wrong, but small mismatches like this accumulate into a product that feels assembled rather than designed.

**One radius for everything**
A blanket 4–8px radius applied to both a small chip and a large card, instead of scaling radius by element size and role. The tell: a large card looks under-designed (too sharp for its size) or a small chip looks broken (too round for its size).

**Spacing ownership confusion**
Margin on a child element fighting with `gap`/`space-y` on the parent for the same rhythm, instead of picking the one rule that owns it. The tell: the same visual gap is controlled from two different places, so changing one spacing value somewhere else in the stack doesn't propagate the way it should.
