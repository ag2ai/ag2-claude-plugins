# Component Specifications

Detailed specs for AG2 UI components beyond the core button and card patterns.

## Form Inputs

- Background: `--color-surface`
- Border: 1px solid `--color-border`
- Border radius: `--radius-sm` (8px)
- Text: White, Geist 14px/400 Regular
- Padding: 10px 14px
- Focus state: border-color `--color-primary`, ring 2px `rgba(243,255,155,0.2)`
- Placeholder: `--color-text-muted`
- Label: Geist 12px/500 Medium, positioned above input with 4px gap

```css
.ag2-input {
  background: var(--color-surface);
  border: 1px solid var(--color-border);
  border-radius: var(--radius-sm);
  color: var(--color-text-primary);
  font-family: var(--font-body);
  font-size: 14px;
  padding: 10px 14px;
  transition: border-color 0.2s, box-shadow 0.2s;
}
.ag2-input:focus {
  border-color: var(--color-primary);
  box-shadow: 0 0 0 2px rgba(243,255,155,0.2);
  outline: none;
}
.ag2-input::placeholder {
  color: var(--color-text-muted);
}
.ag2-label {
  font-family: var(--font-body);
  font-size: 12px;
  font-weight: 500;
  color: var(--color-text-secondary);
  margin-bottom: 4px;
  display: block;
}
```

## Tags / Badges

- Background: brand color at 15% opacity
- Text: corresponding full-opacity brand color
- Padding: 4px 12px
- Border radius: `--radius-full` (pill)
- Font: Geist 12px/500 Medium

```css
.ag2-tag {
  display: inline-flex;
  align-items: center;
  padding: 4px 12px;
  border-radius: var(--radius-full);
  font-family: var(--font-body);
  font-size: 12px;
  font-weight: 500;
  line-height: 1.5;
}
.ag2-tag--primary {
  background: rgba(243,255,155,0.15);
  color: var(--color-primary);
}
.ag2-tag--green {
  background: rgba(155,255,163,0.15);
  color: var(--color-green);
}
.ag2-tag--blue {
  background: rgba(155,221,255,0.15);
  color: var(--color-blue);
}
.ag2-tag--pink {
  background: rgba(213,155,255,0.15);
  color: var(--color-pink);
}
```

## Code Blocks

- Background: #141312 (darker than page)
- Border: 1px solid `--color-border`
- Border radius: `--radius-md` (12px)
- Font: Geist Mono 14px/400 Regular
- Syntax highlighting with brand colors:
  - Keywords: Primary (#F3FF9B)
  - Strings: Blue (#9BDDFF)
  - Functions: Pink (#D59BFF)
  - Comments: Green (#9BFFA3)
  - Numbers: Soft Yellow (#FCFFE9)

```css
.ag2-code {
  background: #141312;
  border: 1px solid var(--color-border);
  border-radius: var(--radius-md);
  padding: var(--space-lg);
  font-family: var(--font-mono);
  font-size: 14px;
  line-height: 1.6;
  color: var(--color-text-primary);
  overflow-x: auto;
}
```

## Icons

- Style: Outlined, 1.5px stroke weight
- Default size: 20px (within 24px container)
- Color: inherit from text (usually White on dark)
- Preferred library: Lucide React or custom SVG
- AG2 has custom pixel-art agent/robot iconography — use geometric,
  minimal robot illustrations when relevant

## Layout Rules

- **Viewport**: 1600px reference width
- **Max content width**: 1300px, centered with 150px side margins (desktop)
- **Grid**: 64px increment grid within content area
- **Page margins**: 24px (mobile) / 48px (tablet) / 150px (desktop)
- **Mobile breakpoint**: 768px
- **Tablet breakpoint**: 1024px
- **Section pattern**: Full-width dark background → centered content container
- **Vertical rhythm**: Sections separated by 80–120px
- **Grid lines**: rgba(255,255,255,0.1) on dark backgrounds
