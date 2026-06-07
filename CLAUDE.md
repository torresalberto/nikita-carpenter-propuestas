# Nikita Carpenter — Portfolio Website

**Client:** Nikita (custom woodworker, originally from Moscow, based in Mexico)
**Status:** ✅ Production-ready (SEO + analytics + sitemap + robots wired; awaiting GA4 / GSC ID handoff). Proposals 2 and 3 archived under `archive/` (not deployed).
**Stack:** Single HTML file (inline CSS + JS)
**Repo:** `git`
**Production URL:** `https://nikita.albto.me/`

## Overview
Single-file static portfolio for a custom woodworker. Features an interactive SVG-based isometric "dollhouse" cutaway with 5 rooms and 19 photos. Workshop journal aesthetic. Production-ready with local SEO (Chihuahua), Schema.org JSON-LD (Carpenter / Person / Service / ImageGallery / FAQPage), OpenGraph + Twitter Card, sitemap.xml, robots.txt, and GA4 + Google Search Console hooks (placeholders — see "Pending user actions" below).

## File Structure
```
Nikita_carpenter/
├── index.html              (~1966 lines, the whole site — PRODUCTION)
├── AGENTS.md               (detailed tech notes)
├── DEPLOY.md               (deploy instructions)
├── README.md
├── CLAUDE.md               (this file)
├── sitemap.xml             ← points only to canonical https://nikita.albto.me/
├── robots.txt              ← allows /, disallows /archive/
├── mesa_*.jpeg             (19 photos total)
├── silla_*.jpeg
├── cocina_*.jpeg
├── banco_*.jpeg
├── screenshots/            (visual QA reference)
├── screenshots_v2/
├── screenshots_live/
└── archive/                ← ARCHIVED proposals (NOT deployed; preserved for reference)
    ├── proposal-2-cuaderno/    (noindex meta)
    └── proposal-3-russian-craft/ (noindex meta)
```

## Architecture
- **No build step** — open `index.html` directly
- **Lightbox:** photo cards attach `openLightbox()` via click handler
- **Reveal animations:** `.reveal` class + IntersectionObserver
- **Dollhouse:** pure inline SVG (no CSS 3D), cabinet projection, 2x2 isometric room grid
- **FAQ section:** native `<details>/<summary>` (no JS)
- **WhatsApp tracking:** every `.wa-btn` (photo card) + lightbox WA + nav-cta + faq link + footer link calls `trackWa(source, label)` which emits a GA4 `whatsapp_click` event

## Conventions
- **Spanish content** with Cyrillic `Москва` stamps for narrative
- **No duplicate photos** across rooms (each photo appears in exactly one room — by filename)
- **WhatsApp number:** `5216144579884` — hardcoded in 5 places (nav, footer, faq, const wa, wa.me link)
- **Design language:** Workshop journal — warm wood tones, DM Serif Display + Fraunces + JetBrains Mono
- **Local SEO:** the word "Chihuahua" appears 23 times in visible body text and is the areaServed hub in JSON-LD

## Common Tasks

### Local preview
```bash
python3 -m http.server 7777 -d /home/alb/projects/free-websites/Nikita_carpenter
```
Then open `http://localhost:7777/`.

### Headless screenshot
```bash
google-chrome --headless --disable-gpu --no-sandbox --hide-scrollbars \
  --window-size=1440,900 --virtual-time-budget=8000 \
  --screenshot=out.png http://localhost:7777/
```

### Deploy
Static — any host works.

**Production** is `https://nikita.albto.me/` (a2hosting shared hosting). Deploy runs through the user's "secret action" scripts already in the repo (do not touch those). My workflow is: push to git, the user triggers the deploy.

**Pre-deploy checks (all should pass):**
1. `grep "5216144579884" index.html` → 5 hits
2. `grep "G-XXXXXXXXXX" index.html` → 3 hits (all 3 must be replaced together with the real GA4 ID after the user creates the property)
3. `grep "google-site-verification" index.html` → 1 hit (GSC placeholder, must be replaced with the real token)
4. `grep -oE "src: '[^']+'" index.html | sort | uniq -d` → nothing (no duplicate photo filenames)
5. `grep -c '"@type": "ImageObject"' index.html` → 19
6. `ls sitemap.xml robots.txt` → both present at root
7. `archive/proposal-2-cuaderno/index.html` and `archive/proposal-3-russian-craft/index.html` still carry `<meta name="robots" content="noindex,nofollow">`

## Pending user actions (after my push)
1. Trigger the secret-action deploy to a2hosting.
2. In Google Analytics: create a GA4 property for `nikita.albto.me`, copy the Measurement ID (format `G-XXXXXXXXXX`).
3. In Google Search Console: add the `https://nikita.albto.me/` property, choose "HTML tag" verification, copy the `content="..."` token.
4. Paste the GA4 ID into the 3 placeholder spots in `index.html` and the GSC token into the 1 verification meta. Push again.
5. In GSC → Sitemaps, submit `https://nikita.albto.me/sitemap.xml` for faster indexing.

## Key Files
| File | Purpose |
|------|---------|
| `AGENTS.md` | Detailed architecture, dollhouse geometry, SEO setup, conventions |
| `DEPLOY.md` | Deploy instructions |
| `index.html` | The whole site (~1966 lines) |
| `sitemap.xml` | SEO sitemap (canonical URL only) |
| `robots.txt` | Crawler directives |

## Working Rules
- **Do not** replace the SVG location pin with an emoji — it inherits `currentColor`
- Wrap proper nouns in `<span class="loc-text">` to escape uppercase styling
- Nikita learned carpentry IN Mexico (2019), not Russia — Moscow origin is his story, craft is Mexican
- The repo's "secret action" deploy scripts must NOT be modified or removed
- 3 pairs of photo filenames share the same MD5 (pre-existing data issue) — see AGENTS.md "Pre-existing data hygiene issue" for details. Not blocking production.
