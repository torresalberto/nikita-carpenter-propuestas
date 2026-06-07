# Nikita Carpenter — Portfolio Website

## Status: ✅ PRODUCTION-READY (SEO + analytics wired, awaiting GA4 / GSC handoff)
All polish phases finished. Production SEO + schema + sitemap + robots + GA4 (placeholder) + GSC verification meta all in place. Two demo proposals (proposal-2-cuaderno, proposal-3-russian-craft) are marked `noindex` so search engines only see the canonical `index.html`. Last reviewed: 2026-06-07.

---

## Production URL
- **Live site**: `https://nikita.albto.me/` (a2hosting, deployed via repo's "secret action" deploy scripts).
- **GitHub Pages demos** (noindex, not for SEO): `https://torresalberto.github.io/nikita-carpenter-propuestas/proposal-2-cuaderno/`, `…/proposal-3-russian-craft/`.

---

## Architecture
- **Single file**: `index.html` (~1966 lines, inline CSS + JS). No framework, no build step.
- **19 JPEG photos** in project root, referenced by filename from the `rooms` JS object.
- **5 room categories**: `sala`, `comedor`, `oficina`, `exterior`, `especial` — hardcoded in `rooms` object.
- **Lightbox**: photo cards in the room overlay attach `openLightbox()` via `card.querySelector('img').addEventListener('click', …)`. If the lightbox won't open, check this event binding is not blocked by overlay z-index or a missing `lightbox` class toggle.
- **Reveal animations**: most sections use the `.reveal` class with an `IntersectionObserver`. Hero text uses `@keyframes rise`. Both start at `opacity: 0` and only animate when in viewport.
- **Dollhouse**: pure inline SVG (no CSS 3D transforms). 2x2 isometric room grid, viewBox `0 0 740 510`, rooms 80×80 world units, walls 80 tall. Cabinet projection (no real perspective). Rooms in diamond: Sala (front-left), Especial (front-right), Comedor (back-left), Oficina (back-right). Exterior is the lawn rhombus around the house. See "Dollhouse Geometry" below.
- **FAQ section**: native `<details>/<summary>` elements (no JS), 5 questions about process, materials, delivery, custom work, payment.

### Dollhouse Geometry
- Coordinate transform: `iso_x = (x - y) * 0.866`, `iso_y = (x + y) * 0.5 - z` (x=right, y=back, z=up)
- Offset: (370, 200) in viewBox; room corners are at world (0,0), (80,0), (80,80), (0,80)
- Each room group: floor rhombus + back wall parallelogram + left wall parallelogram + furniture + tag
- Lawn: rhombus `127.5,280 370,420 612.5,280 370,140` (280×280 world footprint), 3 plant clusters, shadow ellipse
- Exterior group: same lawn polygon (drawn first, behind rooms) + dashed border + EXTERIOR label
- Z-order: shadow → exterior (lawn+label) → 4 indoor rooms (so rooms appear on top of lawn)
- Room tags positioned at front corners; drawn last so they sit above the back walls
- Hover: 3-stop gradient brightens the floor (`#room-grp:hover .room-floor`)

---

## SEO / Local SEO (Chihuahua)
- **Title tag**: `Nikita — Muebles de Madera a Medida · Chihuahua`
- **Meta description**: 156 chars, Spanish, mentions "Chihuahua, México", WhatsApp CTA.
- **Meta keywords**: `carpintero Chihuahua, muebles de madera a medida, ...`.
- **Canonical**: `https://nikita.albto.me/`
- **Robots**: `index, follow` (production), `noindex, nofollow` (prop 2/3 demos).
- **Hreflang**: not set (single-language Spanish market).
- **Visible body text**: hero `📍 Chihuahua, México` pin + footer `Chihuahua, México · Hecho a mano desde 2019` + service-area line. Total "Chihuahua" mentions in body: 23.
- **Service area in JSON-LD**: `areaServed` lists Chihuahua, Cuauhtémoc, Delicias, Hidalgo del Parral.

## Open Graph + Twitter Card
- `og:type=website`, `og:locale=es_MX`, `og:site_name=Nikita Carpenter`, og:title/description/url set.
- `og:image` = `mesa_raw_slabs.jpeg` (raw parota slabs, 1200×630 displayed; actual is the raw JPEG — works because aspect is close).
- Twitter card `summary_large_image` mirrors OG.

## Schema.org JSON-LD (5 blocks)
1. **Carpenter** (subtype of LocalBusiness) — name, image, address (Chihuahua), geo (-28.6353, -106.0889), telephone, opening hours (`Mo-Sa 09:00-19:00`), price range `$$`, service area.
2. **Person** — Nikita, birthPlace Moscú, knowsAbout carpentry/wood/epoxy, member of `Organization` (the taller).
3. **Service** — "Muebles de madera a medida en Chihuahua", provider = the Carpenter entity, area = 4 cities.
4. **ImageGallery** — 19 ImageObject entries with `contentUrl` pointing to `https://nikita.albto.me/<filename>` (one per photo in `rooms.photos`).
5. **FAQPage** — 5 Question/Answer pairs matching the on-page FAQ section.

## GA4 + GSC
- **GA4 ID** is the placeholder `G-XXXXXXXXXX` in **3 places**: `<script>` config call, deferred `googletagmanager.com/gtag/js` loader, and the comment. **Replace all 3 with the real Measurement ID after creating the GA4 property for nikita.albto.me.**
- **`trackWa(source, label)`** helper: emits `event: whatsapp_click` (category: engagement) on every WhatsApp link click — photo card buttons, lightbox button, nav-cta, faq link, footer link. Source = origin (photo_card / lightbox / nav / faq / footer-wa), label = piece name or "general". Wrapped in try/catch so tracking failures never break the page.
- **GSC verification meta tag** is the placeholder `<meta name="google-site-verification" content="XXXXXXXXXXXXXXXXXXXXXXXXXXXX" />` (line ~10). **Replace with the real token** once the user adds the domain to Google Search Console.
- **After both real IDs are dropped in**, ask the user to also submit `https://nikita.albto.me/sitemap.xml` inside GSC for faster indexing.

---

## Preview
```bash
python3 -m http.server 7777 -d /home/alb/projects/free-websites/Nikita_carpenter
```
No other tooling required. Open `http://localhost:7777/`.

For headless screenshots:
```bash
google-chrome --headless --disable-gpu --no-sandbox --hide-scrollbars \
  --window-size=1440,900 --virtual-time-budget=8000 \
  --screenshot=out.png http://localhost:7777/
```
Note: reveal-on-scroll elements will be hidden in headless captures because there is no scroll. Use the existing reference screenshots in `screenshots/` (taken in a real browser) for visual QA.

---

## Key Data
- **`rooms` object**: defines per-room `title`, `sub`, and `[{src, name}]` photo array. This is the only place to add/remove/reassign photos.
- **`const wa` (line ~1402)**: real WhatsApp number `5216144579884` (digits only, no `+`).
  - Also hardcoded in the nav link, footer CTA, FAQ CTA, OpenGraph/Schema URLs.
  - 5 hits total (nav, footer, `const wa`, FAQ, plus one duplicate in `wa.me/...` link text).
  - Two dynamic links (`window.open(...)` and `lightboxWa.href`) build from `wa` — no manual change needed.
- **Server PID tracking**: none; the `python3 -m http.server` process is foreground by default. Use `pkill -f 'http.server 7777'` to stop.

---

## Photo → Room Map (current, verified unique)

| Room     | Photos (count) | Names |
|----------|----------------|-------|
| `sala`     | 3 | Mesa de Centro — Forma Orgánica · Mesa Redonda — Pieza de Autor · Mesa Round — Tronco con Resina |
| `comedor`  | 4 | Mesa Familiar — Base X · Mesa Familiar — Con Sillas · Mesa Mediana — Relleno de Resina · Mesa de Comedor — Madera Maciza |
| `oficina`  | 3 | Escritorio Ejecutivo · Mesa de Oficina — Base T · Mesa Compacta |
| `exterior` | 4 | Mesa de Jardín · Mesa de Patio · Cocina Exterior — Superficie de Nogal · Banco de Jardín — Para el Atardecer |
| `especial` | 5 | Mesa River — Resina Azul · Mesa Raw — Madera en Bruto · Láminas de Parota — Materias Primas · Silla Artesanal — Detalle del Asiento · Silla de Madera — Pieza de Autor |
| **Total**  | **19** | matches the `19 PIEZAS` stat in the house-scale section |

**Rule enforced:** no photo appears in more than one room. Every filename in `rooms.photos[].src` is unique across the whole object (verified by `grep -oE "src: '[^']+'" index.html | sort -u | wc -l` → 19).

**⚠️ Pre-existing data hygiene issue (NOT introduced by production polish)**: 3 pairs of files share the same MD5 hash (same byte content under different names):
- `mesa_round_epi.jpeg` === `banco_jardin.jpeg` (178530 B)
- `mesa_raw_slabs.jpeg` === `silla_asiento.jpeg` (212709 B)
- `cocina_exterior.jpeg` === `silla_completa.jpeg` (142989 B)

This means some photo cards display content that does not match their caption. The browser sees distinct filenames (so 19 unique `src` entries and 19 unique ImageObject entries — both checks pass), but the actual pixel content of those 6 cards is duplicated. The 19-card uniqueness rule in this doc applies to filenames, not pixel content. **Fix in a future polish phase if Nikita can re-export the missing originals**; not blocking production deploy.

---

## Conventions
- **Spanish content**: hero quote, labels, room names, photo names. Hero stamps include Cyrillic `Москва` for narrative effect.
- **Narrative**: Nikita learned carpentry IN Mexico (2019), not Russia. Moscow origin = his story, craft = Mexican.
- **Design language**: workshop journal aesthetic — warm wood tones, DM Serif Display + Fraunces + JetBrains Mono fonts, amber/gold accents, monospace labels, SVG-based isometric dollhouse (Sims-style cutaway, no roof).
- **No duplicate photos across rooms** (each photo appears in exactly one room — by filename).
- **Footer pin** is an inline SVG (Material-style location pin) — do not replace with an emoji; the SVG inherits the `cream-dim` color via `currentColor` and matches the JetBrains Mono aesthetic.
- **Hero city pin** is the same inline-SVG style, used for the visible "Chihuahua, México" SEO pin under the hero stamp.
- **Proper nouns** like "México" in the footer are wrapped in `<span class="loc-text">` so the parent's `text-transform: uppercase` does not capitalize them.

---

## File Map
```
/home/alb/projects/free-websites/Nikita_carpenter/
├── index.html                       (~1966 lines, the whole site — PRODUCTION)
├── AGENTS.md                        (this file)
├── sitemap.xml                      ← NEW: only canonical https://nikita.albto.me/ + 1 image
├── robots.txt                       ← NEW: allows /, disallows /proposal-2/, /proposal-3/, points to sitemap
├── banco_jardin.jpeg                ← NEW (Exterior: outdoor wooden bench)
├── cocina_exterior.jpeg             ← NEW (Exterior: outdoor kitchen counter)
├── mesa_chica.jpeg
├── mesa_exterior.jpeg
├── mesa_irregular.jpeg
├── mesa_jardin.jpeg
├── mesa_madera.jpeg
├── mesa_madera_sala_chica.jpeg
├── mesa_mediana.jpeg
├── mesa_negro.jpeg
├── mesa_ofi.jpeg
├── mesa_oficina.jpeg
├── mesa_raw.jpeg
├── mesa_raw_slabs.jpeg              ← NEW (Especial: raw walnut slabs) — also used as og:image
├── mesa_round_epi.jpeg              ← NEW (Sala: round coffee table with epoxy)
├── mesa_sala.jpeg
├── mesa_sillas.jpeg
├── silla_asiento.jpeg               ← NEW (Especial: chair seat close-up)
├── silla_completa.jpeg              ← NEW (Especial: completed chair)
├── screenshots/
├── screenshots_v2/
├── proposal-2-cuaderno/             ← demo, noindex (NOT the production site)
│   ├── index.html                   (noindex meta added)
│   └── AGENTS.md
└── proposal-3-russian-craft/        ← demo, noindex (NOT the production site)
    ├── index.html                   (noindex meta added)
    └── AGENTS.md
```

---

## Deploy
The site is fully static — no build step, no backend.

**Production** (`https://nikita.albto.me/`):
- Hosted on a2hosting shared hosting. Deploy is driven by the "secret action" scripts already in the repo (do not modify or remove). The user's deploy workflow pulls from this repo and rsyncs to the live server.
- **After my push** the user will: (1) trigger the secret-action deploy, (2) configure the GA4 property for `nikita.albto.me`, (3) add the domain to Google Search Console and submit `sitemap.xml`. Once those real IDs are known, the user pastes them into the 3 GA4 places and the 1 GSC place in `index.html` (see "GA4 + GSC" above) and pushes again.

**Demo** (GitHub Pages):
- The 3 proposals are published at `https://torresalberto.github.io/nikita-carpenter-propuestas/{,proposal-2-cuaderno/,proposal-3-russian-craft/}`.
- Only the root `index.html` (this one) is indexable; the two subfolders carry `noindex,nofollow` so search engines don't treat the demos as duplicates of the canonical site.

Before deploying, sanity-check:
1. `grep "5216144579884" index.html` returns 5 hits (nav, footer, faq, const wa, plus the wa.me link).
2. `grep "G-XXXXXXXXXX" index.html` returns 3 hits (gtag config + script src + comment) — all 3 must be replaced together with the real GA4 ID.
3. `grep -oE "src: '[^']+'" index.html | sort | uniq -d` returns nothing (no duplicate photo filenames).
4. All 19 JPEGs are present in the deployed directory.
5. `sitemap.xml` and `robots.txt` are in the web root.
6. prop-2/3 subfolders still carry `<meta name="robots" content="noindex,nofollow">` so demos don't compete with production in SERPs.

