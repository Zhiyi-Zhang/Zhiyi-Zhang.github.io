# Audit Report — Zhiyi-Zhang.github.io

**Date:** 2026-08-27  
**Branch audited:** `master` @ `b9aa208` (after Dependabot rebase — 8 bumps from 2025-2026)  
**Scope:** content freshness, tech stack, SEO, accessibility, performance, security, design/UX, maintenance.

---

## Executive Summary

The site is **functional and recently dependency-patched**, but **content is 2–5 years stale** and the **Jekyll 3.9 / github-pages 231 stack is legacy**. No build is broken, but SEO is under-configured, accessibility has duplicate IDs and empty alt text, and several copy typos persist. Experimental pages (`enigma_*`, `test-fingerprinting.html`, `ca.crt`) are public without robots control. **Recommended:** apply 12 low-risk code fixes now, and queue 8 content/decision fixes requiring your input.

---

## 1. Content Audit

### Critical — public-facing staleness

| Area | Current state | Risk |
|------|---------------|------|
| `index.html` banner bio | “Research Scientist @ META … Ph.D. @ CS UCLA … I recevied my Ph.D. … disssertation title is …” — typos `recevied`/`disssertation`, role still generic “privacy enhancing technologies (PETs)”, Chinese name line correct | Looks unmaintained; typos hurt credibility |
| `index.html` News | Only 1 visible entry: “Serving as PC member for PETS 2024 — June, 2023”. 4 older items commented out (2020–2019). No entries for 2024-2026 | Visitors assume site abandoned |
| `index.html` Teaching | Ends at 2020 (CS35L 2018, CS217 2017-2020, CS118 2020). No Meta mentoring / guest lectures / current talks | Mismatch with “Research Scientist @ Meta” |
| `index.html` Recent Posts | 3 posts, all Mar 2020. No newer blog / tutorial | Dead section |
| `publications.html` | Last entry 2024 (ComST survey). No 2025-2026. Uses hard-coded HTML (13 journal/conf + 4 posters + 4 tech reports). Mix of `BibTex` links — some to `publications/bibtex/*.html` (local), some to `ieeexplore` “BibTex” button that is actually a DOI (confusing). Duplicate `id="content"` on every `<h4>` | Hard to maintain, SEO duplicate-ID warning, mixed BibTeX semantics |
| `talks.html` | Stops at 2019. Only 4 entries (2× NDN-Lite tutorial, 2× NDNCom). No Meta / PETs talks | Gap vs. publications |
| `research-topics.html` | Exists but **hidden from nav** (`navigator.html` line commented). Content: Sovereign (2018), AuditShare, DLedger, FITT — all pre-2020 | Either restore with updated PETs/NDN topics or remove; currently dead weight |
| `profile.html` | `<embed src="content/zz-cv.pdf" …>` only. No HTML fallback, no last-updated date. PDFs: `zz-cv.pdf` 61K, `zz-resume.pdf` 50K, `dissertation.pdf` 5.4M, `prospectus.pdf` 780K — dates unknown (file mtime Nov 27 2024) | If CV is stale, the embed hides it |
| `_config.yml` `description` | `Personal Website of Zhiyi Zhang, A UCLA PhD Candidate working on network security…` — still says **PhD Candidate** in 2026 | Search snippet is wrong |

### Experimental / non-public pages

- `enigma_https.html`, `enigma_turn.html` (TURN `127.0.0.1:5000`, `local.zhiyi-zhang.com:10088`), `test-fingerprinting.html` (13K fingerprint harness), `ca.crt` — all crawled by default (no `robots.txt`, no `noindex`). Useful for research but likely not intended for public indexing.

---

## 2. Technical Stack Audit

| Check | Finding |
|-------|---------|
| Jekyll | `github-pages 231` pins `jekyll 3.9.5` + `kramdown 2.4.0`. Jekyll 3.9 is **EOL** (GitHub Pages legacy). Modern path is Jekyll 4 via GitHub Actions (`actions/deploy-pages`). Works today but no future security/features. |
| `Gemfile` | `gem "jekyll", "~> 3.9"` duplicate pin, `minima ~>2.5` unused (theme is Editorial SASS). Missing `jekyll-seo-tag`, `jekyll-sitemap`. `webrick ~>1.8` added correctly for Ruby 3. |
| Ruby | System Ruby 2.6.10 on host; lockfile uses `bundler 2.3.26`. GitHub Pages builds with Ruby 3.2+. No `ruby-version` / `.ruby-version` nor Actions runtime pinned. |
| `_config.yml` | No `exclude:` (so `CONTEXT.md`, `AUDIT_REPORT.md`, `enigma_*` would be published), no `defaults`, no `sass`, no `plugins` beyond `jekyll-feed`. No `jekyll-seo-tag` / `jekyll-sitemap`. No `permalink` or `timezone`. |
| Build | No `Gemfile.lock` in `exclude` — published. No GitHub Actions workflow — relies on legacy Pages build. No CI lint. |
| `assets/css/main.css` | 3568 lines, **93 KB** compressed? Includes full reset + Editorial. Loaded render-blocking; no `preload` or `media` hint. `assets/sass/main.scss` imports Google Fonts via `@import url()` (render-blocking, no `display=swap`). |
| JS | `jquery.min.js` + `browser.min.js` + `breakpoints.min.js` + `util.js` + `main.js` — all synchronously loaded at `</body>`. Font Awesome via `kit.fontawesome.com/7d40fa654a.js` (requires kit ID — if kit is revoked, icons break; no fallback to CDN). jQuery version is 3.x vintage (check `assets/js/jquery.min.js` header). |
| Domain | `CNAME → zhiyi-zhang.com` + `url: https://zhiyi-zhang.com` correct. `baseurl: ""` correct. No `enforce_ssl` or `future: false`. |

