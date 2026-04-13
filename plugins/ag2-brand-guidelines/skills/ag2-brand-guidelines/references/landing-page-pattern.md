# Landing Page Pattern

Follow this section structure for AG2 landing pages and marketing pages.

## Section Order

### 1. Hero

- Dark background (#1D1C1B)
- Large Alpha Lyrae headline (H1, 64px, Regular or Bold)
- Highlight a keyword in Primary (#F3FF9B) for emphasis
- Geist subtitle (Text 20–24, Regular)
- Primary pill CTA button
- Optional: subtle grid or dot pattern overlay

Example:

```html
<section style="background: var(--color-dark); padding: var(--space-5xl) 0;">
  <div style="max-width: 1200px; margin: 0 auto; text-align: center;">
    <h1 style="font-family: var(--font-display); font-size: 64px; letter-spacing: -0.04em;">
      Multi-agent intelligence,<br>
      <span style="color: var(--color-primary);">shipped at scale.</span>
    </h1>
    <p style="font-family: var(--font-body); font-size: 20px; color: var(--color-text-secondary); margin-top: var(--space-lg);">
      Build, orchestrate, and evolve systems of AI agents as your AI workforce with AG2.
    </p>
    <a href="#" style="display: inline-block; margin-top: var(--space-xl); background: var(--color-primary); color: var(--color-dark); font-family: var(--font-body); font-weight: 500; font-size: 14px; padding: 12px 28px; border-radius: var(--radius-full); text-decoration: none;">
      Get started
    </a>
  </div>
</section>
```

### 2. Social Proof / Logo Strip

- Muted logo strip on dark background
- Partner/customer logos in grayscale or low opacity
- Subtle top/bottom border for separation
- Logos: Google, NVIDIA, Walmart, AT&T, HSBC, Johnson & Johnson, Flipkart, Cegid, MediaTek

### 3. Features Grid

- 2–3 column grid on dark background
- Each card: icon + heading (H5, Medium) + body (Text 16, Regular)
- Card style: `--color-surface` background, `--color-border` border, `--radius-xl`
- Generous padding (32px)
- Icons in brand accent colors (Blue, Green, Pink)

### 4. Product Showcase

- Screenshot or illustration of the product
- Blue/Pink/Green accent decorations
- Optional: animated or interactive demo preview
- Caption text in Geist Regular

### 5. Testimonials

- Dark cards with quote text (Text 18, Regular, italic)
- Author name in Geist Medium, role in Geist Regular muted
- Optional: avatar with `--radius-full`
- Quote mark decoration in Primary at low opacity

### 6. CTA Section

- Centered heading (H2, Bold)
- Supporting text (Text 20, Regular, muted)
- Primary pill CTA button
- Optional secondary button (Ghost style)
- Extra vertical padding (120px)

### 7. Footer

- Dark background (#1D1C1B)
- AG2 pixel logo (left or centered)
- Organized link columns (Text 14, Regular)
- Link hover: opacity 0.8 → 1.0
- Copyright and legal text (Text 11, muted)
- Optional social links

## Navigation Bar

- Fixed/sticky at top
- Background: `--color-dark` with transparency + backdrop-blur
- AG2 pixel mascot + "AG2" wordmark (top-left)
- Primary nav links: Product, Developers, Research, Ecosystem, Blog
- Font: Geist 14px/400 Regular, white, opacity 0.8 → 1.0 hover
- Right side: "Contact Sales" text link + "Request Access" Primary pill CTA
- Mobile: hamburger menu

## Agent Mascots

AG2 features 36+ pixel-art agent characters, each representing a specific
role: advocate, brain, chief, clarity, classifier, closer, conductor,
courier, fixer, guardian, helper, herald, hunter, librarian, mapper, muse,
parser, prioritizer, reporter, responder, saas, scout, seeker, sentinel,
strategist, synthesizer, thinker, writer, tempo, and more.

Each mascot has a unique color palette and visual accessory (hat, tool, pose).
Use them on landing pages as hero illustrations, feature section accents,
or as avatars representing different agent capabilities.
