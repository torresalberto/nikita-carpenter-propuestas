# Nikita Carpenter — Portfolio Website

**Client:** Nikita (custom woodworker, originally from Moscow, based in Mexico)
**Status:** ✅ Complete & delivery-ready
**Stack:** Single HTML file (inline CSS + JS)
**Repo:** `git`

## Overview
Single-file static portfolio for a custom woodworker. Features an interactive SVG-based isometric "dollhouse" cutaway with 5 rooms and 19 photos. Workshop journal aesthetic.

## File Structure
```
Nikita_carpenter/
├── index.html              (1646 lines, the whole site)
├── AGENTS.md               (detailed tech notes)
├── DEPLOY.md               (deploy instructions)
├── README.md
├── mesa_*.jpeg             (19 photos total)
├── silla_*.jpeg
├── cocina_*.jpeg
├── banco_*.jpeg
├── proposal-2-cuaderno/    (proposal materials)
├── proposal-3-russian-craft/
├── screenshots/            (visual QA reference)
├── screenshots_v2/
└── screenshots_live/
```

## Architecture
- **No build step** — open `index.html` directly
- **Lightbox:** photo cards attach `openLightbox()` via click handler
- **Reveal animations:** `.reveal` class + IntersectionObserver
- **Dollhouse:** pure inline SVG (no CSS 3D), cabinet projection, 2x2 isometric room grid

## Conventions
- **Spanish content** with Cyrillic `Москва` stamps for narrative
- **No duplicate photos** across rooms (each photo appears in exactly one room)
- **WhatsApp number:** `5216144579884` — hardcoded in 3 places (nav, footer, `const wa`)
- **Design language:** Workshop journal — warm wood tones, DM Serif Display + Fraunces + JetBrains Mono

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
Static — any host works (Netlify, Vercel, GitHub Pages, plain FTP).

**Pre-deploy checks:**
1. `grep "5216144579884" index.html` → 3 hits
2. `grep "52XXXXXXXXXX" index.html` → 0 hits
3. `grep -oE "src: '[^']+'" index.html | sort | uniq -d` → nothing

## Key Files
| File | Purpose |
|------|---------|
| `AGENTS.md` | Detailed architecture, dollhouse geometry, conventions |
| `DEPLOY.md` | Deploy instructions |
| `index.html` | The whole site (1646 lines) |

## Working Rules
- **Do not** replace the SVG location pin with an emoji — it inherits `currentColor`
- Wrap proper nouns in `<span class="loc-text">` to escape uppercase styling
- Nikita learned carpentry IN Mexico (2019), not Russia — Moscow origin is his story, craft is Mexican
