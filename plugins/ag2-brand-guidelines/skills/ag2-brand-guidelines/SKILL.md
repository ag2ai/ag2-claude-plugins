---
name: ag2-brand-guidelines
description: >
  Applies AG2's official design system and brand guidelines to all generated
  code, artifacts, UI components, and visual output. Use this skill whenever
  building web pages, landing pages, dashboards, product UIs, React components,
  HTML artifacts, presentations, or any visual output that should follow
  AG2's brand identity. Always activate this skill when the user mentions
  AG2 brand, design system, brand colors, typography, visual consistency,
  component styling, or wants any UI to look like AG2's product. Even if
  the user doesn't explicitly say "brand guidelines," use this skill for
  any AG2-related visual work.
metadata:
  version: "0.1.0"
---

# AG2 Design System & Brand Guidelines

AG2 is the open-source multi-agent AI framework (formerly AutoGen). The brand
identity is **dark-first, futuristic, and technical** — cutting-edge AI
infrastructure with bold neon accents on deep dark backgrounds.

Apply these guidelines to ALL visual output: HTML, React components, CSS,
landing pages, dashboards, presentations, and any UI code.

---

## Brand Identity

- **Product**: AG2 — Open-Source Multi-Agent AI Framework / AgentOS
- **Tagline**: "Multi-agent intelligence, shipped at scale."
- **Tone**: Technical, bold, futuristic, developer-focused
- **Visual Direction**: Dark-first interfaces, neon accent colors, clean
  geometric layouts, generous whitespace within dark surfaces

## Logo

The AG2 logo is a pixel-art robot mascot with "AG2" wordmark. Two variants
are provided in `references/`:

- **Dark version** (`ag2-logo-dark.svg`): `fill="#000001"` — use on light backgrounds
- **White version** (`ag2-logo-white.svg`): `fill="#ffffff"` — use on dark backgrounds

The SVG is 90x50 viewBox. Always use the official SVG inline — never
approximate or recreate the logo. The dark logo SVG for inline use:

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="90" height="50" fill="none"><path fill="#000001" d="M69.285 0h-3.232v3.232h3.232V0Zm-3.232 16.095h-3.21v6.442h3.21v-6.442Zm0-12.863h-3.21v3.21h3.21v-3.21Zm-3.21 9.652h-3.21v3.21h3.21v-3.21Zm0-6.442h-3.21v3.232h3.21V6.442Zm-3.211 3.232H53.19v3.21h6.442v-3.21ZM53.19 6.442H37.095v3.232H53.19V6.442Zm6.442 19.305v-6.42h-3.231v-3.232H33.885v3.232h-3.232v6.42h28.98Zm-9.652-6.42h3.21v3.21h-3.21v-3.21Zm-12.885 0h3.21v3.21h-3.21v-3.21Zm0-9.653h-6.442v3.21h6.442v-3.21Zm-6.442 3.21h-3.21v3.21h3.21v-3.21Zm0-6.442h-3.21v3.232h3.21V6.442Zm-3.211 9.653h-3.21v6.442h3.21v-6.442Zm0-12.863h-3.21v3.21h3.21v-3.21ZM24.232 0H21v3.232h3.232V0Z"/><path fill="#000" d="M65.867 37.748V34.33H55.615v-3.418h10.252v3.418h3.418v3.417h-3.418Zm-6.834 3.417v-3.417h6.834v3.417h-6.834ZM55.615 48v-6.835h3.418v3.418h10.252V48h-13.67Zm-13.89-13.67v-3.417h10.252v3.418H41.725Zm-3.417 10.253V34.33h3.417v10.252h-3.417Zm10.252 0v-3.418h-3.417v-3.417h6.834v6.835H48.56ZM41.725 48v-3.417h6.835V48h-6.835ZM21 48V34.33h3.417v-3.417h6.835v3.418h3.418V48h-3.418v-6.835h-6.835V48H21Zm3.417-10.252h6.835v-3.28h-6.835v3.28Z"/></svg>
```

For white (dark backgrounds), change both fill values to `#ffffff`.

---

## Color Palette

### 7 Brand Colors

