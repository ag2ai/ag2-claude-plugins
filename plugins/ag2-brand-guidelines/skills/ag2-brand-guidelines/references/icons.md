# Icons

## Library

AG2 uses **[Iconify](https://iconify.design)** with the **Fluent UI Regular 24**
set as the canonical icon system for React components.

- Package: `@iconify/react`
- Set: [`fluent:*-24-regular`](https://icones.js.org/collection/fluent?variant=Regular+24)
- Install: `npm i @iconify/react` (or `pnpm add`, `yarn add`)

Fluent Regular 24 is the default. Other Iconify sets (Lucide, Tabler, Phosphor)
are acceptable when a concept is unavailable in Fluent, but prefer Fluent for
consistency.

## Why Iconify

- 200k+ icons, tree-shakable per-icon at runtime
- On-demand fetch with built-in IndexedDB cache (zero network after first load)
- SSR-safe, works with Next.js and Vite out of the box
- No SVG copy-pasting into JSX

## Usage

```tsx
import { Icon } from '@iconify/react';

<Icon icon="fluent:brain-circuit-24-regular" width={32} height={32} />
```

### Sizing

| Context                    | Size  |
|----------------------------|-------|
| Inline with 14–16px text   | 16    |
| Inline with 18–20px text   | 20    |
| Card icon (Problem/Feature)| 32    |
| Hero / empty-state         | 48–64 |

### Color

Icons inherit `currentColor`. Set color via the parent:

```tsx
<span style={{ color: 'var(--c-dark)' }}>
  <Icon icon="fluent:flash-24-regular" width={32} />
</span>
```

For AG2 dark mode: `currentColor` should be `var(--color-white)` or a brand
accent (`var(--color-primary)` for highlights).

For AG2 light-mode campaign pages: use `var(--c-dark)` (#1D1C1B).

## Canonical concept → icon mapping

Use these icons for the matching concept on any AG2 page. When a new concept
appears, pick the closest Fluent Regular 24 match from
[icones.js.org/collection/fluent](https://icones.js.org/collection/fluent?variant=Regular+24)
and add it here.

### Product / agents

| Concept                   | Icon                                      |
|---------------------------|-------------------------------------------|
| AI / agents / reasoning   | `fluent:brain-circuit-24-regular`         |
| Multi-agent / workflow    | `fluent:flow-24-regular`                  |
| Code / engineering        | `fluent:code-24-regular`                  |
| Bot / assistant           | `fluent:bot-24-regular`                   |
| Terminal / CLI            | `fluent:window-console-24-regular`        |

### Signals / data

| Concept                   | Icon                                      |
|---------------------------|-------------------------------------------|
| Data trending up          | `fluent:data-trending-24-regular`         |
| Chart / analytics         | `fluent:chart-multiple-24-regular`        |
| Insights                  | `fluent:lightbulb-24-regular`             |
| Search / discovery        | `fluent:search-24-regular`                |
| Filter / classify         | `fluent:filter-24-regular`                |

### Problem / pain

| Concept                   | Icon                                      |
|---------------------------|-------------------------------------------|
| Alert / warning           | `fluent:warning-24-regular`               |
| Error / broken            | `fluent:error-circle-24-regular`          |
| Opaque / hidden           | `fluent:eye-off-24-regular`               |
| Slow / delay              | `fluent:clock-24-regular`                 |
| Cost / bleed              | `fluent:money-dismiss-24-regular`         |
| Fragmentation             | `fluent:puzzle-piece-24-regular`          |

### Benefits / outcomes

| Concept                   | Icon                                      |
|---------------------------|-------------------------------------------|
| Speed / real-time         | `fluent:flash-24-regular`                 |
| Secure / compliance       | `fluent:shield-checkmark-24-regular`      |
| Cost savings              | `fluent:money-24-regular`                 |
| Success / verified        | `fluent:checkmark-circle-24-regular`      |
| Growth                    | `fluent:arrow-growth-24-regular`          |
| Customers / people        | `fluent:people-24-regular`                |
| Reliability / uptime      | `fluent:shield-24-regular`                |
| Global / scale            | `fluent:globe-24-regular`                 |

### Navigation / UI

| Concept                   | Icon                                      |
|---------------------------|-------------------------------------------|
| Arrow forward / CTA       | `fluent:arrow-right-24-regular`           |
| External link             | `fluent:open-24-regular`                  |
| Copy                      | `fluent:copy-24-regular`                  |
| Close                     | `fluent:dismiss-24-regular`               |
| Menu                      | `fluent:navigation-24-regular`            |

### Social

| Concept                   | Icon                                      |
|---------------------------|-------------------------------------------|
| X / Twitter               | `fluent:arrow-trending-lines-24-regular` (or `ri:twitter-x-fill`) |
| LinkedIn                  | `mdi:linkedin` (Fluent has no brand mark) |
| GitHub                    | `mdi:github`                              |
| Discord                   | `ic:baseline-discord`                     |

Brand marks (GitHub, LinkedIn, Discord, X) don't exist in Fluent — use
`mdi:*`, `ri:*`, or `simple-icons:*` from Iconify for those.

## Rules

- **Always** use `@iconify/react`, not inline SVG strings, in React output.
- **Always** use Fluent Regular 24 as the default set.
- Do **not** recreate icons with hand-drawn SVG paths in JSX — pick an Iconify icon.
- Icon stroke weight = Regular (not Filled) unless an emphasis state requires Filled.
- Pair icon sizing with text scale (16/20/32/48) — never arbitrary px values.
- Color via `currentColor` on the parent — don't set `color` prop on `<Icon>`.
