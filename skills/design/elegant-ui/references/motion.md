# Motion

Primary source: `github.com/emilkowalski/skills`, specifically `skills/emil-design-eng/SKILL.md` — Emil Kowalski, Design Engineer at Linear (ex-Vercel), creator of Sonner and Vaul (thousands of production apps). This is the highest-confidence source in this whole skill; if a motion rule ever needs revisiting, check his repo first, not a general web search.

Secondary reference: `codedvisuals.com/visuals` — a showcase library of animated UI illustrations (React/shadcn, built on Framer Motion), not a real product. Use it only to see *technique*, never to copy its amount of motion — it animates constantly because that's the product being sold, which is the opposite of the restraint this skill asks for everywhere else. Techniques worth borrowing, for the rare cases that pass the four tests below:

- Entrances layer two things at once — opacity together with a blur fade (`blur(8px)` → `blur(0px)`) or an SVG path draw (`pathLength` 0→1) — instead of opacity alone. More convincing, but blur and non-transform SVG properties both cost a repaint; see "What to animate" below before reaching for this on anything real.
- Position, opacity, and other properties each get their own timing/easing curve instead of sharing one — this is what keeps a multi-part entrance from feeling like one rigid unit.
- Elements enter in sequence by index (stroke draws, then fill fades in ~0.5–0.7s later), not just staggered — layers can be sequenced, not only offset.
- Loops (a pulse, a glint sweep) use uneven, hand-placed keyframe timing rather than a plain sine loop, so a repeat feels organic instead of mechanical. This still has to clear the "never loop a static/waiting state" rule above — it's for an active, in-progress indicator, not a steady-state one.

Real measurement, not a guess: Linear — the actual reference product — animates only **~3% of its DOM elements** (67 of 2,324, measured live). Beautifului.dev's harness animates **~23%**. The more restrained product is the one universally agreed to look more expensive. Motion is the smallest lever in this whole skill, and it's the last one to reach for.

## The four tests

Every animation must clear one of these — if none apply, ship with no animation at all:

| Test | What it looks like |
|---|---|
| Explains a change | A panel expanding shows where its content came from, instead of popping into existence |
| Confirms the tap registered | A button scales down slightly on press so the user knows the click landed |
| Preserves spatial continuity | A toast slides in and out from the same edge every time, so dismissing it feels predictable |
| Rare and genuinely delightful | A one-time entrance the user sees once per session — never something repeated on every page load |

**Frequency overrides all four tests.** Keyboard-triggered and 100+-times-a-day actions (shortcuts, typing) get zero animation. Tens-of-times-a-day actions (hovers, nav, tabbing) get none or the bare minimum. Occasional actions (opening a modal or a drawer) get the standard treatment below. Rare, first-time moments (onboarding, an empty-state greeting) are the only place delight is worth the extra duration.

**Never animate a static or waiting state with a loop.** `animate-pulse`, `animate-bounce`, and similar looping animations read as "still uncertain," not "responded" — reserve looping motion for an active spinner that is genuinely still working, never for a steady-state indicator.

## Duration

| Use | Tailwind | Why |
|---|---|---|
| Press/tap feedback | `duration-100` (100–160ms) | Confirms the tap landed, nothing more |
| Tooltip, small popover | `duration-150` (125–200ms) | Fast enough to feel responsive, not sluggish |
| Dropdown, select, tab switch | `duration-200` (150–250ms) | Enough to track motion without waiting on it |
| Modal, drawer, sheet entrance | `duration-300` (200–500ms) | The one case allowed past 300ms — still snappy, never draggy |

Default to under 300ms everywhere else — slower doesn't read as more polished, it reads as the product being slow. Exit is always faster than enter: press can be slow when a user is deciding (e.g. a 2s hold-to-delete), release or dismiss is always fast (~200ms). A one-time, once-per-session entrance (e.g. an empty-state greeting) can run longer still — up to ~800ms — but only if the same view never plays it twice. (Beautifului.dev's own "Hello Shane" entrance measures at exactly 800ms, blur(17px) + translateY(23px), staggered across four elements at 170/330/400/550ms — this is the real, measured example of that exception.)

## Easing

| Motion | Token | Curve |
|---|---|---|
| Entering or exiting the screen | `--ease-out` | `cubic-bezier(0.23, 1, 0.32, 1)` |
| Moving or morphing in place (tab indicator, accordion) | `--ease-in-out` | `cubic-bezier(0.77, 0, 0.175, 1)` |
| Hover, color shift | `ease` (Tailwind default) | — |
| Constant/looping motion (a genuine loading spinner) | `linear` | — |

