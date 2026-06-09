# Hazen Gone Loco — Trip Countdown Teaser

A single-page, mysterious **trip countdown / teaser** site for six friends ("the hazen") taking a 10-day trip. The destination stays secret and is revealed through monthly "clues." Built around a live countdown to departure (**29 Oct 2026, 08:00**), an animated "stardust" depletion field (grains of light = time remaining), a "six become one" convergence section, and a monthly-unlocking photo-clue grid.

The vibe is **electric dusk**: deep indigo/violet background with a warm amber accent, a single-line yellow hare mascot, film grain, and tasteful motion.

> *Drop a screenshot here once you've taken one — `assets/preview.png` is the convention.*

---

## Quick start

The page is a single self-contained HTML file. No build step, no dependencies beyond Google Fonts (loaded from the CDN) and the two image assets in `assets/`.

```bash
# easiest — just open it
open option-8.html

# or, if you need a local origin (recommended for fonts/canvas):
python3 -m http.server 8000
# then visit http://localhost:8000/option-8.html
```

Tested on Chrome 111+, Safari 16.4+, and Firefox 113+. Uses `oklch()` colors, `backdrop-filter`, and `100svh` — older browsers will degrade gracefully but won't look quite right.

---

## What this is (and what it isn't)

`option-8.html` is a **design reference** — a working prototype that demonstrates the look, motion, and behavior. It is **not** production code to ship as-is. The intent is to recreate this design in your target codebase's environment (React, Vue, Svelte, SwiftUI, etc.) using that codebase's component library and conventions. Treat the CSS values below as the source of truth for colors, spacing, and typography.

**Fidelity:** high. Final colors, type, spacing, copy, and interactions are all specified — recreate as close to pixel-perfect as your environment allows.

---

## Design tokens

### Colors

| Token | Value | Use |
|---|---|---|
| `--ink` | `#F6ECF4` | Primary text (near-white, warm) |
| `--ink-soft` | `#B79EC4` | Secondary/muted text, labels |
| `--accent` | `#FFD16B` | Single accent — amber/yellow (numbers, rules, hare, highlights) |
| `--rose` | `#E14E6B` | Secondary glow (particle dots, accents) |
| `--violet` | `#9B6BD0` | Tertiary glow (particles, placeholder stripes) |
| `--panel` | `#271743` | Countdown panel gradient top |
| `--panel-2` | `#1A0F30` | Countdown panel gradient bottom |
| `--panel-line` | `rgba(246,236,244,0.13)` | Panel border |
| `--glass` | `rgba(28,18,46,0.42)` | Glassmorphism fill (origins/dest, with `backdrop-filter: blur(10px)`) |
| `--line` | `rgba(246,236,244,0.14)` | Hairline borders |
| Page background | `#141038` | Body base behind the fixed gradient |

**Background gradient** (fixed, full-viewport, `z-index:0`, slowly parallaxes with pointer + scroll, `scale(1.06)`):

```css
background:
  radial-gradient(120% 78% at 76% 2%, rgba(214,74,40,0.5), transparent 54%),
  radial-gradient(95% 70% at 10% 24%, rgba(225,78,107,0.4), transparent 56%),
  radial-gradient(110% 90% at 50% 112%, rgba(72,48,152,0.85), transparent 62%),
  linear-gradient(176deg, #2a1546 0%, #4a2150 20%, #6c2c4f 35%, #3c2668 56%, #262a6e 78%, #161940 100%);
```

**Film grain** (fixed, `z-index:1`, `opacity:0.34`, `mix-blend-mode: soft-light`): an inline SVG `feTurbulence` (fractalNoise, `baseFrequency=0.9`, `numOctaves=3`, desaturated) tiled at native `220px × 220px`. Do **not** upscale the SVG — keep it crisp.

**Cursor halo** (fixed, `z-index:2`, follows pointer, `mix-blend-mode: screen`): 540px radial `rgba(255,209,107,0.28) → rgba(225,78,107,0.16) → transparent`, fades in on pointer move. Positioned with `transform: translate3d(...)` for compositor-only updates.

### Typography

Google Fonts. The body font is user-switchable at runtime (see Font switcher).

- **Display / body:** `Familjen Grotesk` (default), with switchable alternates `Sora`, `Onest`, `Hanken Grotesk`. Exposed via CSS var `--font`.
- **Mono / labels / numbers:** `Space Mono` (used for eyebrows, the countdown digits, codes, badges, captions).

All sizes use `clamp()` for fluid scaling.

