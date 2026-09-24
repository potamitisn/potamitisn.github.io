# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Personal academic website of Nearchos Potamitis (PhD student, Aarhus University), served at
https://potamitisn.github.io. It is a single hand-written page: no framework, no build step, no
package manager. It replaced an al-folio (Jekyll) site in September 2026, after two rejected redesigns (a minimal
sans-serif page and a dark "reasoning trace" theme); the current direction is warm serif typography
with a compact academic layout: one paragraph, papers with figures, news and awards.

## Commands

There is nothing to install. Preview with any static file server from the repo root:

```bash
python3 -m http.server 8000     # then open http://localhost:8000
```

No tests, no linter. If you want consistent formatting, `npx prettier --write index.html style.css`
with default settings is what the files were written to match.

## Layout

- `index.html` is the whole site. Structure, top to bottom: header (name, role line, one
  paragraph, text links, round portrait on the right on desktop), "Selected papers" (each
  paper is an `<article class="paper">` with a figure thumbnail, title, authors, venue, a
  links row and a one-sentence summary), then a `.wide` two-column block with News and Awards
  that is deliberately wider than the main column, then a one-line footer. Scripts open
  external links in a new tab and drive the theme toggle.
- `style.css` holds tokens on `:root`. Light is the default; the dark set applies via
  `prefers-color-scheme: dark` (guarded by `:root:not([data-theme="light"])`) and via an
  explicit `:root[data-theme="dark"]`. Keep both dark blocks in sync. A `.theme` button in
  the top-right toggles and persists the choice in `localStorage`; a head script applies it
  before first paint.
  `--measure` (40rem) is the main column, `--wide` (54rem) the news/awards block, which is
  centred with `margin-left: 50%; transform: translateX(-50%)`. Warm off-white background and
  one terracotta accent are the identity; keep the palette to those plus ink greys.
- `assets/figures/*.svg` are placeholder thumbnails drawn in the site palette, one per entry
  (six: fleet, cachesaver, tutorial, atomix, reasonbench, retrials).
  Replace each with a real figure (a 4:3 PNG or JPG, roughly 800x600) when available and
  update the `src`.
- `404.html` reuses `style.css` and uses root-relative paths because it is served from any URL.
- `assets/` holds the resized portrait, favicons, figures and the CV PDF.
- `content/` is the data behind the page, kept template-free: `profile.yml`, `papers.bib`,
  `resume.json`, `cv.yml`, `news.yml`, `awards.yml`, and the original full-resolution images
  and PDF under `content/assets/`. It is not deployed. When adding a paper, news item or
  award, update both the matching file in `content/` and `index.html`.

## Deployment

`.github/workflows/deploy.yml` runs on every push to `main`. It copies `index.html`, `404.html`,
`style.css` and `assets/` into a temp folder and force-pushes that folder to the `gh-pages`
branch, which is what GitHub Pages serves (Pages source is set to `gh-pages` / root in the repo
settings). Never edit `gh-pages` by hand. `content/`, `.github/` and this file are not deployed.

## Conventions

- Two Google Fonts, the only external dependency: Fraunces for the name, headings and paper
  titles with its SOFT axis at 100 and WONK at 1 (rounded, relaxed serifs; the owner asked
  for "less sharp, more roundy"), and Lora for everything else. Optical size is set per use
  via `font-variation-settings`.
- Contact links are plain text prefixed with a terracotta arrow via `::before`. Paper entries
  have no link row (removed at the owner's request); the title and thumbnail are the links,
  and both currently point to the Google Scholar profile until per-paper URLs are provided.
  Do not add icon buttons, cards, shadows or badges; the layout borrows ideas from a friend's
  site and must not look like a copy of it.
- News and Awards must not repeat each other: an award goes in Awards only.
- Coauthor names in author lists link to their Google Scholar profiles; the IDs are recorded
  in `content/coauthors.yml` (Chongyang Xu links to a personal site instead). Use the same list
  when adding a paper. Author lists are kept on one line each so name patterns match.
- The bio is one paragraph and names both supervisors, Akhil Arora and Lars Klein. Keep it
  that way; experience and education live in the CV PDF.
- News dates marked approx in `content/news.yml` were inferred from the CV and should be
  verified. Three ReasonBench co-authors (V. Ramani, H. A. Arora, D. Kuchhal) appear with
  initials only because the CV gives no full names; they are linked to Scholar regardless.
- Paper order follows the CV: full papers first, then tutorial and workshop papers.
- Keep images small: regenerate `assets/prof_pic.jpg` from `content/assets/prof_pic.jpg` with
  `sips -Z 960` rather than committing a full-size photo.
