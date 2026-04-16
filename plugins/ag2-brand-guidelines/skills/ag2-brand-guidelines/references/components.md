# Components

All AG2 component specs. Tokens referenced here live in `foundations.md`.

---

## Navigation — Website

- **Top bar**: 1300px content inside 1600px viewport, 24px padding, 1px
  `border-subtle` bottom border.
- **Logo**: AG2 pixel robot SVG, 40×40 (see `logo-and-mascots.md`).
- **Nav links**: Geist 14px/400, white, 24px gap — Product, Developers,
  Research, Ecosystem, Blog.
- **Secondary button**: ghost pill, `surface-interactive` bg, 34px height,
  radius 999px, Geist 15px/500 — "Contact Sales".
- **Primary button**: white pill, 34px height, radius 48px, dark text,
  Geist 15px/500 — "Request Access".

---

## Navigation — AgentOS App

- **Title bar**: full width, 48px height, macOS traffic lights (left),
  centered "AGENT**OS**" wordmark with logo icon.
- **Sidebar**: 200px width, dark bg, full height below title bar.
- **Nav items**: Geist 14px/500, white, icon + label, 16px left padding.
  Items: Pilot, Twins, Conference, Network, Universe, Marketplace.
- **Active**: blue highlight bg `rgba(155,221,255,0.15)`, blue text.
- **Footer items**: Settings, user name — pinned to bottom.

---

## Buttons

- **Primary CTA (glass-shine)**: blue bg `#9BDDFF`, dark text, pill 48px,
  Geist 17px/500. Glass-shine adds: 0.5px outer border
  `rgba(155,221,255,0.64)`, 1px inner white border,
  `rgba(155,221,255,0.5)` bg, inset shadow `0 2px 12px rgba(255,255,255,0.25)`,
  gradient shine overlay (transparent → 29% white), text-shadow.
- **Small glass-shine** (Send): 32px height, Geist 14px/600, white text,
  same shine treatment.
- **Ghost / secondary**: `surface-interactive` bg, white text, pill.
- **Icon button**: 32×32, circle, `surface-interactive` bg, 16×16 icon.

---

## Cards

- Sharp corners (0px radius) — always.
- `1px solid border-subtle` border.
- Hover: `border-active` + `glow-card`, pixel arrow turns primary yellow,
  "Go to Office" text appears.
- **Corner brackets**: 8px L-shapes at `border-subtle` opacity on all 4
  corners. Featured panels use 20px brackets in `border-corner` (`#7E7C7B`).

### Project Card
- 66px height, 12px 16px padding.
- Left: name (Geist 14px/500) + status tag (Geist 12px/500,
  green=Running, pink=Building).
- Right: pixel-art dot arrow (5 white 3px squares in chevron).
- Variant: name + description (no status), description at 80% opacity.
- Fading list: last items at 50% / 30% opacity with gradient overlay
  fading to page bg.

### Use-Case Card
- 16px padding, horizontal layout.
- Left: 64×64 pixel-art agent avatar.
- Right: title (Geist 18px/500) + category tag (category color) +
  channel count (80%) + description (Geist 14px/400, 80%).
- Corner brackets on all 4 corners.

### Anything Card (Showcase)
- 16px padding, vertical layout.
- Top: 200px gradient thumbnail (brand gradient).
- Middle: title (Geist 18px/500) + description (Geist 14px/400, 80%).
- Bottom: insight panel (`surface-subtle` bg, 14px padding) —
  "Vera's Insight" label in soft-yellow + date at 64% + quote at 80%.
- Corner brackets on all 4 corners.

---

## Chat (AgentOS)

- **Chat thread**: centered in content area (650px wide), messages stacked.
- **User message**: right-aligned, dark bg with `border-subtle`, pill
  radius, Geist 14px/400 white text, timestamp below (Geist 12px, 64%).
- **AI message**: left-aligned, no bg, Geist 14px/400 white text,
  timestamp below.
- **Embedded agent block**: `border-subtle`, 0px radius, icon + agent name
  (Geist 14px/500) + description (Geist 12px/400, 80%), pixel arrow right.
- **Claude quote block**: `surface-subtle` bg, 16px padding, icon +
  "Claude says:" label (Geist 14px/500) + quote (Geist 14px/400).
- **Time divider**: centered timestamp with horizontal rules extending L/R.

---

## Chat Input

- **Website variant**: dark bg, `border-active` border, `glow-chat` shadow,
  129px height. Placeholder at 20%, Geist 16px/400. Bottom-right: row of
  icon buttons (32px circle) + glass-shine Send button.
- **AgentOS variant**: 64px height, dark bg, `border-subtle`,
  Geist 16px/400. Placeholder: `|Talk to Pilot (@ to reference)`.
  Right: icon buttons (attachment, mic, code) + Send (blue glass-shine,
  48×32).

---

## Agent Profile

- **Variant A (horizontal)**: 64×64 mascot + Alpha Lyrae 32px name +
  Geist 14px/400 role at 80%. Used on Vera Landing.
- **Variant B (centered)**: 64×64 mascot + Geist 24px/600 uppercase name +
  Geist 14px/500 role at 80%. Used on Product Agent Detail.

---

## Agent Card (Product — Agent Grid)

- Centered column: 64×64 pixel-art mascot on top, uppercase name below
  (Geist 24px/600, -0.24px tracking), role label (Geist 14px/400, 80%).
- 4 agents in a row with equal spacing.
- Connection lines between agents in brand colors (blue, yellow, white)
  with pixel-dot endpoints.

---

## Connection Lines

- 1px solid lines in brand colors connecting agent cards.
- **Pixel-dot endpoints**: 3 layered squares (15px / 9px / 3px) at
  decreasing opacity. Variants: blue `#9BDDFF`, yellow `#F3FF9B`,
  white `#FFFFFF`.
- Horizontal + vertical lines form an engineering-diagram grid.

---

## Status Indicators

- Running: green `#9BFFA3`, Geist 12px/500
- Building: pink `#D59BFF`, Geist 12px/500

---

## Decorative Elements

- **Pixel arrow**: 5 white 3px squares in chevron `>` (SVG 9×15). Turns
  primary yellow on hover.
- **Dot-grid texture**: 6px repeating pattern at 10% opacity.
- **64px grid overlay**: DOM-based (see `grid-overlay.md`).
- **Logo bar** (landing): row of partner/customer logos — Google, NVIDIA,
  Walmart, AT&T, HSBC, Johnson & Johnson, Flipkart, cegid, MediaTek. On
  dark backgrounds apply
  `filter: grayscale(1) brightness(10) contrast(0.5); opacity: 0.6` to
  render logos muted white. On light backgrounds use full-color. Source
  images in `assets/logo-*.png`.

---

## Badges / Tags

- Category tags: Geist 12px/500, category color, no bg.
- Status tags: Geist 12px/500, status color.
- Accent badges: primary yellow.
