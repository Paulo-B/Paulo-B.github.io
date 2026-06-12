# Site Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Restructure Paulo-B.github.io into a professional portfolio site with a consolidated homepage, blog, and a new game reviews section — without modifying the theme submodule.

**Architecture:** All customisation goes through Hugo's layout override system (`layouts/` at repo root takes precedence over `themes/hugo-developer-portfolio/layouts/`), data files (`data/homepage.yml`), and content files (`content/`). The theme submodule is never touched.

**Tech Stack:** Hugo 0.126.0+ extended, UIkit 3.2.6 (CDN), Go modules, GitHub Actions → GitHub Pages

---

## File Map

| Action | Path | Purpose |
|---|---|---|
| Modify | `hugo.toml` | Nav menu + footer email |
| Modify | `data/homepage.yml` | Fix typo, add portfolio section |
| Modify | `content/blog/_index.md` | Real description |
| Create | `content/game-reviews/_index.md` | Section index |
| Create | `content/game-reviews/stardew-valley.md` | Sample review |
| Create | `layouts/game-reviews/list.html` | Card grid with image + rating |
| Create | `layouts/game-reviews/single.html` | Full review with disclaimer |
| Create | `static/images/game-reviews/.gitkeep` | Directory for cover images |
| Create | `CLAUDE.md` | Project docs for future sessions |

---

## Task 1: Update navigation and footer in hugo.toml

**Files:**
- Modify: `hugo.toml`

- [ ] **Step 1: Replace the menu and footer in hugo.toml**

Open `hugo.toml` and replace the entire `[menu]` block and `[params.footer]` block with:

```toml
[menu]

  [[menu.main]]
  name = "About"
  URL = "about"
  weight = 1

  [[menu.main]]
  name = "Blog"
  URL = "blog"
  weight = 2

  [[menu.main]]
  name = "Game Reviews"
  URL = "game-reviews"
  weight = 3

[params]
home = "Home"
description = "Paulo Bento's Lair"
author = "Paulo Bento"
year = 2024

  [params.footer]
  email = ""
  address = "Portugal"
  googlemaps = ""
```

The Portfolio entry is removed. Game Reviews is added. The placeholder footer email is cleared.

- [ ] **Step 2: Verify Hugo builds without errors**

```bash
hugo --gc 2>&1 | tail -5
```

Expected: `Built in X ms` with no `ERROR` lines.

- [ ] **Step 3: Commit**

```bash
git add hugo.toml
git commit -m "feat: update nav — remove portfolio, add game reviews"
```

---

## Task 2: Fix homepage data — typo and portfolio section

**Files:**
- Modify: `data/homepage.yml`

- [ ] **Step 1: Fix the Experience typo**

In `data/homepage.yml`, find the K8S skill item and fix the description:

```yaml
    - title: K8S
      logo: "https://github.com/kubernetes/kubernetes/blob/master/logo/logo.png?raw=true"
      description: |
        Experience with various flavours of K8s, including Rancher on-prem, EKS and AKS. Managing, creating, administrating clusters.
```

(Change "Experinece" → "Experience", "adminstrating" → "administrating")

- [ ] **Step 2: Update the xGeeks experience end date**

```yaml
    - logo: "https://media.licdn.com/dms/image/D4D0BAQH5O7efvI-NjA/company-logo_200_200/0/1692266279301/xgeeks_logo?e=2147483647&v=beta&t=55P71Cl0YC7CciuqsIVMwcx3HyapsRvv1vCNrJ4ksyQ"
      title: Devops Engineer
      company: xGeeks
      duration: January 2024 - Present
```

(Replace `January 2024 - ?` with `January 2024 - Present`)

- [ ] **Step 3: Add portfolio section at the end of homepage.yml**

Append to the end of `data/homepage.yml`:

```yaml
portfolio:
  enable: true
  item:
    - title: "Your Project Name"
      image: "/images/portfolio/placeholder.png"
      description: "Replace this with a real project description."
      tools:
        - kubernetes
        - terraform
      link: "https://github.com/Paulo-B"
```

- [ ] **Step 4: Verify build**

```bash
hugo --gc 2>&1 | tail -5
```

Expected: `Built in X ms`, no `ERROR` lines.

- [ ] **Step 5: Commit**

```bash
git add data/homepage.yml
git commit -m "fix: typos in homepage data, add portfolio section placeholder"
```

---

## Task 3: Clean up blog section index

**Files:**
- Modify: `content/blog/_index.md`

