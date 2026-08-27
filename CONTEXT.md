# Repository Context — Zhiyi-Zhang.github.io

> Personal academic website for **Zhiyi Zhang** (Research Scientist @ Meta, Ph.D. CS UCLA 2021).  
> Generated: 2026-08-26 · Branch: `master` · Remote: `git@github.com:Zhiyi-Zhang/Zhiyi-Zhang.github.io.git`  
> Purpose of this file: durable context for future agent sessions and human maintainers — what the repo is, how it builds, where things live, and how to change it safely.

---

## 1. One-Line Summary

Jekyll static site hosted on **GitHub Pages** at `https://zhiyi-zhang.com` (custom domain via `CNAME`), built from the `master` branch with the **HTML5 UP Editorial** theme (ported to Jekyll via `_layouts/default.html` + `assets/sass/`). No CI workflow — GitHub Pages builds Jekyll automatically on push.

---

## 2. Tech Stack

| Layer | Detail |
|-------|--------|
| Static generator | **Jekyll ~3.9** (pinned via `Gemfile` + `github-pages` gem — not Jekyll 4) |
| Theme | `minima ~2.5` as gem dependency, but visually overridden by HTML5 UP Editorial (`assets/sass/main.scss` + `assets/css/main.css`) |
| Markdown | `kramdown` |
| Plugins | `jekyll-feed ~0.12` |
| Ruby helpers | `webrick ~1.8` (required for Ruby 3+ `jekyll serve`), `tzinfo`, `wdm` on Windows |
| Frontend | Vanilla HTML + jQuery (`assets/js/jquery.min.js`, `browser.min.js`, `breakpoints.min.js`, `util.js`, `main.js`) + Font Awesome (kit `7d40fa654a`) + Google Fonts (Open Sans / Roboto Slab) |
| Hosting | GitHub Pages (branch `master`, root `/`) |
| Domain | `zhiyi-zhang.com` (`CNAME` file) · DNS/HTTPS: **Cloudflare** · Registrar: **GoDaddy** |
| Local serve | `bundle exec jekyll serve` |

Key config: [`_config.yml`](./_config.yml) — `title`, `email`, `description`, `url: https://zhiyi-zhang.com`, `baseurl: ""`, `markdown: kramdown`, `theme: minima`.

---

## 3. Directory Map

```
.
├── _config.yml              # Jekyll site config (title, url, theme, plugins)
├── Gemfile / Gemfile.lock   # Ruby deps (github-pages, minima, jekyll-feed, webrick)
├── CNAME                    # → zhiyi-zhang.com
├── index.html               # Homepage (banner + News + Teaching + Recent Posts)
├── publications.html         # Full publication list (journal/conf, workshop, tech reports)
├── talks.html               # Talks (2018–2019)
├── research-topics.html     # Research topics (currently commented out in nav)
├── profile.html             # Embeds content/zz-cv.pdf via <embed>
├── 404.html                 # Simple 404 with layout: default
├── enigma_https.html        # Standalone test page: fetch http/https to local.zhiyi-zhang.com:10088
├── enigma_turn.html         # Standalone test page: WebRTC TURN / RTCPeerConnection to 127.0.0.1:5000
├── test-fingerprinting.html # Standalone test page: device/browser/OS fingerprinting
├── ca.crt                   # Test CA certificate
├── icon.png                 # Favicon (41613 bytes, also /icon.png in layouts)
├── _layouts/
│   ├── default.html         # Main wrapper: <head>, #wrapper > #main + sidebar, script includes
│   └── bibtex.html          # Minimal <pre>{{content}}</pre> wrapper for bibtex pages
├── _includes/
│   ├── header.html          # Top header: logo + social icons (GitHub, GitLab, LinkedIn, ORCID, Scholar)
│   └── navigator.html       # Sidebar: nav menu (Homepage/Publication/Talks/Profile) + Contact + footer
├── assets/
│   ├── css/main.css         # Compiled CSS (from SASS, includes reset + theme)
│   ├── js/{jquery,browser,breakpoints,util,main}.js
│   └── sass/{main.scss, base/, components/, layout/, libs/}  # Editorial theme source
├── content/
│   ├── zz-cv.pdf / zz-resume.pdf / dissertation.pdf / prospectus.pdf
│   ├── client-conf.txt      # NDNCERT client config example
│   └── generic.html / elements.html
├── posts/
│   ├── ndn-code-review.md
│   ├── try-ndncert-today.md  # NDNCERT tutorial (2020-03-20)
│   └── cs217-apr-20-2020.html
├── publications/
│   ├── bibtex/              # BibTeX files per paper (linked from publications.html)
│   └── *.pdf                # Hosted PDFs (ccr-hostmodel-18.pdf, etc.)
├── images/                  # headpic.jpg, security.jpg, ndn-lite-logo.jpg, etc.
└── .gitignore               # _site, .sass-cache, .jekyll-cache, vendor, .bundle, tmp, DS_Store
```

