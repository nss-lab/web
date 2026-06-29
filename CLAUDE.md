# CLAUDE.md — NSS Lab Website (nss-lab/web)

Guidance for Claude Code (and humans) working in this repository.

## What this project is

This repo hosts the **new** website for the **NSS Lab** (Network and System
Security Laboratory) at KAIST, led by Prof. **Seungwon Shin**.

We are **migrating away from the legacy site at https://nss.kaist.ac.kr**, which
runs a 10+ year old WordPress install on a 15+ year old machine. That stack is
unstable and could go down permanently at any time. The new site is a **static
site (Jekyll) on GitHub Pages** — stable, free, and easy to maintain — that
**reproduces the look, structure, and content of the legacy site as closely as is
reasonable** (a faithful recreation, not a pixel-perfect clone).

The lab owns the source site and its content, so copying text, images, layout,
and styling from the live site is fully authorized. **Save copies of needed text
and images into this repo while the old server is still up** — it will be retired.

## Current status

Built and verified (real `jekyll build`, June 2026):

- Jekyll skeleton: `_config.yml`, shared layouts, header/footer/people includes,
  stylesheet (navy `#000080` / Poppins), and minimal JS (mobile menu + dropdown).
- **Homepage** (`docs/index.html`): hero → research cards → news feed → location map.
- Inner pages live with real or seed content: Research (full), Faculty (Prof. Shin),
  Doctoral Students (11-name roster, no photos), Publications (recent seed),
  Awards (seed). Master's / Alumni / Gallery are stubs ready for content.

**Still to do:** import the full publications list (2006–2026, ~200+ entries) and
awards from the legacy site; fill Master's/Alumni rosters; decide on and re-host
gallery images; verify all names/emails against the live site; attach the custom
domain.

## Architecture — Jekyll on GitHub Pages

- **Static site generator: Jekyll**, which GitHub Pages **builds for you on every
  push** (no GitHub Action, no local build required to publish). Content is written
  in Markdown/HTML + small YAML data files; shared chrome lives in one layout.
- **Publish source:** GitHub Pages serves from the **`docs/`** directory on `main`.
  Keep this setting as "Deploy from a branch → `main` / `/docs`".
- There is **no `.nojekyll`** file (it was removed) — its presence would disable
  the Jekyll build and the templates would render as raw `{{ }}`.
- If a build ever fails, GitHub **keeps the last working version live** and emails
  the error — the site does not go blank.

### Repository layout

```
docs/                     # <- GitHub Pages builds & serves this folder
  _config.yml             # site settings: title, baseurl, lab_email, address
  Gemfile                 # for LOCAL preview only (uses github-pages gem)
  index.html              # Homepage (hero, research cards, news, map)
  research.html           # /research/  (loops _data/research.yml)
  publications.html       # /publications/  (loops _data/publications.yml)
  awards.md  gallery.md   # /awards/  /gallery/
  people/
    faculty.md            # /people/faculty/  (prose)
    phd.html              # /people/phd/      (include people.html + _data/phd.yml)
    masters.html alumni.html
  _layouts/
    default.html          # <head>, header, footer, scripts — the page shell
    page.html             # inner-page wrapper (title hero + narrow content)
  _includes/
    header.html footer.html   # shared nav + footer (edit once, applies everywhere)
    people.html           # renders a roster as name + link buttons (no photos)
  _data/
    research.yml news.yml publications.yml   # content for the looped sections
    phd.yml masters.yml alumni.yml           # people rosters
  assets/css/style.css  assets/js/main.js
CLAUDE.md  README.md  .gitignore             # repo root (not published)
```

## How to update content (the maintainable bits)

Edits are simple, single-file changes. After editing, commit & push — GitHub
rebuilds automatically.

- **Add a news item:** edit `docs/_data/news.yml`, add a block at the top:
  ```yaml
  - date: July 2026
    title: "Paper Title Here"
    venue: Accepted to VENUE 2026   # optional
    link: https://...               # optional
  ```
- **Add a publication:** edit `docs/_data/publications.yml`, add a line under the
  right year (or add a new `- year:` block):
  ```yaml
  - year: 2026
    items:
      - "Title (Venue 2026)"
  ```
- **Add / edit a person:** edit `docs/_data/phd.yml` (or `masters.yml`,
  `alumni.yml`). One block per person; a button appears only for fields present:
  ```yaml
  - name: Jane Doe
    email: jane@kaist.ac.kr
    github: janedoe        # -> github.com/janedoe
    linkedin: jane-doe     # -> linkedin.com/in/jane-doe
    homepage: https://...
    scholar: https://...
  ```
  **No photos** — by design, for privacy. Don't add image fields.
- **Edit research areas:** `docs/_data/research.yml` (drives both the homepage
  cards and `/research/`).
- **Change contact info / lab name:** `docs/_config.yml` (`lab_email`, `address`,
  `title`). Used across the footer and pages via `{{ site.lab_email }}` etc.
