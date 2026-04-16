# Foundations — Tokens

Canonical tokens for the AG2 design system. All Figma-extracted values live
here. Other references import from this file rather than duplicating.

---

## Typography

- Scale: 12 / 14 / 15 / 16 / 18 / 24 / 32 / 40 / 48 / 56 / 64
- Display font: **Alpha Lyrae** → fallback Inter, SF Pro Display
- Body font: **Geist** → fallback system-ui, -apple-system, "Segoe UI"
- Mono font: **Geist Mono** → fallback JetBrains Mono, Fira Code
- Weights: 400 body, 500 headings/labels/buttons, 600 glass-shine button text,
  700 inline emphasis only
- Display tracking: -4% @ 64px, -3% @ 40–56px, -2% @ 24–32px
- Display line-height: 1.00 @ 64px, 1.20 @ 24–56px
- Body line-height: 1.50
- Code line-height: 1.60
- Geist feature settings (always apply): `'cv09' 1, 'ss11' 1, 'liga' 0`
- Minimum Alpha Lyrae size: **24px** — never smaller

### Font loading
```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/geist@1.0.0/dist/fonts/geist-sans/style.css">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/geist@1.0.0/dist/fonts/geist-mono/style.css">
```

---

## Color Palette

Dark-mode-first. Neutrals have warm brown undertones — no cool blue-grays.

| Token       | Hex       | Role                                                     |
|-------------|-----------|----------------------------------------------------------|
| primary     | `#F3FF9B` | Heading highlights, emphasis, hover borders, badges. Never a CTA fill. |
| soft-yellow | `#FCFFE9` | Muted highlights, insight labels, subtle badges          |
| green       | `#9BFFA3` | Success, "Running" status, secondary accents             |
| blue        | `#9BDDFF` | Primary CTA buttons, info states, interactive elements   |
| pink        | `#D59BFF` | Tertiary accent, "Building" status, decorative tags      |
| dark        | `#1D1C1B` | Page bg, all card/panel bgs, text on light surfaces      |
| white       | `#FFFFFF` | Text on dark surfaces                                    |

### Surfaces
| Token               | Value                    | Usage                                  |
|---------------------|--------------------------|----------------------------------------|
| surface-page        | `#1D1C1B`                | Page + card backgrounds (single tone)  |
| surface-subtle      | `rgba(255,255,255,0.04)` | Insight/detail panels within cards     |
| surface-interactive | `rgba(255,255,255,0.1)`  | Icon buttons, ghost buttons            |

### Text Hierarchy (opacity on white)
| Level       | Opacity | Usage                           |
|-------------|---------|---------------------------------|
| Primary     | 100%    | Headlines, primary labels       |
| Body        | 80%     | Descriptions, subtitles         |
| Muted       | 64%     | Metadata, dot separators        |
| Placeholder | 20%     | Input placeholder text          |

### Borders
| Token          | Value                    | Usage                              |
|----------------|--------------------------|------------------------------------|
| border-subtle  | `rgba(255,255,255,0.1)`  | Standard cards, grid, nav          |
| border-active  | `#F3FF9B`                | Hover/active card border (+ glow)  |
| border-corner  | `#7E7C7B`                | Corner brackets on featured panels |
| border-outline | `rgba(255,255,255,0.2)`  | Faint outline for dark-on-dark     |

### Category Colors (use-case tags)
| Category  | Color     |
|-----------|-----------|
| OSS       | `#9BDDFF` |
| Community | `#9BFFD4` |
| Investor  | `#CF9BFF` |
| B2B       | `#FFBC9B` |
| Consumer  | `#FF9B9B` |
| Gaming    | `#9BFFA7` |
| Telecom   | `#ACFF9B` |
| Pharma    | `#FF9BAF` |

### Brand Gradient
```css
linear-gradient(96deg, #F3FF9B 1%, #9BFFA3 35%, #9BDDFF 83%, #D59BFF 137%)
```

### Glow System
Neon depth — no traditional drop shadows.

| Name         | Value                                   | Usage                    |
|--------------|-----------------------------------------|--------------------------|
| glow-primary | `0 0 20px rgba(243,255,155,0.15)`       | Yellow halo on highlights |
| glow-card    | `0 0 4px 4px rgba(243,255,155,0.1)`     | Active/hover card state  |
| glow-chat    | `0 0 20px 4px rgba(243,255,155,0.06)`   | Chat input highlight     |
| glow-green   | `0 0 20px rgba(155,255,163,0.15)`       | Success elements         |
| glow-blue    | `0 0 20px rgba(155,221,255,0.15)`       | CTA hover                |

---

## Spacing

8px baseline grid. `3xl` is the Figma-verified grid increment.