---

## 4. Page & Layout Inventory

### Top-level pages (all use `layout: default` except standalone test pages)

- **`index.html`** (`title: zhiyi-home`): Banner (name, title, Chinese name, bio), News (1 visible article: PETS 2024 PC, rest commented out), Teaching (3 TA entries), Recent Posts (3 links to `posts/`).
- **`publications.html`** (`title: zhiyi-publications`): Sections — Journal & Full Conference Papers, Workshop/Demo/Poster, Technical Reports. Each entry is a `.row` with 8-col title/authors/venue + 2-col BibTeX button + 2-col Read button.
- **`talks.html`**: Grouped by year (2019, 2018), same row pattern.
- **`research-topics.html`**: `/system/security` overview + subtopics (Sovereign smart home, AuditShare PII, DLedger, etc.). **Hidden from nav** — commented out in `navigator.html`.
- **`profile.html`**: Single `<embed src="content/zz-cv.pdf" width="100%" height="1300px"/>`.
- **`404.html`**: Centered container, h1 404.

### Standalone pages (no Jekyll layout — raw HTML)

- **`enigma_https.html`**: Two buttons fetching `http://local.zhiyi-zhang.com:10088` and `https://local.zhiyi-zhang.com:10088`.
- **`enigma_turn.html`**: WebRTC test against `turn:127.0.0.1:5000` (`hellohello`/`secretsecret`), logs ICE candidates/connection state.
- **`test-fingerprinting.html`**: Fingerprinting harness (Navigator, UA parsing, Screen, Touch, Mobile APIs).

### Layouts & Includes

- **`_layouts/default.html`**: HTML5 UP Editorial shell — `<title>{{ page.title }}</title>`, viewport, `/icon.png`, `main.css`, Font Awesome kit, `#wrapper > #main > .inner > header.html + {{content}}` + `navigator.html` sidebar + 5 JS includes. `is-preload` body class.
- **`_layouts/bibtex.html`**: Bare `{{content}}` in `<pre>`.
- **`_includes/header.html`**: Logo link + 5 social icons.
- **`_includes/navigator.html`**: Sidebar menu (Homepage, Publication, Talks, Profile; Research Topics commented out), Contact list (email `zhiyi@cs.ucla.edu`, GitHub, GitLab, LinkedIn, ORCID, Scholar), footer copyright.

### Assets