- [ ] **Step 1: Replace the placeholder description**

Replace the entire contents of `content/blog/_index.md` with:

```markdown
---
title: "Blog"
date: 2023-05-15T12:14:34+06:00
description: "Random thoughts, notes, and things I found interesting."
---
```

- [ ] **Step 2: Commit**

```bash
git add content/blog/_index.md
git commit -m "fix: replace placeholder blog description"
```

---

## Task 4: Create game reviews content structure

**Files:**
- Create: `content/game-reviews/_index.md`
- Create: `content/game-reviews/stardew-valley.md`
- Create: `static/images/game-reviews/.gitkeep`

- [ ] **Step 1: Create the section index**

Create `content/game-reviews/_index.md`:

```markdown
---
title: "Game Reviews"
date: 2026-06-12T00:00:00Z
description: "Personal game reviews. Not professional. Just a DevOps guy with opinions."
---
```

- [ ] **Step 2: Create the image directory**

```bash
mkdir -p static/images/game-reviews
touch static/images/game-reviews/.gitkeep
```

- [ ] **Step 3: Create a sample review**

Create `content/game-reviews/stardew-valley.md`:

```markdown
---
title: "Stardew Valley"
date: 2026-06-12T00:00:00Z
description: "A farming sim that somehow ate 300 hours of my life."
image: "/images/game-reviews/stardew-valley.jpg"
rating: 9
---

Stardew Valley is one of those games you pick up for "just an hour" and then look up to find it's 2am. Created by a single developer (ConcernedApe), it is a masterclass in satisfying game loops.

You inherit your grandfather's farm and must restore it from an overgrown mess to a thriving homestead. Along the way you fish, mine, fight monsters in a dungeon, build relationships with townspeople, and slowly unravel a light story about a soulless corporation vs a small community.

The game never rushes you. There is no fail state. It respects your time while consistently giving you something to work toward.

**What I liked:**
- Endlessly relaxing, never boring
- Incredible value for the price
- Regular free updates years after launch
- Works great in short sessions or marathon runs

**What could be better:**
- Multiplayer is functional but clearly bolted on
- Late-game content thins out compared to early-game density

If you have never played it, you owe it to yourself to try.
```

- [ ] **Step 4: Verify Hugo recognises the new section**

```bash
hugo list all 2>&1 | grep game-reviews
```

Expected: one line for `stardew-valley` in the `game-reviews` section.

- [ ] **Step 5: Commit**

```bash
git add content/game-reviews/ static/images/game-reviews/
git commit -m "feat: add game-reviews content section with sample review"
```

---

## Task 5: Create game reviews list layout

**Files:**
- Create: `layouts/game-reviews/list.html`

- [ ] **Step 1: Create the layouts directory**

```bash
mkdir -p layouts/game-reviews
```

- [ ] **Step 2: Create list.html**

Create `layouts/game-reviews/list.html`:

```html
{{ define "main" }}

{{ partial "page-title.html" . }}

<section class="uk-margin-large-bottom">
    <div class="uk-container">
        <div class="uk-grid-small uk-child-width-1-3@m uk-child-width-1-2@s" uk-grid uk-height-match="target: > div > .uk-card">
            {{ range .Data.Pages.ByPublishDate.Reverse }}
            <div>
                <div class="uk-card uk-card-default uk-width-1@m">
                    {{ if .Params.image }}
                    <div class="uk-card-media-top">
                        <img src="{{ .Params.image }}" alt="{{ .Title }}" style="width:100%; height:200px; object-fit:cover;">
                    </div>
                    {{ end }}
                    <div class="uk-card-body">
                        <div class="uk-flex uk-flex-between uk-flex-middle uk-margin-small-bottom">
                            <h3 class="uk-card-title uk-margin-remove">{{ .Title }}</h3>
                            {{ if .Params.rating }}
                            <span class="uk-badge" style="background:#1e87f0; font-size:0.9rem; padding:4px 8px;">{{ .Params.rating }}/10</span>
                            {{ end }}
                        </div>
                        <p class="uk-text-meta uk-margin-remove-top">
                            <time datetime="{{ .Date }}">{{ .Date.Format "Jan 2, 2006" }}</time>
                        </p>
                        <p>{{ .Params.description }}</p>
                    </div>
                    <div class="uk-card-footer">
                        <a href="{{ .Permalink | relURL }}" class="uk-button uk-button-text">Read review</a>
                    </div>
                </div>
            </div>
            {{ end }}
        </div>
    </div>
</section>

{{ end }}
```

