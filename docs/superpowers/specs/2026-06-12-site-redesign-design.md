# Site Redesign Design

**Date:** 2026-06-12
**Status:** Approved

## Overview

Restructure Paulo-B.github.io into a professional portfolio site with three clearly separated concerns: professional background (homepage), personal blog, and game reviews. The `hugo-developer-portfolio` theme submodule is not modified; all customisation goes through Hugo's standard layout override system and content/data files.

---

## Navigation

Remove Portfolio from the nav menu. Final order:

| Label | URL |
|---|---|
| About | /about |
| Blog | /blog |
| Game Reviews | /game-reviews |

Home (`/`) is the root and is not a nav item.

---

## Homepage — Professional Background

**No layout changes required** — the theme's `layouts/index.html` already renders sections from `data/homepage.yml`.

Changes to `data/homepage.yml`:

- Fix typo: "Experinece" → "Experience"
- Add a `portfolio` section (the theme's `layouts/index.html` already renders `homepage.portfolio.item` natively — no layout changes needed). Add a placeholder item so the section appears; Paulo can fill it in with real projects.

Note: `data/portfolio.yml` currently only contains a filter label, not actual items. It is left as-is. The separate `/portfolio/` page is simply removed from the nav.

Changes to `hugo.toml`:

- Remove the `Portfolio` menu entry
- Add `Game Reviews` menu entry pointing to `/game-reviews`

Changes to `hugo.toml` params:

- Replace placeholder footer email with a real or intentionally blank value

---

## Blog Section

No structural changes. The existing `content/blog/` section and `_default/list.html` layout remain unchanged.

Cleanup only:

- Update `content/blog/_index.md` description from placeholder to something real

---

## Game Reviews Section

### Content structure

```
content/game-reviews/
  _index.md          # section index (title, description)
  <game-slug>.md     # one file per review
```

Each review front matter:

```yaml
---
title: "Game Title"
date: 2024-01-01
description: "One-line tagline shown on the card and single page"
image: "/images/game-reviews/game-cover.jpg"
rating: 8
---

Full written review in Markdown.
```

Cover images go in `static/images/game-reviews/`.

### Layouts

Two new layout files in the site root (not in the theme submodule):

#### `layouts/game-reviews/list.html`

Card grid (consistent with the rest of the site's UIkit style). Each card shows:
- Cover image (full-width top of card)
- Title
- Date
- Rating badge (e.g. **8/10**)
- Description/tagline
- "Read review" link

#### `layouts/game-reviews/single.html`

Full review page. Top-to-bottom:
1. Disclaimer banner — *"These are personal opinions and not professional reviews."*
2. Cover image (centred)
3. Title + date
4. Rating display — large **8 / 10**
5. Full Markdown content (`.Content`)

Both layouts extend `_default/baseof.html` via `{{ define "main" }}`.

---

## CLAUDE.md

Project-level documentation at the repo root covering:

- Site structure — purpose of `content/`, `data/`, `layouts/`, `static/`
- How to add a blog post — front matter fields, file naming convention
- How to add a game review — front matter fields, cover image location
- How to update professional info — which keys in `data/homepage.yml` to edit (skills, experience, certifications, education, projects)
- How to build locally — `hugo server -D`
- Deployment — push to `main`, GitHub Actions builds and deploys to GitHub Pages automatically

---

## Files Changed / Created

| Action | Path |
|---|---|
| Modify | `hugo.toml` |
| Modify | `data/homepage.yml` |
| Modify | `content/blog/_index.md` |
| Create | `content/game-reviews/_index.md` |
| Create | `content/game-reviews/example-review.md` (sample) |
| Create | `layouts/game-reviews/list.html` |
| Create | `layouts/game-reviews/single.html` |
| Create | `CLAUDE.md` |

---

## Out of Scope

- Theme CSS/JS changes (submodule not modified)
- Authentication, comments, or any backend
- Migrating existing portfolio items beyond duplicating them into `homepage.yml`
