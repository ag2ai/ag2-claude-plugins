# AgentOS — Chat View (Figma `235:879`)

Desktop application with chat interface.

```
[Title Bar — 1440px x 48px]
  Left: macOS traffic lights (red/yellow/green circles, 14px)
  Center: AG2 logo icon (18x16) + "AGENT" (Geist 9px) + "OS" (Geist 9px, bold)

[Sidebar — 200px width, dark bg]
  Nav items (top): Pilot, Twins, Conference, Network, Universe, Marketplace
    Icon (16x16) + label (Geist 14px/500), white
    Active: blue highlight bg + blue text ("Pilot" default active)
  Footer items (bottom): Settings, user name ("Enes Aktas")

[Content Area — 1240px, dark bg]
  [Time Divider: centered "Today 9:24 AM", horizontal rules L/R]

  [Chat Thread — 650px wide, centered in content]
    User messages (right-aligned):
      "Hey what's up?" — dark bg, border-subtle, pill shape
      "Do you have access to claude code?" — same treatment
      "Chat with it again!" — same treatment
      Timestamp below each: "9:24 AM", Geist 12px, 64% opacity
    AI messages (left-aligned):
      "Hey Rick! Not much just here and ready to roll…"
      Multi-paragraph with line breaks
      Timestamp below

    Claude Quote Block:
      surface-subtle bg, 16px padding
      Row: Claude icon (16x16) + "Claude says:" (Geist 14px/500)
      Quote: Geist 14px/400 white

    Embedded Agent Card:
      border-subtle border, 0px radius, 16px padding
      Row: code icon (16x16) + "Claude Code" (Geist 14px/500) + pixel arrow
      Below: "Local agent" (Geist 12px/400, 80%)

  [Chat Input — bottom, 650px wide, 64px height]
    Left: "|Talk to Pilot" (Geist 16px) + "(@ to reference)" (Geist 16px, 20%)
    Right: attachment + mic + code (32px circle buttons) + Send (blue glass-shine, 48x32)

  [Alt bar — bottom edge, 1240px x 16px, decorative]
```
