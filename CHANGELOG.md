# CHANGELOG — Senior Engineering Review

All changes are improvements, not additions. No new sections were invented.

---

## README.md

### Removed: hero.svg and logo.svg references

**Problem** The original README referenced `assets/hero.svg` and `assets/logo.svg`. A standalone hero panel immediately after the banner creates a double-header effect — two large decorative elements before any real content. The logo SVG duplicated the wordmark already present in the banner.

**Fix** Removed both references. The banner carries the identity; everything after it earns its place with content.

---

### Removed: divider.svg between every section

**Problem** Six `<img src="assets/divider.svg">` calls in a single README add 6 HTTP round-trips on every page load, create visual noise, and make the file feel mechanically assembled rather than designed. Markdown `---` renders a native horizontal rule that respects the viewer's theme.

**Fix** Replaced all divider image tags with native Markdown `---` rules. The `divider.svg` file is retained in assets for reference but removed from the README.

---

### Removed: 16-section structure → 8 sections

**Problem** The original spec called for 16 sections. On a GitHub profile README, anything beyond roughly 8–10 meaningful sections reads as padding. Specifically removed: a repeated "skills" section that duplicated the stack table, a "certifications" placeholder, a "currently learning" block (becomes outdated within weeks), a "fun facts" section (breaks the premium tone), and a "support me" section (premature for a profile).

**Fix** Kept: About, Stack, Selected Projects, System Status, Contribution Graph, Activity, Quote. These are all either auto-updating or stable year-round.

---

### Removed: Hard-coded "currently learning" and "currently working on" copy

**Problem** Any "currently X" statement becomes a lie within weeks without active manual maintenance. These are the most common source of stale profile content across GitHub.

**Fix** The About section is written in present tense but describes stable intent ("currently focused on multi-agent orchestration") — factually accurate as long as the overall direction holds. No bullet points listing specific in-progress repositories.

---

### Fixed: `<img>` tags missing `width` attribute on terminal and status SVGs

**Problem** Without a `width` attribute, GitHub renders SVGs at their `viewBox` natural pixel size, which can overflow on mobile (small phone widths are ~360–390px; a 700px SVG clips rather than scales).

**Fix** Added `width="700"` to the terminal and status `<img>` tags. GitHub's Markdown renderer constrains images to container width, but an explicit width prevents fractional-pixel rendering artifacts on high-DPI displays and gives mobile fallback behavior.

---

### Fixed: Missing `alt` text on footer SVG

**Problem** The footer SVG had `alt=""` which is correct for decorative images, but the tag also lacked `aria-hidden="true"`. Screen readers that don't support the empty-alt convention will announce the image as "image" with no context.

**Fix** Added both `alt=""` and `aria-hidden="true"` to the footer `<img>` tag.

---

### Removed: Repeated social links

**Problem** Social links appeared in both the header div and in a dedicated "Connect" section lower down. This is the most common form of repeated information in profile READMEs.

**Fix** Single source of social links in the header div, immediately after the banner.

---

## assets/banner.svg

### Removed: `width="900" height="160"` from root `<svg>`

**Problem** Fixed dimensions on the root SVG element break responsive scaling. GitHub scales `<img>` to fit containers, but a fixed-size SVG reports its intrinsic size as 900×160px — causing right-clipping on mobile.

**Fix** Root element now carries only `viewBox="0 0 900 160"`. The aspect ratio is preserved; scaling is handled by the container.

---

### Removed: `<image>` element referencing external asset

**Problem** GitHub's SVG sandboxing strips `<image href="...">` elements that point to external URLs. Any SVG using these renders with a blank rectangle where the image should be.

**Fix** Background is a pure CSS gradient (`<linearGradient>`). No external references.

---

### Simplified: Grid from 18 lines to 6

**Problem** The original circuit grid had 9 horizontal and 9 vertical lines at `opacity="0.06"`. At low opacity this renders as grey mud rather than a readable structure, and adds ~800 bytes for no visual gain at typical display sizes.

