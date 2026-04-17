# Campaign Landing Page Pattern

Canonical structure for AG2 campaign / product-announcement landing pages.
Use this pattern whenever the user provides content for a campaign landing
page (problem, stats, product flow, feature cards, CTA). Substitute the
content into the section skeletons below — do not alter the structural
markup, spacing, or color application.

Source: Figma AG2 – 2026, node 310:2240 ("Campaign"). This is a **light-mode**
variant of the AG2 brand, used for campaign / product marketing pages.

**Icons:** Use `@iconify/react` with Fluent UI Regular 24
(`fluent:*-24-regular`). Never hand-draw SVG paths. Full mapping and rules
in `references/icons.md`.

---

## Page-Level Specs

- Viewport / outer frame: `1600px` wide background, `1300px` centered content
  column with `150px` side gutters.
- Background: `#FFFFFF`. Section dividers: `1px` left + right borders in
  `rgba(29,28,27,0.1)` running the full height of the content column.
- Default text: `#1D1C1B`. Secondary text uses `opacity: 0.8`; captions
  `opacity: 0.64`.
- Section padding: `80px` top, `40–80px` bottom, `40px` horizontal inside the
  1300px column.
- Inter-section dividers: `1px rgba(29,28,27,0.1)` bottom border between
  stacked sections.

---

## Section Order (top → bottom)

1. **Nav** — logo left, single primary CTA right, bottom border.
2. **Hero** — headline + sub + primary CTA + three checkmark benefit chips
   + product screenshot block with grid overlay.
3. **Why This Matters** — 3 stat columns on `#FCFFE9` (soft yellow) band.
4. **Built by AG2** — trust headline + 9–10 partner logo grid.
5. **The Problem** — 3 icon cards on `rgba(155,221,255,0.1)` (blue tint).
6. **How [Product] Works** — 3 numbered step cards with blue pill badges.
7. **What You Get** — 2×3 feature grid on `rgba(213,155,255,0.1)` (pink tint).
8. **CTA Footer** — headline + sub + primary CTA on `#F3FF9B` (primary) band
   with decorative spiral background (`cta-spiral.png` served from plugin
   CDN, bleeds above the top edge — see section 8 for exact sizing).
9. **Footer** — copyright left, 4 social icons right, with dotted dividers.

---

## Font Loading

The campaign page uses **Geist only** — no Alpha Lyrae, no Inter. Headings
and body both use Geist (400 Regular, 500 Medium). Verified against Figma
node `442:4117` — every text layer in the campaign frame resolves to
`Geist:Regular` or `Geist:Medium`. Drop these into `<head>` **before** any
stylesheet that consumes `var(--font-body)`:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Geist:wght@400;500&display=swap">
```

Source: https://fonts.google.com/specimen/Geist. Only 400 and 500 are used —
don't pull extra weights or Geist Mono.

---

## Asset URLs — brand vs. project content

The plugin distinguishes two asset tiers, and generated code must treat
them differently. **Do not collapse these categories** — shipping a
generic trust bar of Fortune 500 logos with every campaign is wrong; so
is hardcoding AG2's mascots into each consumer's `public/` folder.

### Tier 1 — Brand assets (CDN, plugin-owned)

AG2-owned, identity-level, never vary by campaign (mascots, spiral,
AG2 logo, dot-pattern). These are served from jsDelivr off this
plugin's GitHub repo. Generated components reference them **directly by
absolute URL** — no copy step, renders correctly on first run.

One base URL to pin — change this line (and only this line) if the
asset CDN ever moves (e.g. to `assets.ag2.ai` on Cloudflare R2):

```
ASSET_BASE = https://cdn.jsdelivr.net/gh/ag2ai/ag2-claude-plugins@main/plugins/ag2-brand-guidelines/skills/ag2-brand-guidelines/references/assets
```

Every brand-asset `<img>` in generated code uses `${ASSET_BASE}/…`:

| Asset              | CDN path                       |
|--------------------|--------------------------------|
| Hero mascots       | `${ASSET_BASE}/mascot-1.png` … `mascot-5.png` |
| Mascot sprite      | `${ASSET_BASE}/mascots-sprite.png`            |
| CTA spiral         | `${ASSET_BASE}/cta-spiral.png`                |
| AG2 logo (dark)    | `${ASSET_BASE}/ag2-logo-dark.svg`             |
| AG2 logo (white)   | `${ASSET_BASE}/ag2-logo-white.svg`            |

Pinning is `@main`, not a version tag — campaign pages are one-offs,
we want brand refreshes to propagate automatically. Revisit only if
immutability ever becomes a requirement.

### Tier 2 — Project content (consumer-supplied)

Customer/partner logos, product screenshots, campaign-specific imagery.
These **vary per campaign** (a developer-tools product doesn't want a
bank-industry logo grid) and must come from the consumer's project,
not the plugin.

Generated code emits **TODO placeholders** at `/logos/customer-N.png`
with a comment telling the developer to replace them. The consumer
provides the real files in their own `public/logos/` folder.

### Required project assets

When a consumer adopts a generated campaign page, they must add these
files to their project's public folder before the page is
production-ready:

- `public/logos/customer-1.png` … `customer-9.png` — partner/customer
  logos for the "Built by AG2" section. Recommended: PNG, ~129×64px,
  transparent background, monochrome grey (`rgba(29,28,27,0.6)` works
  well). 9 slots by default; the grid tolerates 3–10.

Until the consumer supplies these, the logo grid renders with broken
image icons. That's intentional — a placeholder logo grid is more
honest than a fake one.

---

## CSS Tokens (campaign-specific)

```css
/* Geist is loaded via the Google Fonts <link> tags in the Font Loading
   section above — do NOT add a local @font-face here. Alpha Lyrae is NOT
   used on the campaign landing; headings and body both use Geist. */

