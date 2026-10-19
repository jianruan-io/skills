# Color

> **Source of truth: your project's own design tokens** (e.g. the theme variables in a `globals.css`). Every colour in your UI comes from a token there — background, foreground, card, muted, border, primary, success/warning/destructive, chart colours. Never introduce a colour value that isn't there. The measured Linear / beautifului.dev values below are background research on *how* those products layer colour (lightness steps, hairlines, soft shadows) — they are not colours to paste in. If a needed shade is missing, use an existing token (or that token at lower opacity) and propose adding the token.

Every value below except the accent was measured directly from Linear (`linear.app`, light mode) and beautifului.dev's harness demo (`beautifului.dev/harness`, light mode — its theme toggle lives on the homepage and the choice carries over to the demo). Where the two references agreed, that value is treated as high-confidence. Where they differed, beautifului.dev's value is used, since it's the named target. Values are given in `oklch()` — Tailwind v4 and modern CSS support it directly; if your tokens are in `hsl()`, that's an implementation gap, not a reason to convert these numbers by hand — see the note at the bottom.

## Canvas and elevation

| Layer | Value | Note |
|---|---|---|
| Page canvas | `oklch(0.97 0 0)` | Beautifului.dev's exact background — pure neutral, zero chroma. Not warm, not cool. |
| Elevated card/panel | `oklch(1 0 0)` | Pure white. The gap to the canvas is only 3% lightness — that small a gap is what reads as "lifted," a bigger jump reads as a harsh cutout. |
| Linear's version (cross-check) | canvas `oklch(0.944 0.0025 264)`, card `oklch(0.9794 0.0025 264)` | Linear tints both very slightly toward blue-violet (hue ~264) instead of staying at zero chroma. Either approach is legitimate; beautifului.dev's zero-chroma version is the one to copy since it's the named target. |

**The technique, if a third elevation level is ever needed:** raise lightness by a small, consistent step in the same hue, don't add a shadow instead of raising lightness, and don't add a shadow *and* raise lightness — pick one. Both references pick lightness-as-primary-signal and use a very soft shadow only as a secondary edge cue (see Shadow, below).

## Border

`oklch(0.946 0.003 264.542)` at **0.5–0.6px** width (both references land on a genuinely sub-pixel hairline, not a full 1px). If a design tool or browser won't render sub-pixel borders reliably, `0.5px` with a fallback to `1px` at very low opacity is the acceptable compromise — never a full-opacity 1px border, it reads immediately as heavier than either reference.

## Shadow (the actual recipe, not a description)

The elevated-card shadow measured on beautifului.dev's harness:

```css
box-shadow:
  0 0 0 1px oklch(0.946 0.003 264.542),   /* the hairline ring, doing the border's job */
  0 18px 47px rgba(0, 0, 0, 0.03),         /* wide, very soft ambient layer */
  0 7.5px 19px rgba(0, 0, 0, 0.02),        /* mid layer */
  0 4px 10px rgba(0, 0, 0, 0.02);          /* tight layer, close to the edge */
```

Four layers: one hairline ring (replaces a hard border), three graduated blurs that get tighter and slightly more opaque as they get closer to the edge. Never use a single-layer `box-shadow` for a card that's meant to look "expensive" — a single shadow reads as a drop-shadow effect, not depth.

**The full elevation scale, not just the one card recipe** — read directly off `getComputedStyle(document.documentElement)` as CSS custom properties, so this is the harness's own token system, not a one-off measurement of a single component:

| Token | Use | Layers |
|---|---|---|
| `--shadow-xs` | Barely-there separation (a chip, an inset field's own edge) | `0 0 4px 0 rgba(0,0,0,.04)` — one layer |
| `--shadow-sm` | Standard elevated card (this is the recipe above) | 6 stacked layers from 18px/3% blur down to 0.5px/0.3% — the composer container and message cards both use this |
| `--shadow-md` | A raised/hovered card, or anything meant to read one level above the standard card | 5 layers, tighter and less spread than `sm`, plus a `-2px` inset layer that pulls the shadow slightly under the element |
| `--shadow-lg` | Overlays: dropdown menus, popovers, the model-selector panel | 5 layers, widest spread (`0 25px 50px`) — the only tier that reads as "floating above the page" rather than "resting on it" |

Every tier is applied as `0 0 0 1px var(--line), var(--shadow-*)` — the hairline ring is never dropped, even at the smallest tier. If your token file has no equivalent elevation scale (no `--shadow-xs/sm/md/lg` tokens), introducing one is real production code, not a doc-only change; see the implementation note at the bottom of this file.

## Full token map — names, not just descriptions

The rows above describe roles; these are the harness's actual CSS custom-property names, so a future measurement can be diffed against the same names instead of re-deriving them:

| Token | Value | Role |
|---|---|---|
| `--page` | `oklch(98.5% .001 286.376)` | Outermost background, behind the canvas |
| `--canvas` | `oklch(96.1% .002 247.84)` | App background (this is the "Page canvas" row above, both references agree it's ~effectively zero-chroma) |
| `--surface` | `oklch(100% 0 0)` | Elevated card/panel — pure white |
| `--inset` | `oklch(97.9% .002 247.839)` | A recessed area inside a surface (e.g. a code block, a well) |
| `--hover` | `oklch(97% .002 247.839)` | Row/item hover background — a step *down* from surface, not up |
| `--hover-2` | `oklch(93.3% .003 247.86)` | A second, stronger hover/active step for the same element type |
| `--ink` | `oklch(24.7% .006 258.361)` | Primary text (matches the "Primary text" row above) |
| `--ink-2` | `oklch(50.6% .01 264.477)` | A mid-weight secondary text — between primary and muted, not documented as its own row before |
| `--ink-3` | `oklch(69.5% .009 264.505)` | Muted/secondary text (matches the "Muted/secondary text" row above) |
| `--line` | `oklch(94.6% .003 264.542)` | The hairline border (matches the "Border" section above) |
| `--line-strong` | `oklch(91.2% .005 258.326)` | A more visible border — used on buttons (`--shadow-btn`) where a hairline alone would disappear against a white fill |
| `--line-soft` | `oklch(96.6% .002 264.542)` | Barely-there separator, softer than the default hairline |
| `--field` | `oklch(96.1% .001 286.375)` | A form field's own resting background, distinct from both canvas and surface |
| `--accent` | `oklch(62.6% .205 254.947)` | Beautifului.dev's own accent (blue) — your brand accent replaces this, per the Accent section below |
| `--accent-tint` | `oklch(96% .019 252.878)` | The tint half of the accent's categorical-tag pairing |
| `--green` / `--green-tint` | `oklch(60.3% .155 150.883)` / `oklch(95.8% .017 159.118)` | Success semantic pair — text/background, per the categorical-tag formula below |
| `--orange` / `--orange-tint` | `oklch(68.9% .179 49.902)` / `oklch(96.4% .021 67.581)` | Warning semantic pair |
| `--red` / `--red-tint` | `oklch(62.1% .192 23.042)` / `oklch(95.6% .017 17.462)` | Destructive semantic pair |
| `--tooltip-bg` / `--tooltip-fg` / `--tooltip-border` | `oklch(27.2% .008 264.435)` / `oklch(97.6% .002 247.839)` / `oklch(35.6% .007 264.474)` | Tooltips are the one surface that inverts — dark background, light text, even in light mode |

`--green`/`--orange`/`--red` plus their `-tint` counterparts are the categorical-tag formula (below) with real names attached — e.g. a "lead time: 7 days" pill on an approval card renders `bg-green-tint text-green`, confirmed live on the harness, not inferred.

## Text

| Role | Value | Note |
|---|---|---|
| Primary text | `oklch(0.247 0.006 258.361)` | Very dark, but not pure black — near-zero chroma with a barely-perceptible cool tint. |
| Muted/secondary text | `oklch(0.695 0.009 264.505)` | A *lighter step of the same near-neutral*, not a different gray and not an opacity trick on the primary color. Use this exact value for de-emphasized labels, timestamps, helper text. |

## Semantic colors

Neither reference site showed a live error/success state during inspection, so there are no measured values here. Keep your own `success`/`warning`/`destructive` tokens, as long as they're muted rather than neon — e.g. `hsl(142, 71%, 45%)`, `hsl(38, 92%, 50%)`, `hsl(0, 72%, 51%)`.

## Categorical tag / badge formula

For distinguishing many discrete values (tags, categories, status pills): background is a dark, low-saturation tint of the hue; text is a brighter version of the *same* hue; border matches the background. Never a saturated fill with white text, never a pastel fill with black text — both read louder than the information warrants. This lets a dozen categories sit next to each other legibly instead of turning into confetti.

## Accent — the one deliberately different value

Your brand accent (`--primary`) replaces whatever accent color either reference uses, and only there. It is reserved for CTAs — never on focus rings, hover text, or decorative elements. This is the single intentional brand deviation from "copy everything." Everything else on this page is copied.

## Implementation note (not part of this skill's scope)

If your token system is defined in `hsl()`, migrating it to `oklch()` to use these exact values — or converting these `oklch()` values into `hsl()` — is real production code touching every app. Do it on its own branch with its own review, not as a side effect of reading this reference.

## Common mistakes — recognize these, don't build them

**The saturated fill**
A solid, high-saturation background with white text for a tag or badge, instead of the dark-tint-background-plus-bright-same-hue-text categorical formula above. The tell: a row of status pills reads louder than the information warrants, almost neon.

**Primary color creep**
The brand accent showing up on a focus ring, a hover state, or a decorative flourish instead of only a CTA. The tell: the accent appears somewhere the user isn't being asked to take the one primary action on the screen.

**The hard shadow**
A single-layer `box-shadow` on an elevated card instead of the four-layer graduated recipe. The tell: the card reads as "has a drop-shadow" rather than "sits above the page" — flat with a smudge under it, not lifted.

**Full-opacity border**
A full-strength 1px border where a sub-pixel hairline was measured. The tell: the element's edge reads heavier than anything on Linear or beautifului.dev — a line the user notices, not one they only sense.

**Muted-as-opacity**
Dimming the primary text color with an opacity value to fake secondary/muted text, instead of using the actual lighter near-neutral step (`--ink-3` / `oklch(0.695 0.009 264.505)`). The tell: the muted text looks washed-out or slightly colored instead of a clean lighter gray.
