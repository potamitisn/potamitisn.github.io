# potamitisn.github.io

Personal website of Nearchos Potamitis. Plain HTML and CSS, no build step.

- `index.html`, `style.css`, `assets/` are the site.
- `content/` holds the underlying data (bio, publications BibTeX, CV JSON, original images and PDF).
- Pushing to `main` deploys to GitHub Pages via `.github/workflows/deploy.yml`.

Preview locally with any static server, for example:

```bash
python3 -m http.server 8000
```