:root {
  --c-dark: #1D1C1B;
  --c-white: #FFFFFF;
  --c-primary: #F3FF9B;
  --c-soft-yellow: #FCFFE9;
  --c-blue: #9BDDFF;
  --c-blue-tint: rgba(155,221,255,0.1);
  --c-pink-tint: rgba(213,155,255,0.1);
  --c-border: rgba(29,28,27,0.1);
  --c-text-80: rgba(29,28,27,0.8);
  --c-text-64: rgba(29,28,27,0.64);
  --c-blue-btn-bg: rgba(155,221,255,0.5);
  --c-blue-btn-border: rgba(155,221,255,0.64);
  --c-blue-btn-text: #002F48;

  /* Single family for this page — headings and body both use Geist.
     --font-display is kept as an alias so section CSS below can stay
     unchanged, but it points at the same stack. */
  --font-body: "Geist", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  --font-display: var(--font-body);

  --dot-grid: radial-gradient(circle, rgba(29,28,27,0.1) 1px, transparent 1px);
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
body {
  font-family: var(--font-body);
  font-size: 16px;
  line-height: 1.5;
  color: var(--c-dark);
  background: var(--c-white);
  font-feature-settings: 'cv09' 1, 'ss11' 1, 'liga' 0;
  -webkit-font-smoothing: antialiased;
}
```

---

## 1. Nav

```html
<header class="c-nav">
  <div class="c-nav-inner">
    <a href="#" class="c-logo" aria-label="AG2">
      <!-- paste 90x50 AG2 dark logo SVG from SKILL.md (fill #000001) -->
    </a>
    <a href="#" class="c-btn-dark">Get Started</a>
  </div>
</header>
```

```css
.c-nav { border-bottom: 1px solid var(--c-border); }
.c-nav-inner {
  max-width: 1300px; margin: 0 auto; padding: 24px;
  display: flex; align-items: center; justify-content: space-between;
}
.c-btn-dark {
  background: var(--c-dark); color: var(--c-white);
  font: 500 15px/1 var(--font-body);
  height: 34px; padding: 0 14px;
  display: inline-flex; align-items: center; border-radius: 48px;
  text-decoration: none;
}
```

---

## Campaign Grid Overlay (hero-scoped)

**The campaign page has its own grid — do NOT use `grid-overlay.md`.** That
spec is for the dark landing-page grid (12 H-lines). The campaign grid is
different and **scoped to the Hero section only** (Figma Hero frame is
1453px tall — the grid terminates at the bottom of the hero, before the
"Why This Matters" section begins).

- Lives inside `.c-hero` (absolute, `inset: 0`), **not** page-level.
- **Coordinate translation:** Figma's y-values are **page-relative** (measured
  from the top of the Campaign frame, where the nav occupies y=0→88). When
  the HTML puts the nav as a separate element above `.c-hero`, subtract
  **88px** from every Figma y-value to get the hero-relative CSS `top`:
  | Figma y (page) | hero-relative (CSS `top`) |
  |---|---|
  | 152 (first H-line) | 64 |
  | 87 (V-lines start) | 0 (was -1, snap to 0) |
  | 171 (mascots / content start) | 83 (hero padding-top) |
  | 673 (band 1) | 585 |
  | 1388 (band 2) | 1300 |
- **21 horizontal lines**, **64px flex-gap** (with 1px line = 65px cadence),
  starting at `top: 64px`, spanning the full 1300px hero width.
- **19 vertical lines**, **64px flex-gap**, centered horizontally, at
  `top: 0` with `height: 1365px` — they do NOT span the full hero.
- **Two 64px-tall dotted bands** at `top: 585px` and `top: 1300px` —
  `--dot-grid` (6×6px) with **10% opacity**.
- The 1px left/right hero borders serve as the full-height edge rules.
- Source: Figma node `310:2277` (line frame inside Hero `312:2462`).

```html
<section class="c-hero">
  <div class="c-hero-grid" aria-hidden="true">
    <div class="c-hero-grid-h-lines">
      <div class="c-hero-grid-h-line"></div><!-- repeat ×21 -->
    </div>
    <div class="c-hero-grid-v-lines">
      <div class="c-hero-grid-v-line"></div><!-- repeat ×19 -->
    </div>
    <!-- Figma y=673, 1388 (page) → subtract 88px nav -->
    <div class="c-hero-grid-band" style="top:585px"></div>
    <div class="c-hero-grid-band" style="top:1300px"></div>
  </div>
  <!-- mascots, title, CTA, chips, visual — all with position:relative; z-index:1 -->
</section>
```

```css
.c-hero { position: relative; overflow: hidden; }

.c-hero-grid {
  position: absolute; inset: 0;
  pointer-events: none; z-index: 0; overflow: hidden;
}

.c-hero-grid-h-lines {
  position: absolute;
  left: 0; width: 100%;
  top: 64px;   /* Figma y=152 page-relative → 152-88 nav = 64 hero-relative */
  display: flex; flex-direction: column; gap: 64px;
}
.c-hero-grid-h-line {
  height: 1px; width: 100%;
  background: rgba(29,28,27,0.1);
  flex-shrink: 0;
}

.c-hero-grid-v-lines {
  position: absolute;
  left: calc(50% - 585.5px);   /* 19 lines × 1px + 18 × 64 gap = 1171px */
  width: 1171px;
  top: 0;                      /* Figma y=87 page-relative → 87-88 ≈ 0 */
  height: 1365px;
  display: flex; gap: 64px; align-items: center;
}
.c-hero-grid-v-line {
  width: 1px; height: 100%;
  background: rgba(29,28,27,0.1);
  flex-shrink: 0;
}

.c-hero-grid-band {
  position: absolute;
  left: 1px; width: 1298px; height: 64px;
  background-image: var(--dot-grid);
  background-size: 6px 6px;
  opacity: 0.1;
}

.c-hero > *:not(.c-hero-grid) { position: relative; z-index: 1; }

@media (max-width: 1024px) {
  .c-hero-grid { display: none; }
}
```

Sections below the hero (Why This Matters, Built by AG2, The Problem, etc.)
do **not** have the grid behind them — they render on plain backgrounds.

---

## 2. Hero

The hero (1300×1453 in Figma) contains the headline, sub, CTA, chips, and
the big 649px product visual. The hero-scoped grid above runs behind all
of that and terminates at the hero bottom.

A row of 5 small 30×30 mascot icons sits centered above the headline.

```html
<section class="c-hero">
  <!-- Mascots are brand assets — served from the plugin CDN (see "Asset
       URLs" section above). ASSET_BASE expands to the jsdelivr URL. -->
  <div class="c-hero-mascots">
    <img src="${ASSET_BASE}/mascot-1.png" alt="" width="30" height="30">
    <img src="${ASSET_BASE}/mascot-2.png" alt="" width="30" height="30">
    <img src="${ASSET_BASE}/mascot-3.png" alt="" width="30" height="30">
    <img src="${ASSET_BASE}/mascot-4.png" alt="" width="30" height="30">
    <img src="${ASSET_BASE}/mascot-5.png" alt="" width="30" height="30">
  </div>

  <h1 class="c-hero-title">{{HEADLINE}}</h1>
  <p class="c-hero-sub">
    {{SUB_LINE_1}}<br>{{SUB_LINE_2}}
  </p>

  <div class="c-hero-cta-wrap">
    <a href="#" class="c-btn-dark-lg">{{CTA_LABEL}}</a>
    <p class="c-hero-cta-note">{{CTA_NOTE}}</p>
  </div>

  <div class="c-hero-chips">
    <span class="c-chip"><svg class="c-check">…</svg>{{CHIP_1}}</span>
    <span class="c-chip"><svg class="c-check">…</svg>{{CHIP_2}}</span>
    <span class="c-chip"><svg class="c-check">…</svg>{{CHIP_3}}</span>
  </div>

  <div class="c-hero-visual">
    <!-- product screenshot / illustration block; see §Visual Block below -->
  </div>
</section>
```

```css
.c-hero {
  position: relative;
  max-width: 1300px; margin: 0 auto;
  padding: 152px 66px 64px;
  border-left: 1px solid var(--c-border);
  border-right: 1px solid var(--c-border);
  text-align: center;
}
.c-hero-mascots { display: flex; gap: 35px; justify-content: center; margin-bottom: 32px; }
.c-hero-title {
  font-family: var(--font-display); font-weight: 500;
  font-size: 40px; line-height: 1.2; letter-spacing: -1.2px;
  max-width: 748px; margin: 0 auto 8px;
}
.c-hero-sub {
  max-width: 748px; margin: 0 auto 32px;
  font-size: 16px; color: var(--c-text-80);
}
.c-btn-dark-lg {
  background: var(--c-dark); color: var(--c-white);
  font: 500 17px/1 var(--font-body);
  padding: 11.5px 20px; border-radius: 48px;
  display: inline-flex; text-decoration: none;
}
.c-hero-cta-note { font-size: 12px; color: var(--c-text-80); margin-top: 17px; }
.c-hero-chips {
  display: flex; gap: 24px; justify-content: center;
  margin-top: 130px; padding: 0 24px;
}
.c-chip { display: inline-flex; align-items: center; gap: 4px; font-size: 14px; }
.c-check { width: 20px; height: 20px; flex-shrink: 0; }
.c-hero-visual {
  margin-top: 130px;
  height: 649px;
  background: #F7F5F0;
  position: relative; overflow: hidden;
}
```

Use a checkmark icon (dark stroke, 20×20) or the existing inline SVG.

---

## 3. Why This Matters — 3 Stats on Soft Yellow

```html
<section class="c-section c-section--softyellow">
  <h2 class="c-h6">Why This Matters</h2>
  <div class="c-stats">
    <div class="c-stat">
      <p class="c-stat-value">{{STAT_1_VALUE}}</p>
      <p class="c-stat-copy">{{STAT_1_COPY}}</p>
      <p class="c-stat-source">{{STAT_1_SOURCE}}</p>
    </div>
    <!-- repeat ×3 -->
  </div>
</section>
```

```css
.c-section { max-width: 1300px; margin: 0 auto; padding: 80px 0;
  border-left: 1px solid var(--c-border); border-right: 1px solid var(--c-border);
  border-bottom: 1px solid var(--c-border);
  display: flex; flex-direction: column; align-items: center; gap: 32px; text-align: center; }
.c-section--softyellow { background: var(--c-soft-yellow); }

.c-h6 { font-family: var(--font-display); font-weight: 500;
  font-size: 24px; line-height: 1.2; letter-spacing: -0.48px; }

.c-stats { display: flex; width: 100%; }
.c-stat { flex: 1; padding: 0 40px; text-align: left;
  display: flex; flex-direction: column; gap: 8px; }
.c-stat-value { font-family: var(--font-display); font-weight: 500;
  font-size: 64px; line-height: 1.2; letter-spacing: -2.56px; }
.c-stat-copy { font-size: 16px; color: var(--c-text-80); }
.c-stat-source { font-size: 12px; color: var(--c-text-64); }
```

---

## 4. Built by AG2 — Logo Grid

```html
<section class="c-section c-section--plain c-section--left">
  <div class="c-intro">
    <h3 class="c-h5">Built by AG2</h3>
    <p class="c-lead">{{TRUST_SUB}}</p>
  </div>
  <div class="c-logos">
    <!-- 9–10 customer/partner logo cells, each 129×64 with 1px border.
         TODO: replace these placeholders with the real customer list for
         THIS campaign. Customer logos are project content, not brand —
         they do NOT ship with the plugin. Drop the PNGs into the
         consumer project at `public/logos/customer-N.png`. See the
         "Required project assets" section at the top of this doc. -->
    <div class="c-logo-cell"><img src="/logos/customer-1.png" alt="Customer 1"></div>
    <div class="c-logo-cell"><img src="/logos/customer-2.png" alt="Customer 2"></div>
    <div class="c-logo-cell"><img src="/logos/customer-3.png" alt="Customer 3"></div>
    <div class="c-logo-cell"><img src="/logos/customer-4.png" alt="Customer 4"></div>
    <div class="c-logo-cell"><img src="/logos/customer-5.png" alt="Customer 5"></div>
    <div class="c-logo-cell"><img src="/logos/customer-6.png" alt="Customer 6"></div>
    <div class="c-logo-cell"><img src="/logos/customer-7.png" alt="Customer 7"></div>
    <div class="c-logo-cell"><img src="/logos/customer-8.png" alt="Customer 8"></div>
    <div class="c-logo-cell"><img src="/logos/customer-9.png" alt="Customer 9"></div>
  </div>
</section>
```

```css
.c-section--plain { background: var(--c-white); }
.c-section--left { text-align: left; align-items: stretch; }
.c-intro { padding: 0 40px; }
.c-h5 { font-family: var(--font-display); font-weight: 500;
  font-size: 32px; line-height: 1.2; letter-spacing: -0.64px; margin-bottom: 8px; }
.c-lead { font-size: 16px; color: var(--c-text-80); }

.c-logos { display: flex; padding: 0 40px; }
.c-logo-cell { flex: 1; height: 64px; border: 1px solid var(--c-border);
  margin-left: -1px; display: flex; align-items: center; justify-content: center; }
.c-logo-cell:first-child { margin-left: 0; }
.c-logo-cell img {
  max-height: 32px; max-width: 100px; object-fit: contain;
  filter: grayscale(1);   /* Figma "Built by AG2" row uses mono logos */
}
```

---

## 5. The Problem — 3 Icon Cards on Blue Tint

Icons use **`@iconify/react`** + Fluent Regular 24. Pick concept-appropriate
icons from `references/icons.md` (Problem / pain section). Do NOT hand-draw
SVG paths.

```tsx
import { Icon } from '@iconify/react';

<section className="c-section c-section--bluetint c-section--left">
  <h3 className="c-h5" style={{padding:'0 40px'}}>The Problem</h3>
  <div className="c-cards-3">
    <article className="c-card">
      <Icon icon="fluent:puzzle-piece-24-regular" width={32} height={32} className="c-card-icon" />
      <h4 className="c-card-title">{PROB_1_TITLE}</h4>
      <p className="c-card-body">{PROB_1_BODY}</p>
    </article>
    {/* repeat ×3 with different Fluent icons */}
  </div>
</section>
```

```css
.c-section--bluetint { background: var(--c-blue-tint); padding: 80px 0 40px; }

.c-cards-3 { display: flex; padding: 0 40px; }
.c-card { flex: 1; padding: 40px; border: 1px solid var(--c-border);
  margin-left: -1px; display: flex; flex-direction: column; gap: 24px; }
.c-card:first-child { margin-left: 0; }
.c-card-icon { color: var(--c-dark); }
.c-card-title { font: 500 18px/1.5 var(--font-body); }
.c-card-body { font-size: 16px; color: var(--c-text-80); }
```

---

## 6. How It Works — 3 Numbered Steps

Same `.c-cards-3` layout as §5, but on plain white and each card leads with
a blue glass-shine number badge (01 / 02 / 03).

```html
<section class="c-section c-section--plain c-section--left" style="padding:80px 0 40px">
  <h3 class="c-h5" style="padding:0 40px">How {{PRODUCT}} Works</h3>
  <div class="c-cards-3">
    <article class="c-card">
      <span class="c-badge-blue">01</span>
      <h4 class="c-card-title">{{STEP_1_TITLE}}</h4>
      <p class="c-card-body">{{STEP_1_BODY}}</p>
    </article>
    <!-- 02, 03 -->
  </div>
</section>
```

```css
.c-badge-blue {
  width: 48px; height: 48px; border-radius: 100px;
  background: var(--c-blue-btn-bg);
  border: 1px solid var(--c-blue-btn-border);
  display: inline-flex; align-items: center; justify-content: center;
  color: var(--c-blue-btn-text); font: 500 16px/1 var(--font-body);
  box-shadow: inset 0 2px 12px rgba(255,255,255,0.25);
  position: relative;
}
.c-badge-blue::before {
  content: ''; position: absolute; inset: 1px; border-radius: 99px;
  border: 1px solid #fff; pointer-events: none;
}
```

---

## 7. What You Get — 2×3 Feature Grid on Pink Tint

Each of the 6 feature cards leads with a `@iconify/react` Fluent icon (see
the Benefits / outcomes section in `references/icons.md`).

```tsx
import { Icon } from '@iconify/react';

<section className="c-section c-section--pinktint c-section--left" style={{padding:'80px 0 40px'}}>
  <h3 className="c-h5" style={{padding:'0 40px'}}>What You Get</h3>
  <div className="c-grid-2x3">
    <div className="c-cards-3">
      <article className="c-card">
        <Icon icon="fluent:flash-24-regular" width={32} height={32} className="c-card-icon" />
        <h4 className="c-card-title">{FEAT_1_TITLE}</h4>
        <p className="c-card-body">{FEAT_1_BODY}</p>
      </article>
      {/* feature 2, 3 */}
    </div>
    <div className="c-cards-3">
      {/* feature 4, 5, 6 */}
    </div>
  </div>
</section>
```

```css
.c-section--pinktint { background: var(--c-pink-tint); }
.c-grid-2x3 { display: flex; flex-direction: column; width: 100%; }
.c-grid-2x3 .c-cards-3 + .c-cards-3 .c-card { border-top: 0; }
```

---

## 8. CTA Footer — Primary Yellow Band

The yellow band has a **decorative spiral background** that bleeds above the
section's top edge. In Figma this is node `442:4438` ("bg"): a 1300×701px
image absolutely positioned at `top:-167px, left:0`, clipped by
`overflow:hidden` on the section. The spiral is a brand asset served
from the plugin CDN (`${ASSET_BASE}/cta-spiral.png`) — no local copy
needed. Do **not** substitute a CSS dot-gradient "spiral" — it won't
match Figma and the real file is only ~350KB.

```html
<section class="c-cta-final">
  <div class="c-cta-final-bg" aria-hidden="true">
    <img src="${ASSET_BASE}/cta-spiral.png" alt="">
  </div>
  <div class="c-cta-final-content">
    <h2 class="c-h4">{{CTA_HEADLINE_LINE_1}}<br>{{CTA_HEADLINE_LINE_2}}</h2>
    <p class="c-lead">{{CTA_SUB}}</p>
  </div>
  <a href="#" class="c-btn-blue-glass">{{CTA_LABEL}}</a>
</section>
```

```css
.c-cta-final {
  max-width: 1300px; margin: 0 auto; padding: 80px 40px;
  background: var(--c-primary);
  border-left: 1px solid var(--c-border); border-right: 1px solid var(--c-border);
  display: flex; flex-direction: column; align-items: center; gap: 32px;
  text-align: center;
  position: relative; overflow: hidden;
}
/* Decorative spiral — bleeds above the top padding. Figma: 1300×701,
   top:-167px. The inner img is slightly oversized (103.53%) and offset
   (-1.76%) to match Figma's crop exactly. Do NOT swap for a plain dot
   pattern — that's a different asset. */
.c-cta-final-bg {
  position: absolute; top: -167px; left: 0;
  width: 1300px; height: 701px;
  pointer-events: none; overflow: hidden;
}
.c-cta-final-bg img {
  position: absolute; top: -1.76%; left: 0;
  width: 100%; height: 103.53%;
  max-width: none; object-fit: cover;
}
.c-cta-final-content { position: relative; display: flex; flex-direction: column;
  align-items: center; gap: 8px; }
.c-h4 { font-family: var(--font-display); font-weight: 500;
  font-size: 40px; line-height: 1.2; letter-spacing: -1.2px; }

.c-btn-blue-glass {
  display: inline-flex; align-items: center; justify-content: center;
  height: 48px; padding: 0 24px; border-radius: 100px;
  background: var(--c-blue-btn-bg);
  border: 1px solid var(--c-blue-btn-border);
  color: var(--c-blue-btn-text);
  font: 500 16px/1 var(--font-body);
  text-decoration: none;
  box-shadow: inset 0 2px 12px rgba(255,255,255,0.25);
  position: relative;
}
.c-btn-blue-glass::before {
  content: ''; position: absolute; inset: 1px; border-radius: 99px;
  border: 1px solid #fff; pointer-events: none;
}
.c-btn-blue-glass::after {
  content: ''; position: absolute; inset: 0; border-radius: 100px;
  background: linear-gradient(180deg, transparent, rgba(255,255,255,0.29));
  pointer-events: none;
}
```

---

## 9. Footer

Two dotted 24px bands (6×6 dot texture, 20% opacity) separate the CTA from
the footer; copyright left, 4 social icons right.

```html
<div class="c-dotted-band"></div>
<div class="c-dotted-band"></div>
<footer class="c-footer">
  <div class="c-footer-inner">
    <p class="c-footer-copy">© 2026 AG2. All rights reserved.</p>
    <div class="c-footer-social">
      <a href="#" aria-label="X"><svg width="20" height="20">…</svg></a>
      <a href="#" aria-label="LinkedIn"><svg width="20" height="20">…</svg></a>
      <a href="#" aria-label="GitHub"><svg width="20" height="20">…</svg></a>
      <a href="#" aria-label="Discord"><svg width="20" height="20">…</svg></a>
    </div>
  </div>
</footer>
```

```css
.c-dotted-band {
  height: 24px;
  border-top: 1px solid var(--c-border);
  border-bottom: 1px solid var(--c-border);
  background-image: var(--dot-grid);
  background-size: 6px 6px;
  opacity: 0.6;
}
.c-footer-inner {
  max-width: 1300px; margin: 0 auto; padding: 16px 24px;
  display: flex; align-items: center; justify-content: space-between;
}
.c-footer-copy { font-size: 14px; }
.c-footer-social { display: flex; gap: 24px; }
.c-footer-social a { color: var(--c-dark); }
```

---

## Content Slot Contract

When the user provides campaign content, map it into these slots:

| Slot                      | Used in      | Notes                                    |
|---------------------------|--------------|------------------------------------------|
| `HEADLINE`                | Hero         | 1 sentence, 40px Geist Medium            |
| `SUB_LINE_1`, `SUB_LINE_2`| Hero         | 2 lines, 16px Geist                      |
| `CTA_LABEL` / `CTA_NOTE`  | Hero, Final  | Label ≤4 words; note ≤60 chars           |
| `CHIP_1..3`               | Hero         | 3 benefit chips, ≤8 words each           |
| `STAT_1..3_VALUE/COPY/SOURCE` | Why Matters | "38%", "$8.9M"-style; source =publication |
| `TRUST_SUB`               | Built by AG2 | 1 line                                   |
| `PROB_1..3_TITLE/BODY`    | The Problem  | Title may wrap 2 lines                   |
| `PRODUCT`                 | How Works    | Product name                             |
| `STEP_1..3_TITLE/BODY`    | How Works    | 3 steps, numbered 01/02/03               |
| `FEAT_1..6_TITLE/BODY`    | What You Get | 6 features on 2×3 grid                   |
| `CTA_HEADLINE_LINE_1..2`  | CTA Footer   | 40px Geist Medium, 2 lines               |
| `CTA_SUB`                 | CTA Footer   | 1 line                                   |

---

## Rules

- **Light mode only** — this pattern uses `#FFFFFF` page bg, unlike the rest
  of AG2 which defaults to dark. Do not invert.
- Preserve the **1300px content column with 1px side borders** through the
  entire page — every section inherits the gutters.
- Section backgrounds rotate in a fixed order: white → soft yellow → white
  → blue tint → white → pink tint → primary. Don't reshuffle.
- Headings use **Geist Medium (500)** with negative letter-spacing
  (`-0.48px` @24px, `-0.64px` @32px, `-1.2px` @40px, `-2.56px` @64px). Body
  copy uses **Geist Regular (400)**. Do not substitute Alpha Lyrae or Inter
  on this page — the campaign variant is single-family by design.
- Icon cards and step cards use the same `.c-card` frame; only the
  interior differs (icon vs numbered badge).
- Primary CTA in hero is **Dark (#1D1C1B) pill** — blue glass-shine CTA is
  reserved for the final yellow band and for the numbered step badges.