---

## 3. SEO Audit

| Issue | Location | Impact |
|-------|----------|--------|
| No `<meta name="description">` | `_layouts/default.html` has only `<title>{{page.title}}</title>` + viewport | Search snippet falls back to first paragraph; `_config.yml description` not emitted |
| No Open Graph / Twitter cards | same layout | Link previews on Slack/Twitter/LI are bare |
| No `canonical` | layout | Duplicate indexing risk |
| No `sitemap.xml` / `robots.txt` | repo root | Googlebot discovers via crawling only; experimental pages leak |
| No `jekyll-seo-tag` / `jekyll-sitemap` | `_config.yml`/`Gemfile` | Missing structured data, auto sitemap |
| Title tags are opaque | `zhiyi-home`, `zhiyi-publications` | Should be “Zhiyi Zhang — Research Scientist @ Meta — …” |
| Language missing | `<html>` has no `lang` | WCAG + SEO signal |
| Duplicate `id="content"` | `publications.html` (×21), `talks.html` | Invalid HTML, SEO crawler confusion |
| Missing alt text | `index.html` `images/headpic.jpg` `alt=""` | A11y + image SEO |
| PDF indexing | `content/*.pdf` served without `content-type` hints, no HTML alternative | OK but CV not discoverable as text |

---

## 4. Accessibility (WCAG 2.1 AA spot-check)

- `[CRITICAL]` Duplicate `id="content"` — fails 4.1.1 Parsing. Replace with `class="pub-title"` or unique IDs.
- `[HIGH]` Headpic `alt=""` — decorative or not? Should be `alt="Portrait of Zhiyi Zhang"`; similarly `research-topics.html` images have `alt=""`.
- `[MED]` Heading hierarchy: `index.html` uses `<h1>Zhiyi Zhang</h1>` then jumps to `<h2>News</h2>` then `<h4>` for cards (skips h3) — should use `<h3>`.
- `[MED]` Icon links have `<span class="label">` (good) but `<a href="#"` for email — should be `mailto:zhiyi@cs.ucla.edu` with `aria-label`.
- `[MED]` No skip-to-content link; sidebar is keyboard-accessible via Editorial JS but no `aria-current` on active nav.
- `[LOW]` Color contrast in Editorial is AA-passing for body but button `primary` on white needs check.

---

## 5. Performance

- **Images:** `headpic.jpg` 333K (no WebP/AVIF, no `loading="lazy"`, no `width`/`height` → CLS), `fitt.png` 164K, `security.jpg` 100K. No `srcset`. `pic01–pic06` etc. unused.
- **CSS:** `main.css` 3568 lines, no purge; Google Fonts `@import` blocks rendering. Could add `display=swap` + `preconnect`.
- **JS:** 5 sync scripts; no `defer`. jQuery for Editorial off-canvas is overkill — but acceptable.
- **PDFs:** `dissertation.pdf` 5.4M served on every profile view via `<embed>` (forces full download). Better: link + PDF.js or `<object>` with fallback.
- **No compression hints:** GitHub Pages auto-gzips, but no explicit `Cache-Control`.
- **No lazy fingerprint page:** `test-fingerprinting.html` runs heavy JS on load (acceptable for a test page).

---

## 6. Security

- Dependencies now **patched** (2026-08-27 rebase brought in all 8 Dependabot bumps). Good.
- `_layouts/default.html` loads `kit.fontawesome.com/7d40fa654a.js` without `integrity`/`crossorigin` — if kit is compromised, XSS. Recommend adding `crossorigin="anonymous"` (already present) + SRI or switching to `cdnjs` with SRI.
- `enigma_https.html` fetches `http://local.zhiyi-zhang.com:10088` (mixed HTTP) — browser will block on HTTPS site; keep as HTTP-only test or add `//` note.
- No `Content-Security-Policy` meta (not critical for Pages, but could add `upgrade-insecure-requests`).
- No `rel="noopener"` on external links (`_includes/header.html`, `publications.html` Read buttons) — tabnabbing.
- `ca.crt` (1.3K) public — ensure it’s not a private CA; if test-only, move to `content/` or exclude from sitemap.

---

## 7. Design / UX

