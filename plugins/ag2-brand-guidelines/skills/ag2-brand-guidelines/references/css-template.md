# CSS Variables Template

Define these variables in every AG2 UI project. Include them at the root level.

## Full Variable Set

```css
:root {
  /* Brand Colors */
  --color-primary: #F3FF9B;
  --color-soft-yellow: #FCFFE9;
  --color-green: #9BFFA3;
  --color-blue: #9BDDFF;
  --color-pink: #D59BFF;
  --color-dark: #1D1C1B;
  --color-white: #FFFFFF;

  /* Surfaces & Backgrounds */
  --color-surface: #2A2926;
  --color-surface-alt: #333130;
  --color-page-bg: #1D1C1B;

  /* Borders */
  --color-border: #3A3836;
  --color-border-light: #E5E5E5;

  /* Text */
  --color-text-primary: #FFFFFF;
  --color-text-secondary: #8A8885;
  --color-text-muted: #6B6865;

  /* Typography */
  --font-display: "Alpha Lyrae", sans-serif;
  --font-body: "Geist", -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif;
  --font-mono: "Geist Mono", "JetBrains Mono", monospace;

  /* Spacing */
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 24px;
  --space-xl: 32px;
  --space-2xl: 48px;
  --space-3xl: 64px;
  --space-4xl: 80px;
  --space-5xl: 120px;

  /* Radius */
  --radius-sm: 8px;
  --radius-md: 12px;
  --radius-lg: 16px;
  --radius-xl: 24px;
  --radius-full: 9999px;

  /* Shadows */
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.3);
  --shadow-md: 0 4px 12px rgba(0,0,0,0.4);
  --shadow-lg: 0 8px 24px rgba(0,0,0,0.5);
  --glow-primary: 0 0 20px rgba(243,255,155,0.15);
  --glow-green: 0 0 20px rgba(155,255,163,0.15);
  --glow-blue: 0 0 20px rgba(155,221,255,0.15);
}
```

## Font Loading

Include these font imports in HTML artifacts:

```html
<!-- Geist: available from Vercel -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/geist@1.0.0/dist/fonts/geist-sans/style.css">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/geist@1.0.0/dist/fonts/geist-mono/style.css">
<!-- Alpha Lyrae: self-hosted or check CDN availability -->
```

If Alpha Lyrae is not available, fall back to:
`"Inter", "SF Pro Display", -apple-system, sans-serif`
with similar negative letter spacing for headings.

## Base Reset

```css
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

body {
  font-family: var(--font-body);
  font-size: 16px;
  line-height: 1.5;
  color: var(--color-text-primary);
  background-color: var(--color-page-bg);
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

h1, h2, h3, h4, h5, h6 {
  font-family: var(--font-display);
  font-weight: 500;
  line-height: 1.2;
  color: var(--color-text-primary);
}

h1 { font-size: 64px; letter-spacing: -0.04em; }
h2 { font-size: 56px; letter-spacing: -0.03em; }
h3 { font-size: 48px; letter-spacing: -0.03em; }
h4 { font-size: 40px; letter-spacing: -0.03em; }
h5 { font-size: 32px; letter-spacing: -0.02em; }
h6 { font-size: 24px; letter-spacing: -0.02em; }

code, pre {
  font-family: var(--font-mono);
}
```

## Light Mode Override

```css
[data-theme="light"] {
  --color-page-bg: #FFFFFF;
  --color-surface: #F5F5F5;
  --color-surface-alt: #EBEBEB;
  --color-border: #E5E5E5;
  --color-text-primary: #1D1C1B;
  --color-text-secondary: #6B7280;
  --color-text-muted: #9CA3AF;
}
```
