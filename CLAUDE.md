# Paulo Bento's Site — CLAUDE.md

This is a Hugo static site deployed to GitHub Pages via GitHub Actions on push to `main`.
Theme: `hugo-developer-portfolio` (git submodule at `themes/hugo-developer-portfolio/`).
**Never modify files inside `themes/`** — use Hugo's layout override system instead.

## Directory Structure

| Path | Purpose |
|---|---|
| `hugo.toml` | Site config: baseURL, nav menu, plugins, markup settings |
| `data/homepage.yml` | All homepage content: banner, social links, skills, experience, certifications, education, portfolio |
| `content/about/_index.md` | About page copy |
| `content/blog/` | Blog posts (one `.md` file per post) |
| `content/game-reviews/` | Game review posts (one `.md` file per review) |
| `layouts/game-reviews/` | Custom layouts for the game-reviews section (overrides theme) |
| `static/images/` | Static images served at `/images/` |
| `static/images/game-reviews/` | Cover images for game reviews |
| `public/` | Hugo build output — do not edit, committed for reference only |

## How to Build Locally

```bash
hugo server -D   # serve with drafts, hot-reload at http://localhost:1313
hugo --gc        # production build into public/
```

Hugo v0.126.0 extended is used in CI. Local version should be >= 0.126.0.

## Post Dates

For every new blog post or game review, set `date:` to **today at midnight UTC**: `YYYY-MM-DDT00:00:00Z`, using the UTC date (`date -u +%F`).
Hugo skips future-dated content, so a later time (e.g. `T10:00:00Z`) can keep the post from publishing on push.

## How to Add a Blog Post

Create `content/blog/my-post-title.md`:

```markdown
---
title: "My Post Title"
date: 2026-06-12T00:00:00Z
description: "One-line summary shown in the blog card."
author: "Paulo Bento"
type: "post"
---

Post content in Markdown.
```

## How to Add a Game Review

1. Add a cover image to `static/images/game-reviews/game-slug.jpg`
2. Create `content/game-reviews/game-slug.md`:

```markdown
---
title: "Game Title"
date: 2026-06-12T00:00:00Z
description: "One-line tagline shown on the card."
image: "/images/game-reviews/game-slug.jpg"
rating: 8
---

Written review in Markdown.
```

Rating is 1-10. A disclaimer ("personal opinions, not professional reviews") is automatically shown on every review page.

## How to Update Professional Info

All sections are in `data/homepage.yml`:

| Key | What it controls |
|---|---|
| `banner.title` | The large heading on the homepage |
| `social` | Social media links (linkedin, github, etc.) |
| `about.content` | Short bio shown below social icons |
| `skill.item` | Skills grid — each item has `title`, `logo`, `description` |
| `experience.item` | Experience cards — `logo`, `title`, `company`, `duration` |
| `certifications.item` | Certification badges — `title`, `image`, `url` |
| `education.item` | Education cards — `title`, `year`, `academy`, `image` |
| `portfolio.item` | Projects shown on homepage — `title`, `image`, `description`, `tools`, `link` |

## Deployment

Push to `main`. GitHub Actions (`.github/workflows/hugo.yaml`) builds with Hugo and deploys to GitHub Pages automatically. No manual steps needed.