| Token                  | Name         | HEX       | Usage                                        |
|------------------------|--------------|-----------|----------------------------------------------|
| `--color-primary`      | Primary      | #F3FF9B   | CTAs, key highlights, accent badges, links   |
| `--color-soft-yellow`  | Soft Yellow  | #FCFFE9   | Subtle backgrounds, muted highlights, tags   |
| `--color-green`        | Green        | #9BFFA3   | Success states, secondary accents, badges    |
| `--color-blue`         | Blue         | #9BDDFF   | Info states, secondary accents, illustrations|
| `--color-pink`         | Pink         | #D59BFF   | Tertiary accents, decorative elements, tags  |
| `--color-dark`         | Dark         | #1D1C1B   | Primary backgrounds, text on light surfaces  |
| `--color-white`        | White        | #FFFFFF   | Text on dark backgrounds, card surfaces      |

### Extended Neutrals

| Token                  | HEX       | Usage                                        |
|------------------------|-----------|----------------------------------------------|
| `--color-surface`      | #2A2926   | Card backgrounds, elevated surfaces on dark  |
| `--color-surface-alt`  | #333130   | Alternate surface, sidebar backgrounds       |
| `--color-border`       | #3A3836   | Borders on dark backgrounds                  |
| `--color-border-light` | #E5E5E5   | Borders on light backgrounds                 |
| `--color-text-secondary`| #8A8885  | Secondary text on dark backgrounds           |
| `--color-text-muted`   | #6B6865   | Muted text, placeholders on dark backgrounds |
| `--color-page-bg`      | #1D1C1B   | Page-level background                        |

### Color Rules