| Element | Size | Weight | Other |
|---|---|---|---|
| Headline (`6 hazen. / 10 days. / 1 epic trip.`) | `clamp(40px, 7.6vw, 116px)` | 700 | `line-height:0.94`, `letter-spacing:-0.03em` |
| Headline leading numbers (`6/10/1`) | inherit | 700 | colored `--accent` |
| Section `h2` | `clamp(34px, 5.2vw, 68px)` | 700 | `letter-spacing:-0.025em` |
| Lede paragraph | `clamp(14px, 1.4vw, 18px)` | 400 | `line-height:1.6`, `color:--ink-soft`, `max-width:46ch` |
| Eyebrow | `12px` | — | mono, `letter-spacing:0.26em`, uppercase, `--accent` |
| Countdown digits | `clamp(34px, 6vw, 74px)` | 700 | mono, `font-variant-numeric: tabular-nums` |
| Countdown labels (Days/Hours/Min/Sec) | `10px` | — | mono, `letter-spacing:0.22em`, uppercase |
| Origin city | `clamp(24px, 2.8vw, 38px)` | 600 | |
| Clue label (`.no`) | `12px` | — | mono, `letter-spacing:0.18em`, `--accent` |
| Clue lock badge | `10px` | — | mono, uppercase |
| Brand wordmark | `13px` | — | mono, `letter-spacing:0.24em`, uppercase |

### Spacing, radius, shadow

- Section padding: `clamp(72px,12vh,150px) clamp(26px,5.5vw,90px)`, `max-width:1300px`, centered.
- Hero padding: `28px clamp(26px,5.5vw,90px) 32px`.
- Border radius: countdown panel `24px`; origin cards `16px`; dest card `20px`; clue cards `18px`; pills/badges `999px`.
- Shadows: panel `0 36px 80px rgba(20,8,40,0.5), inset 0 1px 0 rgba(255,255,255,0.06)`; card hover `0 30px 60px rgba(20,8,40,0.46)`.

---

## Sections

A single scrolling page with four stacked sections plus a footer line.

### 1. Top bar

- Full-width flex row at top of hero (left-aligned brand only).
- **Brand:** a 12px amber diamond (rotated square, `box-shadow: 0 0 16px rgba(255,209,107,0.55)`) + the wordmark **"Hazen gone loco"** (mono, visually uppercased via letter-spacing).

### 2. Hero

- `min-height:100svh`, vertical flex (topbar / centered middle / footer row). Middle block is `max-width:1180px`, centered.
- **Hero head** is a 2-column grid: `grid-template-columns: auto minmax(0,1fr)`, `align-items:center`, `gap: clamp(22px,4vw,60px)`.
  - **Left column — copy:**
    - Kick: a 56px amber rule + eyebrow "The countdown has begun".
    - Headline (3 lines, each animates up on load): `6 hazen.` / `10 days.` / `1 epic trip.` — leading numbers in amber.
    - Lede: *"Six hazen, all moving from different parts of the world to one unique location. Get ready. You think you know what's happening? Think again. It's gonna be huge."*
  - **Right column — hare mascot:** the single-line **yellow hare** (`assets/hare-yellow.png`), `height: clamp(248px,42vh,460px)`, centered within the remaining space, with `drop-shadow(0 6px 30px rgba(255,209,107,0.30))`. Entrance fade/rise then a slow infinite float (±9px, 7s ease-in-out). Float disabled under `prefers-reduced-motion`.
- **Countdown panel:**
  - Rounded `24px`, gradient `--panel → --panel-2`, panel shadow.
  - A `<canvas>` ("stardust") fills it: `height: clamp(150px,21vh,200px)`. Particles = grains of light; the number alive shrinks as time elapses (see Interactions).
  - Overlaid readout row (4 segments, `pointer-events:none`): **Days · Hours · Min · Sec**. The Sec value is amber; others near-white with rose glow.
  - A 3px progress **meter** pinned to the bottom; amber fill with glow, width = % elapsed.
  - Caption row below: `"<X%> of the wait has burned away"` (X in amber) and `"every grain of light = time left"`.
- **Hero footer row** (flex, space-between, wraps):
  - **Dates** group (4 items): `29 Oct / Wheels up`, `7 Nov / Home again`, `10 days / Off the grid`, `······ / Where`.
  - **Scroll cue** (right): "Follow the clues ↓" with a bobbing arrow (hidden on mobile).

### 3. "Six become one" (convergence)