| Token | Value | Usage                                 |
|-------|-------|---------------------------------------|
| xs    | 4px   | Tight gaps (tag icon→text)            |
| sm    | 8px   | Button gaps, small padding            |
| md    | 16px  | Card padding, card gaps               |
| lg    | 24px  | Section padding, nav, card grid gaps  |
| xl    | 32px  | Logo→links gap, major component gaps  |
| 2xl   | 48px  | Tablet margins                        |
| 3xl   | 65px  | Grid increment, vertical section gap  |
| 4xl   | 80px  | Large section breaks                  |
| 5xl   | 120px | Hero vertical padding                 |

---

## Border Radius

- **0px (sharp)** — Cards, panels, inputs. Default.
- **48–100px (pill)** — All buttons (nav, CTA, tags).
- **100% (circle)** — Icon buttons (32×32).

---

## Depth / Elevation

| Level       | Treatment                                                                                 | Use                      |
|-------------|-------------------------------------------------------------------------------------------|--------------------------|
| Flat        | No shadow                                                                                 | Page bg, grid lines      |
| Contained   | `1px solid rgba(255,255,255,0.1)`                                                         | Standard cards, nav      |
| Active      | `border-active` + `glow-card`                                                             | Hover/active card state  |
| Chat        | `border-active` + `glow-chat`                                                             | Active chat input        |
| Glass Shine | `inset 0 2px 12px rgba(255,255,255,0.25)` + gradient overlay + double border              | Primary CTA buttons      |

---

## CSS Variables Quick Start

```css
:root {
  --color-primary: #F3FF9B;
  --color-soft-yellow: #FCFFE9;
  --color-green: #9BFFA3;
  --color-blue: #9BDDFF;
  --color-pink: #D59BFF;
  --color-dark: #1D1C1B;
  --color-white: #FFFFFF;

  --color-page-bg: #1D1C1B;
  --color-surface-subtle: rgba(255,255,255,0.04);
  --color-surface-interactive: rgba(255,255,255,0.1);

  --color-border: rgba(255,255,255,0.1);
  --color-border-active: #F3FF9B;
  --color-border-corner: #7E7C7B;

  --color-text-primary: #FFFFFF;

  --font-display: "Alpha Lyrae", "Inter", sans-serif;
  --font-body: "Geist", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  --font-mono: "Geist Mono", "JetBrains Mono", monospace;

  --space-xs: 4px;  --space-sm: 8px;  --space-md: 16px;
  --space-lg: 24px; --space-xl: 32px; --space-2xl: 48px;
  --space-3xl: 65px; --space-4xl: 80px; --space-5xl: 120px;

  --glow-primary: 0 0 20px rgba(243,255,155,0.15);
  --glow-card:    0 0 4px 4px rgba(243,255,155,0.1);
  --glow-chat:    0 0 20px 4px rgba(243,255,155,0.06);
  --glow-green:   0 0 20px rgba(155,255,163,0.15);
  --glow-blue:    0 0 20px rgba(155,221,255,0.15);

  --gradient-brand: linear-gradient(96deg, #F3FF9B 1%, #9BFFA3 35%, #9BDDFF 83%, #D59BFF 137%);
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

body {
  font-family: var(--font-body);
  font-size: 16px;
  line-height: 1.5;
  color: var(--color-text-primary);
  background-color: var(--color-page-bg);
  -webkit-font-smoothing: antialiased;
  font-feature-settings: 'cv09' 1, 'ss11' 1, 'liga' 0;
}

h1, h2, h3, h4, h5, h6 {
  font-family: var(--font-display);
  font-weight: 500;
  line-height: 1.2;
}
h1 { font-size: 56px; letter-spacing: -1.68px; }
h2 { font-size: 48px; letter-spacing: -1.44px; }
h3 { font-size: 40px; letter-spacing: -1.20px; }
h4 { font-size: 32px; letter-spacing: -0.64px; }
h5 { font-size: 24px; letter-spacing: -0.48px; }

code, pre { font-family: var(--font-mono); }
```

---

## Do's & Don'ts

**Do**
- Default to dark backgrounds (`#1D1C1B`)
- Alpha Lyrae for headings with negative letter-spacing
- Geist with `font-feature-settings: 'cv09' 1, 'ss11' 1, 'liga' 0`
- Sharp corners on cards, pills only on buttons
- Corner bracket decorations on featured cards
- Primary yellow for hover/highlights; blue for CTAs
- Opacity-based text hierarchy (100/80/64/20)
- Glow for depth, not drop shadows
- Pixel-art: dot arrows, connection endpoints, mascots

**Don't**
- Light backgrounds as default (campaign page is the explicit exception)
- Alpha Lyrae below 24px
- Primary yellow as CTA button fill
- Rounded corners on cards
- Cool blue-gray neutrals
- Serif or decorative fonts
- Geist without font-feature-settings
- Traditional drop shadows
- Generic icons — prefer pixel-art or Lucide outline

---

## Accessibility

WCAG 2.2 AA; 4.5:1+ text contrast on dark; keyboard-navigable; visible focus
(primary yellow outline); semantic HTML; reduced-motion support; 44×44px+
touch targets.
