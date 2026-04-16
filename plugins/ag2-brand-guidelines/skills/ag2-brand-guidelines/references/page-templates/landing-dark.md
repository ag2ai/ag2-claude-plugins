# Landing Page — Dark (Figma `7:1065`)

Same layout as `landing-light.md`, inverted to dark theme. Same absolute
positions, same grid snap points.

```
[Page container — same structure as light]

[Grid overlay — rgba(255,255,255,0.1) instead of rgba(29,28,27,0.1)]

[Navigation Bar — dark bg (#1D1C1B), white text, white logo]
  AG2 logo (white) | same links, white text
  Contact Sales (ghost, rgba(255,255,255,0.1) bg)
  Request Access (white pill)

[Mascots — same position y=171, same sprite, same sizes]

[Title block — background: #1D1C1B (masks grid)]
  Headline: primary yellow (#F3FF9B) instead of dark
  Subtext: white at 80%

[Button — background: #1D1C1B (masks grid)]
  CTA: white pill (white bg, dark text) or blue glass-shine

[Logo Bar — background: #1D1C1B per slot (masks grid)]
  Logos: ALWAYS grayscale — filter: grayscale(1) brightness(10) contrast(0.5); opacity: 0.6
```
