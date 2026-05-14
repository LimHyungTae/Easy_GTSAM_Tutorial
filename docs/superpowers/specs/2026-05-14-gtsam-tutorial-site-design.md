# GTSAM Tutorial Site — Design Spec

**Date:** 2026-05-14
**Author:** Hyungtae Lim (via Claude Code brainstorm)
**Status:** Approved by user, proceeding to implementation in one shot.

## Goal

Build a self-contained, bilingual (EN/KO), SEO-friendly, single-page tutorial site that explains how GTSAM works internally. The site mirrors the look and feel of [`paper-writing-checklist`](https://github.com/LimHyungTae/paper-writing-checklist) but adds a sticky left navigation sidebar.

Live target: `docs/index.html` served via GitHub Pages from `/Users/fudxo/git/gtsam/`.

## Source content

All content is sourced from the author's own blog posts at `~/git/LimHyungTae.github.io/_posts/`:

- Tutorial 1: BetweenFactor (`gtsam_solving.png`)
- Tutorial 2: SE(2) transformation, Jacobian, block ops
- Tutorial 3: Skew-symmetric matrix in 2D (`circular_motion.png`)
- Tutorial 4: Unary factor Jacobian (`gtsam_sping_mass_system.png`, `0206_evaluateError.png`)
- Tutorial 5: `Rot2::unrotate` Jacobian
- Tutorial 6: `Pose2::BetweenFactor` Jacobian
- Tutorial 7: Adjoint map
- Tutorial 8: `Pose3::BetweenFactor` Jacobian
- Tutorial 9: Kimera-PGMO Deformation Factor
- Tutorial 10: Debugging factors with `numericalDerivative`

Chapter 0 is paraphrased from <https://engcang.github.io/gtsam_tutorial.html> (high-level usage), with explicit attribution.

## Architecture

### Repository layout

```
gtsam/
├── docs/
│   ├── index.html              # The site (single file)
│   ├── figures/                # All images, self-contained
│   │   ├── gtsam_solving.png
│   │   ├── gtsam_loopclosing.png
│   │   ├── gtsam_sping_mass_system.png
│   │   ├── circular_motion.png
│   │   └── 0206_evaluateError.png
│   └── superpowers/specs/2026-05-14-gtsam-tutorial-site-design.md  (this file)
├── .nojekyll                   # Tell GitHub Pages to skip Jekyll
└── README.md
```

### Page layout

- Top-level CSS grid: `[sidebar 240px][content 1fr]`. Below 1024px the sidebar collapses to a top hamburger.
- Sidebar is `position: sticky; top: 0; height: 100vh`, with `overflow-y: auto` so its own scrollbar is independent.
- Content column: hero, then 12 `<section>` blocks (intro, ch0–ch10, appendix), then footer.
- Hero contains: title, EN/KO toggle, keyword chips, lead, "How's Hyungtae Lim?" expandable author card.
- Footer contains: persistent **About the Author** card with primary link to `https://limhyungtae.github.io` + GitHub + Scholar + Email.

### Components reused from `paper-writing-checklist`

- Design tokens (`--accent`, `--mono`, etc.)
- `.section`, `.section-tag`, `.callout`, `.note`, `.warn`, `.compare` (Don't/Do), `figure.gh-fig`
- Prism.js syntax highlighting (cpp/python/markdown)
- EN/KO toggle via `body[data-lang]` + `[data-lang="en|ko"]` spans
- `.copy-heading` (click to copy section URL)
- `.author-card` expandable

### New components

- `.layout` grid wrapper
- `.sidebar`, `.sidebar-section`, `.sidebar-link`, `.sidebar-link.active`
- `.sidebar-toggle` (mobile hamburger)
- `.about-footer` persistent author card

### Math rendering

KaTeX 0.16.x via CDN with auto-render. Macros: `\bm → \boldsymbol`, `\R → \mathbb{R}`. Delimiters: `$`, `$$`, `\(\)`, `\[\]`.

### EN/KO toggle

Same mechanism as paper-writing-checklist. Both languages live in DOM (good for SEO and to keep Ctrl+F simple). CSS hides the inactive language. Toggle persists in `localStorage`.

**One refinement:** Korean-blog backlinks (`limhyungtae.github.io` original post URLs) live **only inside `<span data-lang="ko">`** because they go to Korean-only content.

### Sidebar interaction

- `IntersectionObserver` on each `<section>` to highlight the active link
- Smooth scroll on click, plus `scroll-margin-top` so headings don't get hidden under sticky header
- Mobile: hamburger toggles a slide-in panel

### SEO

- `<meta>` description + keywords (GTSAM, factor graph, SLAM, Lie group, BetweenFactor, Jacobian, pose graph optimization, …)
- Open Graph + Twitter Card with `og:locale=en_US` and `og:locale:alternate=ko_KR`
- JSON-LD `Article` schema, `inLanguage: ["en", "ko"]`
- `<link rel="canonical">`
- robots `index, follow, max-snippet:-1`
- No in-page search box (12 sections, sidebar + Ctrl+F is enough)

## Phasing

User requested whole-shot delivery with subagent parallelism. Execution order:

1. **Skeleton (single-agent):** index.html with CSS, JS, hero, sidebar, footer-author-card, KaTeX wiring, EN/KO toggle, IntersectionObserver. Empty section placeholders.
2. **Chapter 0** (engcang quick-start, paraphrased + attribution + KO/EN) — single agent so the first chapter sets the format precedent.
3. **Chapters 1–10 in parallel** via 4 subagents:
   - Agent A: Ch 1, Ch 2, Ch 3
   - Agent B: Ch 4, Ch 5
   - Agent C: Ch 6, Ch 7, Ch 8
   - Agent D: Ch 9, Ch 10
   Each agent gets the original KO post path + an HTML template with strict formatting rules (`data-lang` spans, KaTeX delimiters, `<pre><code class="language-cpp">`, blog backlink in `data-lang="ko"` only).
4. **Integration:** main agent assembles the per-chapter HTML fragments into `index.html`, fixes cross-references, adds appendix.
5. **Verification:** open in browser via `python3 -m http.server`, hand-check sidebar, language toggle, math rendering, figures.
6. **Commit.**

## Out of scope (deliberate YAGNI)

- GncOptimizer and XYZ/ZYX Convention posts (user said not in scope)
- In-page search (sidebar + Ctrl+F is enough)
- Dark mode (paper-writing-checklist doesn't have one either)
- Comments / Disqus
- i18n routing — both languages live in one page