- **SASS entry**: `assets/sass/main.scss` imports `libs/vars, functions, mixins, vendor, breakpoints, html-grid`, `font-awesome.min.css`, Google Fonts, then breakpoint map (`xlarge` 1281–1680 … `xxsmall` ≤360).
- **JS**: `jquery.min.js`, `browser.min.js`, `breakpoints.min.js`, `util.js`, `main.js` (Editorial's responsive/off-canvas logic).

---

## 5. Data & Content Notes

- **Author identity**: Zhiyi Zhang — Meta Research Scientist (PETs), UCLA Ph.D. 2021, advisor Lixia Zhang, committee Kleinrock/Lu/Reiher/Afanasyev, IRL lab. NDN / `/system/security` focus. Nankai University B.E. 2016.
- **Publications**: Hard-coded HTML in `publications.html` (not a Jekyll collection/data file). BibTeX buttons link to `publications/bibtex/`; Read buttons to external DOI/arXiv/techrxiv. Adding a paper means editing `publications.html` by hand and optionally adding `publications/bibtex/*.bib` + `publications/*.pdf`.
- **Posts**: Mixed `.md` and `.html` in `posts/` — linked manually from `index.html` Recent Posts. Not using `_posts/` (Jekyll's dated collection).
- **PDFs**: `content/zz-cv.pdf` (61K), `content/zz-resume.pdf`, `content/dissertation.pdf` (5.4M), `content/prospectus.pdf` displayed via `profile.html` embed.
- **Images**: `images/headpic.jpg` (homepage banner), plus topic images (`security.jpg`, `ndn-lite-logo.jpg`, `leaker.jpg`, `dledger.png`, `fitt.png`, etc.)

---

## 6. Build / Serve / Deploy

```bash
# 1. Install (macOS — see https://jekyllrb.com/docs/installation/macos/)
bundle install

# 2. Serve locally (auto-regenerates, but _config.yml changes need restart)
bundle exec jekyll serve
# → http://127.0.0.1:4000  (_site/ is the output, gitignored)

# 3. Update deps
bundle update github-pages

# 4. Deploy — push to master (GitHub Pages builds automatically)
git push origin master
```

- No `Makefile`, no GitHub Actions workflow, no `package.json`.
- `.gitignore` excludes `_site`, `.sass-cache`, `.jekyll-cache`, `vendor`, `.bundle`, `tmp`, `DS_Store`.
- `url` in `_config.yml` must stay `https://zhiyi-zhang.com` + `baseurl: ""` + `CNAME` for custom domain to work.

---

## 7. Conventions & Gotchas

- **HTML comments as feature flags**: Many News articles and nav items are commented out rather than deleted — uncomment to restore. `research-topics.html` exists but is hidden from the sidebar.
- **Jekyll version lock**: `gem "jekyll", "~> 3.9"` + `github-pages` — do **not** bump to Jekyll 4 without checking Pages compatibility; `webrick` is explicitly added for Ruby 3.
- **Minima vs Editorial**: `minima` is declared but Editorial's SASS/CSS/JS are the actual theme. Changing `theme:` may not visibly affect the site unless `assets/` are also swapped.
- **Hard-coded publication HTML**: No data file — edits require careful copy/paste of the `.row` block (8-2-2 columns). Keep `id="content"` on `<h4>` as in existing entries.
- **Font Awesome**: Loaded via `kit.fontawesome.com/7d40fa654a.js` — if icons break, the kit may have expired; fall back to CDN `font-awesome.min.css` (already imported in SASS).
- **Experimental pages** (`enigma_*`, `test-fingerprinting.html`, `ca.crt`) are intentionally public; they hit `local.zhiyi-zhang.com` / `127.0.0.1` and are used for PETs/network experiments. Don't remove without checking ongoing studies.

---

## 8. Recent Git History (last 20)

```
d608278 update
bb2bc61 Bump nokogiri from 1.18.8 to 1.18.9
dcadf74 minor
ff1d081 Update fingerprint testing page
4236b61 update
e58e9da update testing page
8a64876 Add a testing webpage
94ac643 add another testing html file
c981c12 update readme
6b0be0e update enigma test html
d517a98 update enigma test html
a031987 Bump uri from 0.13.0 to 0.13.2
a26c857 Bump rexml from 3.2.6 to 3.3.9
57f196b Bump webrick from 1.7.0 to 1.8.2
0a70285 Bump nokogiri from 1.16.4 to 1.18.8
02361d2 add a testing ca certificate
85b5c29 adding a test file
53e92fe update publication list
86b0b33 minor update
77054d6 Bump nokogiri from 1.13.3 to 1.14.3
```

Dependabot branches exist for `activesupport`, `faraday`, `nokogiri`, `rexml`, `tzinfo`, `uri`.

---

## 9. How to Make Common Changes

| Task | Files to touch |
|------|---------------|
| Add a publication | `publications.html` (copy a `.row` block) + `publications/bibtex/<key>.bib` + optionally `publications/<pdf>` |
| Add a news item | `index.html` → News `<div class="posts">` |
| Add a blog post | `posts/<slug>.md` (with `layout: default` front matter) + link in `index.html` Recent Posts |
| Update CV | Replace `content/zz-cv.pdf` (also `content/zz-resume.pdf` if needed) |
| Change nav | `_includes/navigator.html` |
| Change header/social | `_includes/header.html` + `_includes/navigator.html` Contact section |
| Theme tweaks | `assets/sass/**/*.scss` then `assets/css/main.css` is compiled; or edit `main.css` directly for quick fixes |
| Site metadata | `_config.yml` (`title`, `email`, `description`, `url`) |

---

## 10. Future Agent Checklist

1. **Read this file + `_config.yml` + `_layouts/default.html`** before any edit — the Editorial wrapper is load-bearing.
2. Use `bundle exec jekyll serve` to verify; `_site/` is ephemeral.
3. Keep `CNAME` and `url` in sync.
4. Prefer `muse.edit_file` over `muse.write_file` for surgical HTML edits (the publication list is long and repetition-prone).
5. After `bundle install`/`bundle update`, check `git status` for `Gemfile.lock` drift and keep it intentional.
6. Don't delete `enigma_*` / `test-fingerprinting.html` / `ca.crt` without confirming they're not in active use.

---

*This file is not deployed (add to `exclude:` in `_config.yml` if you want to hide it from `_site`). Keep it at the repo root for discoverability.*