- Section heading: **h2** "Six become *one*" (the word "one" in amber) + eyebrow "Three departures · one meeting point".
- **Layout:** `grid-template-columns: 1fr auto 1fr`, `gap: clamp(20px,4vw,60px)`, centered.
  - **Left — origins:** 3 glass cards (`--glass` + `backdrop-filter: blur(10px)`, `16px` radius, `--line` border). Each: city + IATA code (amber mono) + passenger dots + count.
    - `Amsterdam / AMS — ●●●● ×4`
    - `Lima / LIM — ● ×1`
    - `Bonaire / BON — ● ×1`
    - Hover: `translateX(8px)`, accent border, left-offset shadow.
  - **Middle — arrows:** vertical "converge" label (`writing-mode: vertical-rl`) + a 1px vertical gradient line (amber, fades at ends).
  - **Right — destination:** glass card, label "Meeting point", the word **"Somewhere"** rendered with `filter: blur(13px)` (kept secret), and a note `// classified until the reveal` in amber mono.

### 4. "A few clues, slowly" (monthly photo clues)

- Section heading: **h2** "A few *clues*, slowly" + eyebrow **"A new one unlocks each month"**.
- **Layout:** `display:grid; grid-template-columns: repeat(auto-fit, minmax(258px, 1fr)); gap:18px`.
- **4 clue cards** (photo cards). Each card: `position:relative`, `min-height: clamp(300px,40vh,400px)`, `border-radius:18px`, `overflow:hidden`, `--line` border. Layered children:
  1. `.photofill` — absolute, `background-size:cover; center`. Clue 01 uses `assets/clue-lime.jpg`. Placeholders use a diagonal stripe gradient: `repeating-linear-gradient(45deg, rgba(155,107,208,0.24) 0 14px, rgba(225,78,107,0.15) 14px 28px)`.
  2. `.grad` — absolute gradient overlay: `linear-gradient(to top, rgba(14,8,30,0.95) 4%, rgba(16,9,34,0.6) 34%, rgba(40,20,70,0.16) 66%, rgba(255,209,107,0.06) 100%)`.
  3. `.body` — bottom-aligned, holds **only the clue label** (no title/description text). e.g. `CLUE 01 · JUNE`.
  4. `.lock` (locked cards only) — top-right pill badge with the unlock month.
- **Card content (labels only):**
  - **Clue 01** — *unlocked*, lime photo, label `CLUE 01 · JUNE`.
  - **Clue 02** — *locked*, label `CLUE 02`, badge `Unlocks · Jul`.
  - **Clue 03** — *locked*, label `CLUE 03`, badge `Unlocks · Aug`.
  - **Clue 04** — *locked*, label `CLUE 04`, badge `Unlocks · Sep`.
- **Locked state:** `.photofill` gets `filter: blur(15px) saturate(0.55) brightness(0.8); transform: scale(1.15)`; the label dims (`opacity:0.85`). The lock badge sits top-right: mono 10px uppercase, `rgba(20,12,40,0.5)` fill, `backdrop-filter: blur(8px)`, `--line` border, `999px` radius.
- **Hover (all cards):** card lifts `translateY(-8px)` and gains accent border and a heavier shadow; photo zooms (`scale(1.07)`; locked `scale(1.18)`); gradient lightens (`opacity:0.8`); label nudges up `-4px` and gains an amber text glow; a **diagonal light sweep** crosses the card via a `::after` linear-gradient sheen animating `translateX(-130%) → 130%` over `0.85s`.

### 5. Footer line

- Centered, top hairline border. Big line: "The rest *reveals itself* soon." ("reveals itself" amber). Small mono caption: "Made for the hazen · 2026".

### Font switcher (utility, bottom-right)

- Fixed pill (`right:18px; bottom:18px`), glass background. Label "Font" + 4 buttons: **Familjen / Sora / Onest / Hanken**. Active button has amber fill + dark text. Selection persists to `localStorage` key `lustrum-font-8` and sets CSS var `--font` on `:root`. Marked up as `<div role="group" aria-label="Font">` with `aria-pressed` on each button.

---

## Interactions & behavior

### Countdown

- `START = 2026-06-01T00:00:00`, `TARGET = 2026-10-29T08:00:00`. `TOTAL_SPAN = TARGET - START`.
- Every second: `diff = max(0, TARGET - now)`; display `days` (floor), `hours/mins/secs` (zero-padded). Update meter width and "% burned" caption to `elapsed = clamp((now - START) / TOTAL_SPAN, 0, 1)`.

### Stardust canvas

