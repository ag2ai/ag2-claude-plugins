# Landing Page — Light (Figma `7:955`)

Marketing homepage on white background. All content is absolutely
positioned to snap to 65px grid lines. See `../grid-overlay.md` for the
grid system and `../components.md` for nav/button specs.

```
[Page container — width: 100%, min-height: 100vh, position: relative]

[Grid overlay — FIRST child, z-index: 0, 65px gap, rgba(29,28,27,0.1)]

[Navigation Bar — y=0, h=88, z-index: 10, full width, white bg, border-bottom]
  Inner: max-width 1300px centered, padding 24px
  AG2 logo (dark, 40x40) | Product, Developers, Research, Ecosystem, Blog
  Right: Contact Sales (ghost, #e9e9e9 bg, pill 999px)
         Request Access (dark pill, 48px radius)

[Mascots — y=171, between grid H1 and H2, z-index: 1]
  5 mascots from sprite sheet, each 30x30, gap 35px (65px center-to-center)
  Group: 290px wide, centered at calc(50% - 145px)
  image-rendering: pixelated

[Title block — y=218, snaps to grid H2, z-index: 1, 779x259, centered]
  MUST have background: white (masks grid lines behind text)
  Headline: Alpha Lyrae 64px/500, dark text, -2.56px tracking, centered
    "Multi-agent intelligence,"
    "shipped at scale."
  Subtext: Geist 18px/400, dark at 80%, centered

[Button — y=478, snaps to grid H6, z-index: 1, 259x64, centered]
  MUST have background: white (masks grid)
  CTA: "Request Access" — dark pill (48px radius, dark bg, white text, h=40)

[3 empty grid rows — breathing space between button and logos]

[Logo Bar — y=738, snaps to grid H10, z-index: 1]
  x: calc(50% - 584px), 1169px wide, 9 slots × 129px with 1px gaps
  Each slot: 129x64, background: white (masks grid)
  Logos: ALWAYS grayscale — filter: grayscale(1) brightness(0.3); opacity: 0.7
  Google, NVIDIA, Walmart, AT&T, HSBC, Johnson & Johnson, Flipkart, cegid, MediaTek
```