Define these as CSS custom properties once and reuse them everywhere — a scattered mix of ad-hoc bezier values is what makes a UI feel assembled instead of designed. **Never use `ease-in` on a UI transition.** It delays the initial movement, so the interface feels sluggish at the exact moment the user is watching most closely — an exit or dismissal should be fast, never eased-in. (Live-measured cross-check: Linear's dominant transitions actually use the plain `ease` keyword for most interactions, and its one real custom curve — `cubic-bezier(0.45, 0, 0.55, 1)` — is used on a `margin` transition, which itself breaks the transform/opacity-only rule below. Even the reference product isn't perfectly clean on every axis — don't take "but Linear does X" as an excuse without checking it's actually the rule, not an exception.)

**Beautifului.dev's harness (`/harness`), measured directly via computed styles, not devtools eyeballing — two curves, picked by what's moving, never mixed:**

| Motion | Curve | Duration | Where measured |
|---|---|---|---|
| Layout / spatial change (sidebar collapse, row background+transform on hover) | `cubic-bezier(0.16, 1, 0.3, 1)` | 180ms (row hover), 280ms (sidebar width) | Every `.sidebar-row` button and the collapsible `<aside>` itself |
| Button press feedback (background-color, transform, opacity together) | `cubic-bezier(0, 0, 0.2, 1)` | 150ms | The inline "Accept" / "Alternatives" buttons on an approval card |

`cubic-bezier(0.16, 1, 0.3, 1)` is close to but not identical to the generic `--ease-out` token above (`0.23, 1, 0.32, 1`) — when a value from this skill's generic Emil Kowalski source and a live measurement off the named target site disagree, use the measured one; update the `--ease-out` token to `cubic-bezier(0.16, 1, 0.3, 1)` the next time it's touched in code, since beautifului.dev is the named target and this is now a second, independent measurement confirming it (previously only the duration table cited a live measurement; the curve itself hadn't been).

## What to animate

Only animate `transform` and `opacity`. Animating `box-shadow`, `width`/`height`, `mask-position`, or a `filter` forces the browser to repaint the element every frame instead of just recomposing it on the GPU — it looks fine on a fast machine and stutters everywhere else. If a design calls for something like a shimmer or a glow sweep, build it by moving a translated overlay, not by animating the property that looks closest to the effect. If using Framer Motion, animate the `transform` string directly (`animate={{ transform: "translateX(100px)" }}`) — its `x`/`y`/`scale` shorthand props run on the main thread via `requestAnimationFrame`, not the GPU, and drop frames under load.

## How to build it

- Never scale an entrance in from `scale(0)` — start from `scale(0.95)` with `opacity: 0`. Nothing in reality shrinks to nothing; starting smaller keeps the motion believable.
- A popover or dropdown scales from its trigger (`transform-origin` matches the trigger location), not from center. Modals are the exception — they stay centered since they aren't anchored to a trigger.
- When several elements enter together, stagger them 30–80ms apart instead of all at once — and never block interaction while the stagger plays.
- Gate hover-triggered animation behind `@media (hover: hover) and (pointer: fine)`. Touch devices fire `:hover` on tap, so an un-gated hover animation plays as an unwanted flash on every tap.

## Reduced motion

`prefers-reduced-motion` means fewer and gentler animations, not zero. Keep opacity and color transitions — they aid comprehension — and remove transform/position-based movement. This isn't optional polish — it's the same accessibility bar as the focus ring.

## Common mistakes — recognize these, don't build them

**The uncertain loop**
`animate-pulse` / `animate-bounce` on a steady-state indicator. The tell: the loop keeps playing after the thing it's representing has already finished or hasn't started, reading as "still uncertain" instead of "responded."

**Motion without a reason**
An animation added because it looks nice, without clearing any of the four tests (explains a change, confirms a tap, preserves spatial continuity, rare delight). The tell: can't name which of the four tests it passes — ship with none instead.

**The repaint property**
Animating `box-shadow`, `width`/`height`, or a `filter` instead of `transform`/`opacity` only. The tell: the animation is smooth on a fast dev machine and stutters on anything else.

**Ease-in on exit**
Using `ease-in` on a dismiss or exit transition. The tell: the element sits still for a beat before it starts leaving, at the exact moment the user is watching most closely.

**Same curve for everything**
One bezier value copy-pasted across layout changes, press feedback, and entrances instead of picking the curve by what's actually moving. The tell: a sidebar collapsing and a button press use the identical easing token even though they're different kinds of motion.