- Particle count scales to the canvas area: `count = clamp(80, round(W*H/600), 300)`. The original hard-coded 300 is now an upper bound — mobile gets fewer.
- Colors sampled from a 3-color palette: amber `oklch(0.86 0.14 78)`, rose `oklch(0.70 0.21 5)`, violet `oklch(0.62 0.2 318)`. Each particle has a base "home" position, twinkle phase, radius.
- Each frame: gentle drift back toward home + sinusoidal jitter; pointer within ~95px repels particles.
- **Depletion:** alive count = `round(count * (1 - elapsed))`. Alive = bright twinkling dot with a single `shadowBlur` pass; dead = faint dim dot drawn first (no shadow). The field visibly empties as departure nears.
- DPR-aware (`min(devicePixelRatio, 2)`); rebuilds on resize (debounced 200ms).
- **Paused** when the panel is offscreen (IntersectionObserver) or the tab is hidden (`visibilitychange`). Saves significant CPU once the user scrolls past the hero.

### Motion / entrance

- Headline lines rise in (`translateY(112%) → 0`, staggered 0/0.1/0.2s, `cubic-bezier(.16,.84,.27,1)`, ~1.05s).
- Lede and countdown panel fade-up (staggered).
- Hare: fade/rise in (~0.7s delay) then infinite slow float.
- Sections/cards use an **IntersectionObserver** reveal: `.reveal { opacity:0; translateY(30px) }` → `.in { opacity:1; none }`, threshold 0.16, staggered by `0.08s * (i%3)`.
- Background parallax: lerped translate toward `pointer*16/14` minus `scrollY*0.04`, `scale(1.06)`. The rAF loop self-terminates when the target stops moving and restarts on pointer/scroll input.

### Reduced motion

All entrance/looping motion respects `prefers-reduced-motion: reduce` — both in CSS (set to visible end-state, loops disabled) **and** in JS:

- The canvas draws a single static frame instead of running rAF.
- The parallax rAF never starts.
- Hare float, headline rise, lede fade, scroll-cue bob are all CSS-disabled.

### Responsive behavior

- Hero head stays 2-column; hare shrinks: `clamp(172px,31vw,300px)` ≤860px, `132px` ≤600px, `104px` ≤380px.
- Convergence grid collapses to a single column ≤820px; the arrows row flips horizontal.
- Clue grid is auto-fit and wraps to 1 column on narrow screens.
- Countdown readout wraps/centers, digits shrink (`clamp(30px,9vw,42px)`) ≤600px; scroll cue hidden ≤600px.
- Font switcher shrinks and drops its "Font" label on small screens.
- Layout verified down to ~375px.

### State

- `font` (persisted, `localStorage` key `lustrum-font-8`, wrapped in try/catch for private-mode browsers).
- Derived per-tick: `diff`, `days/hours/mins/secs`, `elapsed %`.
- For production: a per-clue `unlockDate` driving the locked/unlocked state.

---

## Files

```
option-8.html              the complete reference (HTML + CSS + vanilla JS)
assets/hare-yellow.png     single-line hare mascot, 522×837, transparent PNG, amber #FFD16B
assets/clue-lime.jpg       Clue 01 photo (limes), 820×1185, ~84 KB
```

Fonts load from Google Fonts (`Familjen Grotesk`, `Sora`, `Hanken Grotesk`, `Onest`, `Space Mono`).

---

## Known limitations / nice-to-haves

- **Clue unlock is hard-coded.** The "JUNE" label on Clue 01 and the `Unlocks · Jul/Aug/Sep` badges are static — in production this should be date-driven (compare `now` to each clue's `unlockDate`) so cards auto-reveal monthly.
- **Hare PNG is oversized.** The asset is 522×837 but never rendered larger than ~460px wide. A `~460×740` PNG, a WebP, or an inline SVG of the line-art would cut ~70% of bytes.
- **Google Fonts payload could be trimmed.** Familjen Grotesk + Space Mono are always used; the switcher alternates (Sora/Onest/Hanken) are loaded eagerly. Loading them on-click would shave the initial font payload.
- **Clues 02–04 use stripe placeholders.** Drop the real photos into `assets/` and update `--photo` per card.

---

## Assets

| File | Description | Origin |
|---|---|---|
| `assets/hare-yellow.png` | Single-line hare/rabbit profile, recolored to amber (`#FFD16B`) on transparent background. | Derived from a user-supplied black-line drawing; recolored + alpha-masked. |
| `assets/clue-lime.jpg` | Photo of limes (Clue 01 image). | User-supplied photo, downscaled/compressed. |

---

## License

Personal project. Code is free to learn from; the imagery and copy are private to the hazen.