- [ ] **Step 3: Start hugo server and verify the list page renders**

```bash
hugo server -D --port 1313 &
sleep 2
curl -s http://localhost:1313/game-reviews/ | grep -c "uk-card"
kill %1
```

Expected: a number ≥ 1 (at least one card rendered).

- [ ] **Step 4: Commit**

```bash
git add layouts/game-reviews/list.html
git commit -m "feat: add game-reviews list layout with cover image and rating badge"
```

---

## Task 6: Create game reviews single layout

**Files:**
- Create: `layouts/game-reviews/single.html`

- [ ] **Step 1: Create single.html**

Create `layouts/game-reviews/single.html`:

```html
{{ define "main" }}

<style>
  .review-disclaimer {
    background: #fff9e6;
    border-left: 4px solid #faa05a;
    padding: 12px 20px;
    margin-bottom: 2rem;
    font-size: 0.9rem;
  }
  .review-cover {
    display: block;
    max-height: 400px;
    max-width: 100%;
    object-fit: cover;
    margin: 0 auto 2rem;
    border-radius: 4px;
    box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  }
  .review-rating {
    font-size: 2.5rem;
    font-weight: 700;
    color: #1e87f0;
    text-align: center;
    margin-bottom: 0.25rem;
  }
  .review-rating-label {
    text-align: center;
    color: #999;
    font-size: 0.85rem;
    margin-bottom: 2rem;
  }
  .review-content p { margin-bottom: 1em; }
  .review-content h1,
  .review-content h2,
  .review-content h3 {
    color: #000;
    font-family: Roboto, sans-serif;
    margin-top: 2rem;
  }
  .review-content {
    width: 90vw;
    max-width: 800px;
    margin: 0 auto;
  }
</style>

<section>
  <div class="uk-container uk-margin-large-bottom uk-margin-medium-top">

    <div class="review-disclaimer">
      ⚠️ These are personal opinions and not professional reviews. Take them with a grain of salt.
    </div>

    {{ if .Params.image }}
    <img class="review-cover" src="{{ .Params.image }}" alt="{{ .Title }}">
    {{ end }}

    <h1 class="uk-text-center uk-heading-medium">{{ .Title }}</h1>
    <p class="uk-text-center uk-text-meta">{{ .PublishDate.Format "Jan 02, 2006" }}</p>

    {{ if .Params.rating }}
    <p class="review-rating">{{ .Params.rating }}<span style="font-size:1.5rem; color:#999;">/10</span></p>
    <p class="review-rating-label">Personal rating</p>
    {{ end }}

    <hr>

    <div class="review-content">
      {{ .Content }}
    </div>

  </div>
</section>

{{ end }}
```

- [ ] **Step 2: Start hugo server and verify the single review page renders**

```bash
hugo server -D --port 1313 &
sleep 2
curl -s http://localhost:1313/game-reviews/stardew-valley/ | grep -c "review-disclaimer"
kill %1
```

Expected: `1` (disclaimer div is present).

- [ ] **Step 3: Verify full build succeeds**

```bash
hugo --gc --minify 2>&1 | tail -5
```

Expected: `Built in X ms`, no `ERROR` lines.

- [ ] **Step 4: Commit**

```bash
git add layouts/game-reviews/single.html
git commit -m "feat: add game-reviews single layout with disclaimer and rating display"
```

---

## Task 7: Create CLAUDE.md

**Files:**
- Create: `CLAUDE.md`

- [ ] **Step 1: Create CLAUDE.md at the repo root**

Create `CLAUDE.md`:

```markdown
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

Hugo v0.126.0 extended is used in CI. Local version should be ≥ 0.126.0.

## How to Add a Blog Post

Create `content/blog/my-post-title.md`:

```markdown
---
title: "My Post Title"
date: 2026-06-12T10:00:00Z
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
date: 2026-06-12T10:00:00Z
description: "One-line tagline shown on the card."
image: "/images/game-reviews/game-slug.jpg"
rating: 8
---

Written review in Markdown.
```

Rating is 1–10. A disclaimer ("personal opinions, not professional reviews") is automatically shown on every review page.

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
```

- [ ] **Step 2: Verify build one final time**

```bash
hugo --gc --minify 2>&1 | tail -5
```

Expected: `Built in X ms`, no `ERROR` lines.

- [ ] **Step 3: Commit**

```bash
git add CLAUDE.md
git commit -m "docs: add CLAUDE.md with site structure and contribution guide"
```
