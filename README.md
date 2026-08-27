# Zhiyi Zhang — Personal Website

Live at **https://zhiyi-zhang.com** (custom domain via `CNAME`, served by GitHub Pages).

## Stack

- **Jekyll 3.9** via `github-pages` (231) + `minima` theme overridden by **HTML5 UP Editorial** (`assets/sass/` → `assets/css/main.css`)
- Plugins: `jekyll-feed`, `jekyll-seo-tag`, `jekyll-sitemap`
- Domain: `zhiyi-zhang.com` — DNS/HTTPS **Cloudflare**, registrar **GoDaddy**

## Local development

```bash
# macOS prerequisites: https://jekyllrb.com/docs/installation/macos/
bundle install
bundle exec jekyll serve
# → http://127.0.0.1:4000  (_site/ is gitignored)

# config changes need a restart
bundle exec jekyll serve --livereload
```

## Common tasks

| Task | Where |
|------|-------|
| Add publication | `publications.html` (copy a `.row` block: `8-2-2` cols) + `publications/bibtex/*.html` + optional PDF |
| Add news | `index.html` → `News` section (`<h3>` per entry, see placeholder note) |
| Add post | `posts/<slug>.md` (front matter `layout: default`) + link in `index.html` Recent Posts |
| Update CV | Replace `content/zz-cv.pdf` (+ `zz-resume.pdf`); `profile.html` renders via `<object>` with download button |
| Edit nav / header | `_includes/navigator.html` / `_includes/header.html` |
| Theme tweak | `assets/sass/**/*.scss` → `assets/css/main.css` or edit `main.css` directly |
| Site meta | `_config.yml` (`title`, `description`, `author`, `url`, `exclude`, `plugins`) |

## Deployment

Push to `master` — GitHub Pages builds automatically (legacy Pages build, no Action required). Keep `CNAME` and `url` in sync.

```bash
bundle update github-pages
bundle update # to refresh lockfile after Gemfile changes
```

Dependabot handles `Gemfile.lock` security bumps; they appear as PRs and are merged via rebase.

## Project notes

- Experimental pages `enigma_https.html`, `enigma_turn.html`, `test-fingerprinting.html`, `ca.crt` are `noindex` via `robots.txt` and excluded from sitemap.
- Research Topics (`research-topics.html`) is currently hidden from nav — unhide when you have PETs-era content.
- `CONTEXT.md` and `AUDIT_REPORT.md` are maintainer docs, excluded from the published site via `_config.yml`.
