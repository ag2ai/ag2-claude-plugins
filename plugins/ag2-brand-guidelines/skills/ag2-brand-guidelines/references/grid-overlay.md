# Grid Overlay

The 65px engineering grid is a **DOM-based overlay** (Figma node `7:992`),
not a CSS background pattern. Always copy this exact structure — do not
approximate.

---

## Grid Constants

| Constant       | Value                  | Notes                                    |
|----------------|------------------------|------------------------------------------|
| Grid gap       | **65px**               | Spacing between all grid lines           |
| Content width  | 1300px                 | Between edge verticals                   |
| Edge left      | `calc(50% - 650px)`    | Left content boundary                    |
| Edge right     | `calc(50% + 649px)`    | Right content boundary                   |
| H-lines start  | `top: 152px`           | First horizontal line position           |
| H-lines count  | 12                     | At y: 152, 217, 282, 347, 412, 477, 542, 607, 672, 737, 802, 867 |
| V-lines start  | `left: calc(50% - 585px)` | 65px inset from left edge             |
| V-lines top    | `top: 88px`            | Aligns with nav bottom border            |
| V-lines count  | 19                     | 65px gap, container 1171×780, lines 868px tall |

---

## Page Structure

```html
<div class="page">
  <!-- 1. Grid overlay FIRST — behind everything -->
  <div class="grid-wrap" aria-hidden="true">…</div>
  <!-- 2. Nav — z-index: 10 -->
  <nav class="nav">…</nav>
  <!-- 3. Content — z-index: 1, snaps to grid lines -->
  …
</div>
```

- `.page { position: relative; width: 100%; min-height: 100vh; }`
- Content blocks that overlap grid lines **must** have
  `background: var(--color-page-bg)` to mask the grid behind them
  (title block, button block, logo slots).

---

## Content Placement on Grid (Landing Pages)

Each H-line is numbered 1–12.

| Element            | Y position | Grid line        | Notes                          |
|--------------------|------------|------------------|--------------------------------|
| Nav                | 0–88       | —                | Border-bottom aligns to V-top  |
| Mascots            | 171        | Between H1 & H2  | Centered in first grid row     |
| Title block        | 218        | Snaps to H2      | Bg mask, 779×259               |
| Button             | 478        | Snaps to H6      | Bg mask, 259×64                |
| *(3 empty rows)*   | —          | H7–H9            | Breathing space                |
| Logo bar           | 738        | Snaps to H10     | 9 slots × 129px, each bg mask  |

---

## Reference HTML

```html
<div class="grid-wrap" aria-hidden="true">
  <div class="grid-edge grid-edge-l"></div>
  <div class="grid-edge grid-edge-r"></div>
  <div class="grid-h-lines">
    <!-- 12 horizontal lines -->
    <div class="grid-h-line"></div><div class="grid-h-line"></div>
    <div class="grid-h-line"></div><div class="grid-h-line"></div>
    <div class="grid-h-line"></div><div class="grid-h-line"></div>
    <div class="grid-h-line"></div><div class="grid-h-line"></div>
    <div class="grid-h-line"></div><div class="grid-h-line"></div>
    <div class="grid-h-line"></div><div class="grid-h-line"></div>
  </div>
  <div class="grid-v-lines">
    <!-- 19 vertical lines -->
    <div class="grid-v-line"></div><div class="grid-v-line"></div>
    <div class="grid-v-line"></div><div class="grid-v-line"></div>
    <div class="grid-v-line"></div><div class="grid-v-line"></div>
    <div class="grid-v-line"></div><div class="grid-v-line"></div>
    <div class="grid-v-line"></div><div class="grid-v-line"></div>
    <div class="grid-v-line"></div><div class="grid-v-line"></div>
    <div class="grid-v-line"></div><div class="grid-v-line"></div>
    <div class="grid-v-line"></div><div class="grid-v-line"></div>
    <div class="grid-v-line"></div><div class="grid-v-line"></div>
    <div class="grid-v-line"></div>
  </div>
</div>
```

---

## Reference CSS (dark — swap colors for light)

```css
.page {
  position: relative;
  width: 100%;
  min-height: 100vh;
}

.grid-wrap {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 0;
  overflow: hidden;
}

/* 2 edge verticals — full page height */
.grid-edge {
  position: absolute;
  top: 0;
  width: 1px;
  height: 100%;
  background: rgba(255,255,255,0.1); /* light: rgba(29,28,27,0.1) */
}
.grid-edge-l { left: calc(50% - 650px); }
.grid-edge-r { left: calc(50% + 649px); }

/* 12 horizontal lines — gap 65px, starting y=152 */
.grid-h-lines {
  position: absolute;
  left: calc(50% - 650px);
  top: 152px;
  width: 1300px;
  height: 716px;
  display: flex;
  flex-direction: column;
  gap: 65px;
}
.grid-h-line {
  height: 1px;
  width: 100%;
  background: rgba(255,255,255,0.1); /* light: rgba(29,28,27,0.1) */
  flex-shrink: 0;
}

/* 19 vertical lines — gap 65px, inset 65px from left, clipped 780px */
.grid-v-lines {
  position: absolute;
  left: calc(50% - 585px);
  top: 88px;
  width: 1171px;
  height: 780px;
  display: flex;
  gap: 65px;
  align-items: center;
  overflow: hidden;
}
.grid-v-line {
  width: 1px;
  height: 868px;
  background: rgba(255,255,255,0.1); /* light: rgba(29,28,27,0.1) */
  flex-shrink: 0;
}
```

---

## Responsive

- Hide grid below 1024px (`display: none`).
- Grid is purely decorative — always `aria-hidden="true"`.

---

## React Component Layout Strategy

When generating React components, use **flow layout** (flexbox/grid +
spacing tokens) instead of absolute positioning. Absolute positioning is
only appropriate for the grid overlay itself.

### Why
- Components must be composable and reusable across pages.
- Content is often dynamic (variable text, conditional sections).
- Pages need natural document flow for scrolling and responsiveness.
- Absolute positioning breaks when content changes.

### Mapping grid positions to flow spacing
Convert the gaps between Figma Y positions into `margin-top` / `padding-top`:

| From → To                              | Figma gap | Flow equivalent                       |
|----------------------------------------|-----------|---------------------------------------|
| Nav bottom (88) → Mascots (171)        | 83px      | `padding-top: 83px` on hero wrapper   |
| Mascots bottom → Title (218)           | ~17px     | `margin-top: 17px` on title block     |
| Title bottom → CTA (478)               | ~65px     | `margin-top: 65px` on CTA (1 row)     |
| CTA → Logo bar (738)                   | ~195px    | `margin-top: 195px` (3 rows)          |

### Rules
1. Grid overlay: absolute positioned (it's a background decoration).
2. Nav: flow (relative, full width, fixed height 88px).
3. All content sections: flow (flex column, centered, margins for spacing).
4. Grid-masking backgrounds still required — content blocks crossing grid
   lines need `background: var(--color-page-bg)`.
5. Spacing values: use multiples of 65px grid gap or the 8px baseline.