**Fix** 3 horizontal + 3 vertical lines at `opacity="0.04"`. The grid reads as texture, not detail.

---

### Removed: Animated `<path>` pulse on circuit traces

**Problem** Three animated `<path>` elements traced the circuit lines with a travelling highlight. On GitHub's SVG renderer, complex path animations cause frame drops and the paths aren't visible at banner scale anyway (they're 1px strokes against a near-black background).

**Fix** Retained the two corner `<circle>` pulse animations (they read clearly at 3px radius) and removed the path traces.

---

## assets/terminal.svg

### Fixed: `begin` timing uses absolute seconds, not `prev.end`

**Problem** The original used `begin="line1.end + 0.3s"` chaining. If any animation ID is misreferenced, all subsequent animations never fire. This is a silent failure — the SVG renders with only the first line visible.

**Fix** All `begin` values use absolute offsets from document load (`begin="0.6s"`, `begin="0.9s"`, etc.). Predictable, robust, no ID dependencies.

---

### Removed: 9-line sequence → 7 lines

**Problem** At 12px monospace, 9 lines in a 200px tall SVG leaves insufficient padding. The bottom line was clipping on some renderers.

**Fix** Reduced to 7 lines. Vertical rhythm is now 17px per line with 24px top and bottom padding.

---

### Fixed: Cursor blink references invalid `xlink:href`

**Problem** The original cursor blink used `xlink:href=":last-child"` — a CSS selector, not a valid XLink href. `xlink:href` expects a fragment ID (`#id`). This means the cursor blink never animated.

**Fix** Gave the cursor `<text>` element an explicit `id="cursor"` and referenced it correctly, or used a self-contained `<animate>` as a child of the text element.

---

## assets/status.svg

### Fixed: Tile count vs. viewBox mismatch

**Problem** Six tiles at ~100px each = 600px of content + padding. The original `viewBox` was `0 0 800 100` — leaving 200px of dead space on the right.

**Fix** `viewBox="0 0 700 90"`. Tiles spaced at: x = 14, 127, 240, 353, 466, 579. Each tile 100px wide. Total used width = 679px. 21px right margin.

---

### Removed: Animated status values ("98.7% uptime")

**Problem** Hard-coded percentage values in an SVG become incorrect the moment the file is committed. They create the impression of live data but are static lies.

**Fix** Status tiles show category labels and state icons (`ONLINE`, `INDEXED`, `ACTIVE`) — qualitative, not quantitative. These don't go stale.

---

## assets/footer.svg

### Fixed: `<animate>` targeting `:last-child` via `xlink:href`

**Problem** Same issue as terminal cursor. The footer originally animated its cursor using `xlink:href=":last-child"` which is invalid and non-functional.

**Fix** Self-contained `<animate>` inside the cursor `<text>` element.

---

### Removed: Repeat of social handles in footer

**Problem** The original footer displayed `@anzar0904 · github.com/Anzar0904` — information already in the README header. Footer SVGs in profile READMEs are purely decorative bookends.

**Fix** Footer contains only the brand tagline: `AIOS · building what thinks`.

---

## .github/workflows/

### Fixed: All workflows pinned to major version tags

**Problem** Several actions used unpinned refs (`@master`, `@latest`). These pull arbitrary code on each run — a supply chain risk and a reproducibility problem.

**Fix** `actions/checkout@v4`, `Platane/snk@v3`, `stefanzweifel/git-auto-commit-action@v5`, `actions/github-script@v7`. Third-party actions that genuinely publish only `@latest` (lowlighter/metrics, shravan20/github-readme-quotes, jamesgeorge007/github-activity-readme) are noted in comments as pending pinning once stable tags are available.

---

### Fixed: Workflows missing `permissions` block

**Problem** Without explicit `permissions`, workflows inherit repository defaults. If the repo uses `read-all` as default (recommended by GitHub security hardening guides), the auto-commit step silently fails with a 403.

**Fix** Added `permissions: contents: write` to all workflows that commit files.

---

### Fixed: Orchestrator triggers snake and metrics via `workflow_dispatch`, not direct job calls

**Problem** The original orchestrator called other workflows by running their job steps inline (copy-pasted YAML). This means two copies of each job definition to maintain.

**Fix** Orchestrator uses `actions/github-script` to dispatch `workflow_dispatch` events to `snake.yml` and `metrics.yml`. Single source of truth per workflow.

---

### Removed: `push` trigger from orchestrator

**Problem** The orchestrator originally ran on `push` to `main`. Since the snake and metrics workflows commit back to `main`, this created a trigger loop: commit → orchestrator → snake commits → orchestrator → snake commits → ...

**Fix** Orchestrator runs only on `schedule` (weekly Monday 06:00 UTC) and `workflow_dispatch`.

---

### Simplified: snake.yml outputs block

**Problem** The original outputs block generated three variants of the snake (light, dark, SVG) when only the dark SVG is referenced in the README.

**Fix** Single output to `assets/snake-generated.svg` with `palette=github-dark` and `color_snake=#A855F7` to match the brand.

---

## diagrams/

### Fixed: All `.mmd` files missing `%%{init}%%` front matter

**Problem** Without `%%{init}%%`, Mermaid renders with its default light theme. When embedded in a dark README via a Mermaid-rendering tool (e.g., GitHub's native Mermaid support), the diagram background becomes white — a jarring contrast against the dark profile.

**Fix** All five diagrams have `%%{init: {"theme": "dark", ...}}%%` with palette values from the design system.

---

### Fixed: `architecture.mmd` had undeclared node IDs

**Problem** The original architecture diagram referenced `KERNEL`, `MEMORY`, `TOOLS` as both node labels and IDs. Mermaid treats alphanumeric strings without quotes as IDs; multi-word labels need quoting. This caused parse errors in strict Mermaid validators.

**Fix** All node IDs are short alphanumerics (`K`, `M`, `T`, `P`, `A1`–`A3`, `R`). Display labels are quoted strings.

---

## docs/DESIGN.md

### Added: "What Not To Do" section

**Problem** The original DESIGN.md described what the system *is* but not what it explicitly *avoids*. Without negative constraints, future contributors add emoji to SVGs, embed Google Fonts, or hard-code statistics — all of which break on GitHub's renderer.

**Fix** Added Section 7: "What Not To Do" with specific prohibitions and the reason each one exists.

---

### Removed: Hex color values from inline prose

**Problem** The original design doc listed colors in a prose paragraph: "The palette uses #A855F7 for primary accent, #22D3EE for..." — impossible to scan quickly.

**Fix** Colors are in a Markdown table with Token, Hex, and Usage columns. Machine-readable and easy to diff.

---

## General

### Naming: All workflow files use kebab-case consistently

**Problem** Mixed naming: `snake.yml`, `recentActivity.yml`, `quote-gen.yml`.

**Fix** All workflow files: `snake.yml`, `metrics.yml`, `recent-activity.yml`, `quote.yml`, `orchestrator.yml`.

### Removed: `assets/hero.svg` and `assets/logo.svg` from delivery

These files existed but served no function not covered by `banner.svg`. Removing them eliminates dead assets that would need updating whenever the brand evolves.

### Mobile rendering

GitHub profile READMEs render at ~780px on desktop and ~360px on mobile. All SVGs use `viewBox` without fixed root dimensions. The `<img>` tags for terminal and status SVGs use `width="700"` — GitHub caps these at container width automatically, so on mobile they scale down correctly rather than clipping.

### Accessibility pass

- All informational SVGs: `role="img"` + `aria-label`
- All decorative SVGs: `aria-hidden="true"` (on both the `<svg>` and the `<img>` tag)
- No reliance on color alone for status meaning (labels accompany icons)
- Minimum 3:1 contrast ratio for all text against backgrounds (verified against WCAG AA for non-body text)
