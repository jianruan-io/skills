# Typography

## Typeface

**Inter.** Both references converge on it independently — beautifului.dev serves `"Inter", "Inter Fallback", ui-sans-serif, system-ui, sans-serif`; Linear serves `"Inter Variable", "SF Pro Display", -apple-system, system-ui, sans-serif`. A product that declares no typeface (no `@font-face`, no font package, no `font-family` override) renders in whatever the visiting OS defaults to. That's an accident, not a decision. Adopt Inter explicitly.

## Type scale

Use a Perfect Fourth scale (1.333 ratio) — it wasn't contradicted by either reference and stays on-grid with the rest of the system:

| Element | Style | Size |
|---|---|---|
| Page / auth heading | `text-2xl` | 24px |
| Section heading | `text-xl` | 20px |
| Body / description | `text-sm` | 14px |
| Input / button text | `text-sm` | 14px |
| Error / helper | `text-xs` | 12px |

## Weight — a real correction, not a guess

Measured directly: beautifului.dev's large "Hello Shane" greeting (26px) renders at **font-weight 400 (regular)**, not bold or semibold. This contradicts the common default of `font-semibold` on page headings. Adopt the reference's actual choice: large display text can be regular weight if the tracking and size alone carry the hierarchy — don't reach for bold as the default move. Section headings and anything competing with body text on the same screen can still use `font-semibold` for contrast; the correction applies specifically to large, standalone display text (auth screens, empty-state greetings), where regular weight measured as the "expensive" choice on the reference site.

## Letter-spacing (tracking) — scales with size, not a universal constant

This is the one place the two references genuinely diverge, and the honest reconciliation is that tracking is **size-dependent**, not one fixed number applied everywhere:

| Text size | Measured tracking | Source |
|---|---|---|
| Large display (~26px) | `-0.52px` (≈ `-0.02em`) | beautifului.dev "Hello Shane" |
| Body/UI text (~14px) | `-0.14px` (≈ `-0.01em`) | beautifului.dev suggestion rows, input |
| Small UI text (~12–16px) | `normal` (0) | Linear — measured `normal` on real chat UI text at 13–16px |

**The rule, reconciled:** the bigger the text, the more negative the tracking gets — large headlines get real, deliberate tightening (`tracking-tight` / `-0.02em`); body and small UI text get little to none. Don't apply `tracking-tight` to every text size uniformly — Linear's own small text proves that's not what the best examples actually do.

## Tabular numerals — new, not previously documented

Every currency figure must use `tabular-nums` (or the Tailwind `tabular-nums` class). Misaligned digits in a column of money is one of the fastest "cheap" tells in any product that shows money — apply it to every numeric column, not just the ones someone happened to remember.

## Common mistakes — recognize these, don't build them

**Bold-by-default**
Reaching for `font-semibold` or `font-bold` on large display text automatically, instead of checking whether size and tracking alone can carry the hierarchy. The tell: every heading on a page is bold, so nothing on the page actually stands out as heavier than anything else.

**One tracking value everywhere**
Applying `tracking-tight` uniformly regardless of text size, instead of scaling it down toward `normal` as text gets smaller. The tell: small UI text (12–16px) looks slightly cramped instead of open.

**Misaligned money**
A column of currency figures without `tabular-nums`, so the digits don't line up row to row. The tell: a `9` sits narrower than a `1` in the same column, and the decimal points drift out of a vertical line.

**Whatever the OS serves**
No deliberate typeface declared anywhere, so the product renders in whatever font the visitor's operating system happens to default to. The tell: the same screen looks different on Windows, Mac, and Linux, and nobody chose that on purpose.
