# Nikita Carpenter — Proposal 03 · РУССКОЕ РЕМЕСЛО (Russian Craft)

## Status: 🚧 CLONE OF PROPOSAL 01 (awaiting design direction)
All 4 polish phases finished. No placeholder content, no duplicate photos, real WhatsApp number wired up. Last reviewed: 2026-06-01.

---

## Architecture
- **Single file**: `index.html` (~1444 lines, inline CSS + JS). No framework, no build step.
- **13 JPEG photos** in project root, referenced by filename from the `rooms` JS object.
- **5 room categories**: `sala`, `comedor`, `oficina`, `exterior`, `especial` — hardcoded in `rooms` object.
- **Lightbox**: photo cards in the room overlay attach `openLightbox()` via `card.querySelector('img').addEventListener('click', …)`. If the lightbox won't open, check this event binding is not blocked by overlay z-index or a missing `lightbox` class toggle.
- **Reveal animations**: most sections use the `.reveal` class with an `IntersectionObserver`. Hero text uses `@keyframes rise`. Both start at `opacity: 0` and only animate when in viewport.

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
- **`const wa` (line ~1269)**: real WhatsApp number `5216144579884` (digits only, no `+`).
  - Also hardcoded in the nav link (line ~1065) and the footer CTA (line ~1210).
  - Two more links (`window.open(...)` and `lightboxWa.href`) build dynamically from `wa` — no manual change needed.
- **Server PID tracking**: none; the `python3 -m http.server` process is foreground by default. Use `pkill -f 'http.server 7777'` to stop.

---

## Photo → Room Map (current, verified unique)

| Room     | Photos (count) | Names |
|----------|----------------|-------|
| `sala`     | 2 | Mesa de Centro — Forma Orgánica · Mesa Redonda — Pieza de Autor |
| `comedor`  | 4 | Mesa Familiar — Base X · Mesa Familiar — Con Sillas · Mesa Mediana — Relleno de Resina · **Mesa de Comedor — Madera Maciza** |
| `oficina`  | 3 | Escritorio Ejecutivo · Mesa de Oficina — Base T · Mesa Compacta |
| `exterior` | 3 | Mesa de Jardín · Mesa de Patio · Banco de Jardín — Para el Atardecer |
| `especial` | 4 | Mesa River — Resina Azul · Mesa Raw — Madera en Bruto · Silla Artesanal — Detalle del Asiento · Silla de Madera — Pieza de Autor |
| **Total**  | **16** | matches the `16 PIEZAS` stat in the house-scale section |

**Rule enforced:** no photo appears in more than one room. Every filename in `rooms.photos[].src` is unique across the whole object (verified by `grep -oE "src: '[^']+'" index.html | sort -u | wc -l` → 13).

---

## Conventions
- **Spanish content**: hero quote, labels, room names, photo names. Hero stamps include Cyrillic `Москва` for narrative effect.
- **Narrative**: Nikita learned carpentry IN Mexico (2019), not Russia. Moscow origin = his story, craft = Mexican.
- **Design language**: workshop journal aesthetic — warm wood tones, DM Serif Display + Fraunces + JetBrains Mono fonts, amber/gold accents, monospace labels, 3D isometric house with CSS transforms.
- **No duplicate photos across rooms** (each photo appears in exactly one room).
- **Footer pin** is an inline SVG (Material-style location pin) — do not replace with an emoji; the SVG inherits the `cream-dim` color via `currentColor` and matches the JetBrains Mono aesthetic.
- **Proper nouns** like "México" in the footer are wrapped in `<span class="loc-text">` so the parent's `text-transform: uppercase` does not capitalize them.

---

## File Map
```
/home/alb/projects/free-websites/Nikita_carpenter/
├── index.html                       (1377 lines, the whole site)
├── AGENTS.md                        (this file)
├── banco_jardin.jpeg                ← NEW (Exterior: outdoor wooden bench)
├── mesa_chica.jpeg
├── mesa_exterior.jpeg
├── mesa_irregular.jpeg
├── mesa_jardin.jpeg
├── mesa_madera.jpeg                 ← now used in `comedor` (was previously unused)
├── mesa_madera_sala_chica.jpeg
├── mesa_mediana.jpeg
├── mesa_negro.jpeg
├── mesa_ofi.jpeg
├── mesa_oficina.jpeg
├── mesa_raw.jpeg
├── mesa_sala.jpeg
├── mesa_sillas.jpeg
├── silla_asiento.jpeg               ← NEW (Especial: chair seat close-up)
├── silla_completa.jpeg              ← NEW (Especial: completed chair)
├── screenshots/                     (original reference renders)
└── screenshots_v2/                  (post-polish verification renders)
    ├── 01-hero.png
    └── 02-house-stats.png
```

---

## Deploy
The site is fully static — no build step, no backend. Any static host works:
- **Netlify**: drag-drop the folder, or `netlify deploy --dir=. --prod`.
- **Vercel**: `vercel --prod` in the folder.
- **GitHub Pages**: push to `gh-pages` branch, enable Pages in repo settings.
- **Plain FTP**: upload `index.html` + the 13 JPEGs to web root.

Before deploying, sanity-check:
1. `grep "5216144579884" index.html` returns 3 hits (nav, footer, `const wa`).
2. `grep "52XXXXXXXXXX" index.html` returns 0 hits.
3. `grep -oE "src: '[^']+'" index.html | sort | uniq -d` returns nothing (no duplicate photos).
4. All 13 JPEGs are present in the deployed directory.

