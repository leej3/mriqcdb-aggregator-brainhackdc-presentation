# AGENTS.md

This file is for future coding agents working on the presentation in this repo.

## Purpose

This presentation is:

- informal
- short
- meant for a Brainhack-style group update
- intended to let project members discuss what they worked on
- intended to show other people at the hackathon what was achieved

The target length is **5 minutes**.

That should drive editing decisions:

- keep the deck compact
- keep slide density moderate
- prefer one clear point per slide
- optimize for speaking, not for reading silently

## Current Stack

- Slides: Quarto + reveal.js
- Styling: `styles.css`
- Output: `_site/index.html`
- Publish target: GitHub Pages
- Repo: `leej3/mriqcdb-aggregator-brainhackdc-presentation`

Key files:

- `index.qmd`: slide source
- `styles.css`: theme and reusable layout classes
- `_quarto.yml`: reveal.js config
- `.github/workflows/publish.yml`: GitHub Pages deploy
- `playwright.config.mjs` and `tests/navigation.spec.mjs`: browser smoke tests

## Editing Rules

### 1. Do not accidentally create nested slides

This repo previously had a reveal navigation bug caused by nested slide sections.

Important rule:

- Use `##` headings for top-level slides only.
- Do **not** use markdown `#`, `##`, or `###` headings inside a slide body unless you explicitly want a new reveal section.

Inside a slide body, prefer:

- plain paragraphs
- lists
- `<div class="panel-title">...</div>`
- `<strong>...</strong>`

If you need more structure inside a slide, use HTML elements or existing CSS utility classes rather than markdown headings.

### 2. For complex mockups, use raw HTML fences

If a slide contains nested layout markup, wrap it in:

```markdown
```{=html}
...html...
```
```

That avoids Pandoc turning parts of the content into escaped code or unintended slide structure.

### 3. Keep the deck short

For a 5-minute talk, the ideal deck is roughly:

- 1 title/context slide
- 4 to 6 content slides
- 1 closing/future directions slide

If you add material, strongly consider merging or removing another slide.

### 4. Prefer repo-grounded claims

When editing content, prefer claims already supported by the project repo:

- `README.md`
- `docs/data-migration.md`
- `docs/schema-design.md`
- `docs/ingestion.md`
- `docs/backend.md`
- `docs/erd.md`
- `docs/temp/db-profiles/...`

Avoid inventing metrics or performance claims that are not grounded in the repo.

## Tone Guidance

This is not a formal investor pitch and not a polished conference talk.

Prefer:

- concrete
- modest
- technically honest
- easy to speak through

Avoid:

- sales-heavy language
- speculative performance claims stated as facts
- long, dense bullets
- too much architecture detail for every slide

A good tone is:

- “here’s what we built”
- “here’s why it mattered”
- “here’s what we learned”
- “here’s what we would do next”

## Visual Guidance

The current deck aims for:

- a clean card-on-gradient reveal look
- readable type
- light dashboard mockups
- minimal but intentional color accents

When making aesthetic changes:

- start with CSS variables near the top of `styles.css`
- prefer reusable classes over one-off inline styles
- check that text is still readable on projectors
- avoid making slides too top-heavy or too sparse

Useful classes already in the stylesheet:

- `hero-grid`
- `grid-2`
- `feature-grid`
- `stack-grid`
- `hero-panel`
- `card`
- `feature-card`
- `panel-title`
- `callout`
- `stat-grid`
- `metric-list`
- `mini-browser`
- `mock-dashboard`
- `demo-steps`
- `closing`

## Safe Workflow

### Render locally

```bash
quarto render
```

or

```bash
pixi run build
```

### Browser navigation smoke test

Run this after changing slide structure or CSS:

```bash
npm run test:nav
```

This checks keyboard and control navigation in:

- Chromium
- WebKit

### Local preview

If needed:

```bash
quarto preview
```

or serve `_site/` directly.

## Deployment Notes

- Pushes to `main` trigger the GitHub Pages workflow.
- The production URL is:
  `https://leej3.github.io/mriqcdb-aggregator-brainhackdc-presentation/`

If the deployed site looks stale, check:

1. the latest GitHub Actions run
2. whether `quarto render` succeeded locally
3. whether the built output still passes `npm run test:nav`

## Content Priorities For Future Edits

If there is pressure to cut content for time, keep:

1. what the project is
2. why the API/storage shape was a problem
3. what the migration/backend work accomplished
4. what the dashboard enables
5. what comes next

Cut or compress first:

- duplicate implementation detail across slides
- repeated explanations of the same architecture
- too many numbers on one slide
- speculative future features

## If You Need To Restructure Slides

Good pattern:

- slide title as `##`
- 2-column layout with `grid-2` or `hero-grid`
- one side for key message
- one side for evidence, metric, mockup, or takeaway

Bad pattern:

- many nested markdown headings
- three or four unrelated ideas on one slide
- dense paragraphs that the presenter cannot say naturally in under 30 seconds

## Final Check Before Finishing

Before you hand off changes:

1. run `quarto render`
2. run `npm run test:nav`
3. sanity-check at least the title slide and one mid-deck slide visually
4. make sure no raw HTML is showing up as escaped code in the deck
5. keep the deck suitable for a 5-minute informal update