- **Edit the nav or footer:** `docs/_includes/header.html` / `footer.html` once.

## Local preview (optional — only for developers)

You never need this to publish; GitHub builds the site. To preview locally:

```bash
cd docs
bundle install          # first time
bundle exec jekyll serve
# open the printed URL, e.g. http://localhost:4000/web/
```

Note: a clean `gem install jekyll` needs the Ruby dev headers
(`sudo apt install ruby-dev` on Debian/Ubuntu) to compile a serve-only native
extension. This is a local-tooling requirement only and has nothing to do with
the GitHub Pages build.

## Deployment & URLs

- Push to `main`; GitHub Pages rebuilds `docs/` and publishes. No other step.
- Current URL (project page): **https://nss-lab.github.io/web/**, so
  `_config.yml` has `baseurl: "/web"`. **All internal links/assets use the
  `relative_url` filter** so they respect the baseurl — keep doing this.
- **Custom domain (e.g. nss.kaist.ac.kr):** when attached, the site serves at the
  domain root, so set `baseurl: ""` in `_config.yml` and add a `docs/CNAME` file
  containing the domain. (DNS for `nss.kaist.ac.kr` is managed by KAIST.)

## Source site reference (what we are recreating)

Legacy site: **https://nss.kaist.ac.kr** — WordPress 5.4.19 + Business Gravity
theme + Elementor, Bootstrap, OwlCarousel, Font Awesome. We do **not** keep any
of that; we extract content/design and rebuild lean. Page `<title>`:
`NSS – KAIST, Network and System Security Laboratory`.

### Navigation / site map

Legacy uses `?page_id=N` URLs; the new site uses the clean paths.

| Menu item | Legacy URL | New path |
|---|---|---|
| Home | `/` | `/` |
| People ▸ Faculty | `?page_id=29` | `/people/faculty/` |
| People ▸ Doctoral Students | `?page_id=6554` | `/people/phd/` |
| People ▸ Master's Students | `?page_id=6448` | `/people/masters/` |
| People ▸ Alumni | `?page_id=6447` | `/people/alumni/` |
| Research | `?page_id=7155` | `/research/` |
| Publication/Talk | `?page_id=6856` | `/publications/` |
| Award | `?page_id=7260` | `/awards/` |
| Gallery | `?page_id=62` | `/gallery/` |

### Homepage sections (legacy, top to bottom)

Header nav → hero ("Welcome to the NSS Lab @ KAIST") → research areas →
news feed (paper acceptances, reverse chronological) → photo gallery carousel →
location (Google Map) → footer. The new homepage mirrors this minus the photo
gallery (pending image decisions).

### Page content inventory (for the ongoing clone)

- **Faculty** — Prof. Seungwon Shin: Associate Professor, School of Electrical
  Engineering, KAIST; Ph.D. Texas A&M; prior MIT visiting scientist, SRI
  internships, ONF; joined KAIST 2013. ⚠️ Verify exact office/email on the live
  page — do NOT trust auto-summaries (one returned a hallucinated email).
- **Doctoral Students** — ~11 students; names captured into `_data/phd.yml`
  (verify spelling, add contact links).
- **Master's / Alumni** — rosters not yet captured.
- **Research** — four areas (SDN/NFV Security, Cyber Threat Intelligence,
  Container Security, Blockchain Security), each with a paragraph — in
  `_data/research.yml`.
- **Publications** — by year, reverse chronological, ~200+ entries (2006–2026);
  some tagged "(Distinguished Paper Award)"/"(to appear)"; co-first authors `*`.
  Only recent items seeded so far.
- **Award / Gallery** — lists/photos not yet fully captured.

### Design system (extracted from the legacy CSS — now in `style.css`)

- **Primary brand color:** `#000080` (navy). Text `#1c1f2b` / muted `#6e6e6e`.
- **Font:** **Poppins** (Google Fonts, weights 300–800).
- These live as CSS variables at the top of `docs/assets/css/style.css`.

## Verified contact details (from raw legacy HTML)

- **Lab email:** `nsslab@kaist.ac.kr`
- **Address:** 291 Daehak-ro, Yuseong-gu, Daejeon, Korea

## Conventions & gotchas

- **Keep everything inside `docs/`** — files elsewhere are not published.
- **Use the `relative_url` filter** for every internal link/asset (baseurl-safe).
- **Do not re-add `.nojekyll`** — it disables the Jekyll build.
- **Re-extract precise content from the live site** (names, emails, paper titles,
  dates) rather than trusting summaries; save it here while the old server is up.
- **Host images locally** in `docs/assets/img/` — never hotlink the legacy
  `wp-content/uploads/` URLs (they die with the old server).
- **No member photos** anywhere (privacy decision).
- `docs/_site/`, caches, and `Gemfile.lock` are git-ignored build artifacts.