- Dark backgrounds are the default. Light backgrounds are the exception.
- Primary (#F3FF9B) is used sparingly — CTAs, key labels, active states,
  highlighted text. Never as a large background fill.
- Green (#9BFFA3) for success indicators, positive actions, secondary CTAs.
- Blue (#9BDDFF) and Pink (#D59BFF) are supporting accents for variety in
  illustrations, charts, tags, decorative elements.
- Soft Yellow (#FCFFE9) for muted background highlights or subtle badges.
- Use the 7-color palette exclusively — no random colors.

---

## Typography

### Font Families

- **Display / Headings**: `"Alpha Lyrae", sans-serif` — distinctive geometric
  display typeface for all headings and hero text.
- **Body / UI / Labels**: `"Geist", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`
  — clean modern sans-serif for paragraphs, subheadings, labels, UI elements.
- **Monospace / Code**: `"Geist Mono", "JetBrains Mono", "Fira Code", monospace`
  — for code blocks, terminal output, technical content.

### Heading Scale — Alpha Lyrae

All headings: 120% line height, negative letter spacing.

| Level | Size | Letter Spacing | Default Weight | Usage              |
|-------|------|----------------|----------------|--------------------|
| H1    | 64px | -4%            | 400 Regular    | Hero headlines     |
| H2    | 56px | -3%            | 400 Regular    | Page titles        |
| H3    | 48px | -3%            | 400 Regular    | Section headings   |
| H4    | 40px | -3%            | 400 Regular    | Subsection heads   |
| H5    | 32px | -2%            | 400 Regular    | Card titles        |
| H6    | 24px | -2%            | 400 Regular    | Small headings     |

Weight variants: Regular (400) default, Medium (500) for emphasis, Bold (700) for hero impact.

### Body Scale — Geist

All body text: 150% line height.

| Size | Default Weight | Usage                    |
|------|----------------|--------------------------|
| 32px | 400 Regular    | Lead paragraphs, hero sub |
| 24px | 400 Regular    | Subheadings, intro text  |
| 20px | 400 Regular    | Large body text          |
| 18px | 400 Regular    | Feature descriptions     |
| 16px | 400 Regular    | Standard body text       |
| 14px | 400 Regular    | UI text, secondary       |
| 12px | 400 Regular    | Captions, metadata       |
| 11px | 400 Regular    | Legal, fine print        |

Weight variants: Regular (400) for body, Medium (500) for labels/buttons/nav,
Bold (700) for inline emphasis and key stats.

### Typography Rules

- Headings always use Alpha Lyrae with negative letter spacing.
- Body text always uses Geist — never Alpha Lyrae for body.
- On dark backgrounds, text is White; on light, text is Dark.
- Highlighted text in headings may use Primary (#F3FF9B) for emphasis.
- Never use Alpha Lyrae below 24px.

---

## Spacing, Borders & Shadows

**Base unit**: 8px grid. Tokens: xs(4), sm(8), md(16), lg(24), xl(32),
2xl(48), 3xl(64), 4xl(80), 5xl(120).

**Border radius**: sm(8px), md(12px), lg(16px), xl(24px), full(9999px/pill).

**Shadows on dark**: sm `0 1px 3px rgba(0,0,0,0.3)`, md `0 4px 12px rgba(0,0,0,0.4)`,
lg `0 8px 24px rgba(0,0,0,0.5)`.

**Glow effects**: primary `0 0 20px rgba(243,255,155,0.15)`,
green `0 0 20px rgba(155,255,163,0.15)`, blue `0 0 20px rgba(155,221,255,0.15)`.

---

## Components

### Buttons

- **Primary CTA**: bg Primary (#F3FF9B), text Dark, Geist 14px/500, pill shape,
  padding 12px 28px, hover brightness(1.05) + glow, active brightness(0.95).
- **Green/Blue/Pink**: same sizing as Primary, bg of respective color, text Dark.
- **Secondary**: transparent bg, 1px border `--color-border`, white text,
  pill shape, hover bg `rgba(255,255,255,0.05)`.
- **Ghost**: transparent, white text with underline or arrow, hover opacity 0.8.

### Cards

bg `--color-surface` or `--color-dark`, 1px border `--color-border`,
radius md–xl, padding 24–32px, hover subtle border brightness or glow.
Title: H5/H6 Alpha Lyrae. Body: Text 16/14 Geist Regular.

### Navigation

bg `--color-dark` with transparency + backdrop-blur. Logo left. Links white,
Geist 14px/400, opacity 0.8 → 1.0 hover. Primary pill CTA in nav.

For detailed component specs (form inputs, tags, code blocks), see
`references/components.md`.

---

## Dark Mode (Default) vs Light Mode

AG2 is dark-mode-first. Always default to dark.

| Property         | Dark (Default)  | Light           |
|------------------|-----------------|-----------------|
| Page background  | #1D1C1B         | #FFFFFF         |
| Surface          | #2A2926         | #F5F5F5         |
| Primary text     | #FFFFFF         | #1D1C1B         |
| Secondary text   | #8A8885         | #6B7280         |
| Borders          | #3A3836         | #E5E5E5         |
| Accent colors    | Same            | Same or darker  |

---

## Brand Do's and Don'ts

**Always**:
- Default to dark backgrounds (#1D1C1B)
- Alpha Lyrae for headings, Geist for body
- Negative letter spacing on headings (-2% to -4%)
- Primary (#F3FF9B) as highlight only, not background fill
- Generous spacing between sections (80px+)
- Subtle glow effects on primary CTAs
- 7-color palette exclusively

**Never**:
- Light backgrounds as default
- Alpha Lyrae for body text or below 24px
- Primary as large background fill
- Decorative or serif fonts
- Heavy gradients or drop shadows
- Colors outside the 7-color palette
- Rounded corners larger than 24px (except pills)

---

## Application Checklist

When generating any AG2 UI code:

1. Set dark background: `background: var(--color-dark)`
2. Define all CSS variables (see `references/css-template.md`)
3. Apply Alpha Lyrae to H1–H6 with tight letter spacing
4. Apply Geist to body text, labels, UI elements
5. Correct weights: Regular body, Medium labels/buttons, Bold emphasis
6. Primary (#F3FF9B) only for CTAs, highlights, accents
7. Pill-shaped CTA buttons with brand color bg and Dark text
8. Subtle glow effects on interactive elements
9. Generous spacing — sections breathe (48–120px gaps)
10. 7-color palette only — no random colors
11. White on Dark contrast: 4.5:1+ (WCAG AA)

For the full CSS variables template, see `references/css-template.md`.
For landing page section patterns, see `references/landing-page-pattern.md`.

---

## Design Source

These tokens are the canonical source of truth for this plugin.
All design values are baked in — no Figma access or external tools required.
