# Logo & Mascots

All brand assets (logo, mascots, decorative art) live in
`references/assets/` **and** are served from the plugin's CDN so
generated code can reference them by URL with no local copy step. See
`page-templates/campaign-landing.md` for the canonical `ASSET_BASE`
declaration. In short:

```
ASSET_BASE = https://cdn.jsdelivr.net/gh/ag2ai/ag2-claude-plugins@main/plugins/ag2-brand-guidelines/skills/ag2-brand-guidelines/references/assets
```

## Logo

Pixel-art robot mascot with "AG2" wordmark. 90×50 SVG viewBox.

- Dark variant: `${ASSET_BASE}/ag2-logo-dark.svg` (fill `#000001`) — light bgs.
- White variant: `${ASSET_BASE}/ag2-logo-white.svg` (fill `#FFFFFF`) — dark bgs.

Source paths in this plugin: `assets/ag2-logo-dark.svg`,
`assets/ag2-logo-white.svg`. Either inline the SVG (best — no network
hop, instant render) or reference via `<img src="${ASSET_BASE}/…">`.
Never approximate or recreate.

Inline SVG (dark):

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="90" height="50" fill="none"><path fill="#000001" d="M69.285 0h-3.232v3.232h3.232V0Zm-3.232 16.095h-3.21v6.442h3.21v-6.442Zm0-12.863h-3.21v3.21h3.21v-3.21Zm-3.21 9.652h-3.21v3.21h3.21v-3.21Zm0-6.442h-3.21v3.232h3.21V6.442Zm-3.211 3.232H53.19v3.21h6.442v-3.21ZM53.19 6.442H37.095v3.232H53.19V6.442Zm6.442 19.305v-6.42h-3.231v-3.232H33.885v3.232h-3.232v6.42h28.98Zm-9.652-6.42h3.21v3.21h-3.21v-3.21Zm-12.885 0h3.21v3.21h-3.21v-3.21Zm0-9.653h-6.442v3.21h6.442v-3.21Zm-6.442 3.21h-3.21v3.21h3.21v-3.21Zm0-6.442h-3.21v3.232h3.21V6.442Zm-3.211 9.653h-3.21v6.442h3.21v-6.442Zm0-12.863h-3.21v3.21h3.21v-3.21ZM24.232 0H21v3.232h3.232V0Z"/><path fill="#000" d="M65.867 37.748V34.33H55.615v-3.418h10.252v3.418h3.418v3.417h-3.418Zm-6.834 3.417v-3.417h6.834v3.417h-6.834ZM55.615 48v-6.835h3.418v3.418h10.252V48h-13.67Zm-13.89-13.67v-3.417h10.252v3.418H41.725Zm-3.417 10.253V34.33h3.417v10.252h-3.417Zm10.252 0v-3.418h-3.417v-3.417h6.834v6.835H48.56ZM41.725 48v-3.417h6.835V48h-6.835ZM21 48V34.33h3.417v-3.417h6.835v3.418h3.418V48h-3.418v-6.835h-6.835V48H21Zm3.417-10.252h6.835v-3.28h-6.835v3.28Z"/></svg>
```

For white (dark backgrounds), change both `fill` values to `#ffffff`.

---

## Agent Mascots

5 pixel-art robot mascots used in the hero section of landing pages. Each
is a 120×120 PNG rendered at 30×30 in the UI with
`image-rendering: pixelated`.

| CDN URL                             | Color           | Figma Node |
|-------------------------------------|-----------------|------------|
| `${ASSET_BASE}/mascot-1.png`        | Purple / violet | `26:245`   |
| `${ASSET_BASE}/mascot-2.png`        | Teal / mint     | `26:244`   |
| `${ASSET_BASE}/mascot-3.png`        | Purple / violet | `26:243`   |
| `${ASSET_BASE}/mascot-4.png`        | Gold / yellow   | `26:242`   |
| `${ASSET_BASE}/mascot-5.png`        | Orange / peach  | `26:241`   |

- Sprite sheet: `${ASSET_BASE}/mascots-sprite.png` (600×120, all 5
  side-by-side, Figma node `26:240`).
- Display at 30×30px or 32×32px with `image-rendering: pixelated`.
- Arrange in a horizontal row with 16px gap.
- Always use the official PNGs via the CDN URL — never approximate or
  recreate the pixel art, and never copy the PNGs into a consuming
  project's `public/` folder (the CDN is the canonical source).

---

## Decorative brand imagery

- CTA spiral (yellow-band background): `${ASSET_BASE}/cta-spiral.png`
  (1300×701 PNG, Figma node `442:4438`). Used in section 8 of the
  campaign landing. See `page-templates/campaign-landing.md` for exact
  sizing / positioning.

---

## Partner / customer logos — NOT a brand asset

Partner logos (Google, NVIDIA, Walmart, etc.) **do not ship with this
plugin**. They are project content — each campaign should display its
own real customer list, not a generic Fortune 500 grid. Generated code
emits TODO placeholders at `/logos/customer-N.png`; the consumer
supplies real PNGs in their own `public/logos/` folder.

Recommended format: PNG, ~129×64px, transparent bg, monochrome grey
(`rgba(29,28,27,0.6)` works well). The `.c-logos` grid tolerates 3–10
cells.

Treatment by background:
- On dark backgrounds:
  `filter: grayscale(1) brightness(10) contrast(0.5); opacity: 0.6`
- On light backgrounds: full-color or the grey treatment above.
