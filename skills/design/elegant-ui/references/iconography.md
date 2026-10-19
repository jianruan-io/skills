# Iconography

**Honesty note:** unlike Color, Typography, and Grid, icon-level detail (exact stroke width, exact sizing ratios) was not directly measured on either reference site during inspection — this section is a common, reasonable default, not a reverse-engineered number. Treat it as lower-confidence than the other pillars until someone actually measures it.

## Icon set

`lucide-react`, or whichever single set your product already uses. Don't mix in a second icon library for a new feature; consistency of stroke style across the whole product matters more than any single icon's design.

## Stroke width

Lucide's default stroke width (`2`) is the standard. Don't override it per-icon — a mix of stroke weights on the same screen is one of the fastest ways to make an icon set look assembled rather than designed.

## Sizing relative to text

Icons should be sized relative to the text they sit next to, not a fixed pixel value everywhere:

| Text size | Icon size |
|---|---|
| `text-sm` (14px) | `size-4` (16px) |
| `text-xs` (12px) | `size-3.5` (14px) |
| Standalone icon button | `size-4` inside an `h-9`/`h-10` hit target |

## Open question

If a future pass through Linear or beautifului.dev finds a different, measured convention (e.g. a different stroke width, a documented icon-to-text ratio), replace this section with the measured value and remove this honesty note.

## Common mistakes — recognize these, don't build them

**Mixed icon sets**
Pulling in a second icon library for a new feature instead of staying inside the one chosen set. The tell: two icons on the same screen have visibly different stroke styles or corner treatments.

**Per-icon stroke override**
Overriding stroke width on individual icons instead of leaving the shared default alone. The tell: some icons on a screen look thin and others look bold, with no reason tied to meaning.

**Fixed-pixel icons**
Sizing every icon to one constant value regardless of the text it sits next to, instead of scaling relative to the adjacent text size. The tell: an icon next to `text-xs` helper text looks oversized, or an icon next to a heading looks too small.