- Editorial theme is clean and responsive (breakpoints 360–1680). Sidebar toggles correctly on mobile via `main.js`.
- **Navigation gap:** Research Topics exists but hidden — either unhide (and modernize) or delete to avoid confusion. Talks vs. Publications split is fine.
- **News card layout:** Only 1 card → empty `posts` grid looks sparse; commented-out cards leave dead HTML. Better to either populate 2024-2026 or hide section until updated.
- **No last-updated / edit timestamp** — readers can’t tell freshness.
- **No search** (commented out in navigator). Could enable `jekyll-search` or client-side Lunr if posts grow.
- **No dark mode** — not needed for academic site, low priority.

---

## 8. Maintenance

- `README.md` (486 bytes) only has `bundle exec jekyll serve` + Cloudflare/GoDaddy notes — no deploy, no structure, no contribution guide.
- No GitHub Actions workflow — future GitHub Pages will require Actions (legacy build deprecated 2024-2025).
- No `LICENSE` check — file exists (11K) but verify CCA 3.0 attribution for Editorial.
- No `.editorconfig`, no linter, no `bundle update` cadence beyond Dependabot.

---

## 9. Proposed Fixes (grouped by risk)

### A. Low-risk code fixes (I can apply now — no content decisions needed)

1. **Typos + description:** `index.html` `recevied→received`, `disssertation→dissertation`; `_config.yml` description → `Research Scientist @ Meta … Ph.D. 2021 UCLA, … Named Data Networking and PETs` + `email`, `url` polish, `exclude:` list, `plugins: [jekyll-feed, jekyll-seo-tag, jekyll-sitemap]`, `timezone`, `markdown` options.
2. **Layout SEO/a11y:** `_layouts/default.html` → add `lang="en"`, `<meta name="description" …>`, `{% seo %}` + JSON-LD fallback, `canonical`, `preconnect` for Google Fonts + `display=swap`, `rel="noopener"` + `aria-label` on icon links, `alt` fixes, skip-link, `defer` on JS, SRI note for Font Awesome.
3. **Gemfile/plugins:** add `gem "jekyll-seo-tag"`, `gem "jekyll-sitemap"` to `jekyll_plugins` group.
4. **Accessibility:** `publications.html` + `talks.html` replace `id="content"` with `class="pub-title"`; `index.html` `h4→h3`; `images/headpic.jpg` alt text; email `href="mailto:…"`.
5. **Security/UX:** add `rel="noopener noreferrer"` to every external `Read`/`BibTex` button; add `loading="lazy" width/height` to images.
6. **Site hygiene:** add `robots.txt` (allow `/`, disallow `/enigma*`, `/test-fingerprinting.html`, `/*.crt`), add `404.html` SEO, update `_config.yml` `exclude: [Gemfile, Gemfile.lock, vendor, node_modules, .sass-cache, .jekyll-cache, README.md, CONTEXT.md, AUDIT_REPORT.md, CNAME, Gemfile.lock — keep CNAME]`, actually keep `CNAME` included.
7. **Publication button semantics:** fix “BibTex → BibTeX” button linking to DOI when no bib file (e.g., ComST 2024) — change label to `DOI` or add real bib file stub.

### B. Medium-risk (needs your choice)

8. **News / Teaching / Recent Posts freshness:** keep or hide? Options: (a) populate with 2024-2026 entries (you provide), (b) hide News/Teaching until updated (comment out section + nav), (c) auto-generate from data file.
9. **Research Topics nav:** unhide and rewrite for PETs @ Meta era, or delete `research-topics.html` + remove image assets.
10. **Experimental pages:** keep public but `noindex` + `robots.txt` disallow, or move to `/_experiments/` and exclude.

### C. Higher effort (future sprint)

11. **Migrate Jekyll 3.9 → 4 + GitHub Actions deploy-pages** (new workflow, Ruby 3.2, purge Editorial build).
12. **Image optimization** (WebP, `srcset`, compress `headpic.jpg` 333K → ~80K, `fitt.png` 164K → WebP).
13. **CV refresh** — replace `content/zz-cv.pdf` + add HTML CV page for indexing.

---

## 10. What I would edit (exact file list)

- `_config.yml` (description, plugins, exclude, seo defaults)
- `Gemfile` (+ seo-tag + sitemap)
- `_layouts/default.html` (lang, meta, seo, preconnect, canonical, defer)
- `_includes/header.html` + `_includes/navigator.html` (noopener, aria, mailto)
- `index.html` (typos, heading levels, alt, lazy)
- `publications.html` + `talks.html` (duplicate IDs → classes)
- `robots.txt` (new)
- `README.md` (expand)

All are **review-by-eye** changes — no data invention. Content additions (News, Talks, Publications 2025-26) require your text.

---

**Next step:** tell me which bucket to apply:

- **“Apply A (low-risk) now”** → I’ll patch the 7 files above in one pass and run `bundle exec jekyll build` (via escalated sandbox) to verify.
- **“Apply A + B”** → specify News/Teaching/Research-Topics preference (keep/hide/rewrite) and whether to noindex `enigma_*`.
- **“Report only”** → I’ll leave the repo as-is and you can cherry-pick.

What’s your call?

