# AIOS Profile Design System

Source of truth for all visual decisions in this repository.
Do not modify assets without updating this document.

## 1. Identity

**Name** AIOS — Anzar Operating System  
**Concept** An AI-native OS identity: precise, dark, high-signal, not decorative.

## 2. Color Palette

| Token | Hex | Usage |
|---|---|---|
| Purple | `#A855F7` | Primary accent, borders, icons |
| Cyan | `#22D3EE` | Secondary accent, success states |
| Blue | `#3B82F6` | Tertiary accent, diagrams |
| Void | `#0A0E14` | Background primary |
| Surface | `#0D1219` | Background secondary |
| Panel | `#0F1520` | Card / tile fill |
| Muted | `#4B5563` | Captions, secondary text |
| Body | `#94A3B8` | Body text in SVG |
| Bright | `#E2E8F0` | Emphasized text |

## 3. Typography

**Monospace** `SF Mono`, `Fira Code`, `Courier New`, monospace  
**Sans** `-apple-system`, `BlinkMacSystemFont`, `Segoe UI`, sans-serif  
**Never** Google Fonts (blocked in GitHub SVG sandboxing)

## 4. SVG Rules

- All SVGs use a `viewBox`; no fixed pixel `width`/`height` on the root element.
- Decorative SVGs carry `aria-hidden="true"`. Informational SVGs carry `role="img"` and `aria-label`.
- Animations via `<animate>` only — no `<script>` (blocked by GitHub CSP).
- External assets (images, fonts, hrefs) are forbidden inside SVGs rendered on GitHub.
- Target < 50 KB per file uncompressed.

## 5. Layout Rhythm

- 60px horizontal margin in 900-wide banner / footer SVGs.
- 14px left pad inside 700-wide panel SVGs.
- Gradient rules fade to transparent at both ends (`stop-opacity="0"`).
- Corner node animations: two nodes per panel, pulse offset by 180°.

## 6. Diagram Themes

All Mermaid files use `%%{init: ...}%%` front matter with `"theme": "dark"` and the palette above.
Primary edge color is `#22D3EE` (cyan). Node fill uses the relevant accent per diagram role.

## 7. What Not To Do

- No emoji in SVG text (rendering inconsistent across platforms).
- No GitHub stats cards from third-party services (privacy, reliability).
- No hard-coded follower/star counts or "currently at X" metrics.
- No sections that require manual updating more than once per year.
- No nesting of `<svg>` inside `<svg>` (GitHub strips inner SVGs).
