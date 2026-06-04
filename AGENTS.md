# Nikita Carpenter — Portfolio Website

## Status: ✅ COMPLETE & DELIVERY-READY
All polish phases finished. No placeholder content, no duplicate photos, real WhatsApp number wired up. Dollhouse is a proper Sims-style isometric cutaway. Last reviewed: 2026-06-04.

---

## Architecture
- **Single file**: `index.html` (~1646 lines, inline CSS + JS). No framework, no build step.
- **19 JPEG photos** in project root, referenced by filename from the `rooms` JS object.
- **5 room categories**: `sala`, `comedor`, `oficina`, `exterior`, `especial` — hardcoded in `rooms` object.
- **Lightbox**: photo cards in the room overlay attach `openLightbox()` via `card.querySelector('img').addEventListener('click', …)`. If the lightbox won't open, check this event binding is not blocked by overlay z-index or a missing `lightbox` class toggle.
- **Reveal animations**: most sections use the `.reveal` class with an `IntersectionObserver`. Hero text uses `@keyframes rise`. Both start at `opacity: 0` and only animate when in viewport.
- **Dollhouse**: pure inline SVG (no CSS 3D transforms). 2x2 isometric room grid, viewBox `0 0 740 510`, rooms 80×80 world units, walls 80 tall. Cabinet projection (no real perspective). Rooms in diamond: Sala (front-left), Especial (front-right), Comedor (back-left), Oficina (back-right). Exterior is the lawn rhombus around the house. See "Dollhouse Geometry" below.

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
  - Also hardcoded in the nav link and the footer CTA.
  - Two more links (`window.open(...)` and `lightboxWa.href`) build dynamically from `wa` — no manual change needed.
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

---

## Conventions
- **Spanish content**: hero quote, labels, room names, photo names. Hero stamps include Cyrillic `Москва` for narrative effect.
- **Narrative**: Nikita learned carpentry IN Mexico (2019), not Russia. Moscow origin = his story, craft = Mexican.
- **Design language**: workshop journal aesthetic — warm wood tones, DM Serif Display + Fraunces + JetBrains Mono fonts, amber/gold accents, monospace labels, SVG-based isometric dollhouse (Sims-style cutaway, no roof).
- **No duplicate photos across rooms** (each photo appears in exactly one room).
- **Footer pin** is an inline SVG (Material-style location pin) — do not replace with an emoji; the SVG inherits the `cream-dim` color via `currentColor` and matches the JetBrains Mono aesthetic.
- **Proper nouns** like "México" in the footer are wrapped in `<span class="loc-text">` so the parent's `text-transform: uppercase` does not capitalize them.

---

## File Map
```
/home/alb/projects/free-websites/Nikita_carpenter/
├── index.html                       (1646 lines, the whole site)
├── AGENTS.md                        (this file)
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
├── mesa_raw_slabs.jpeg              ← NEW (Especial: raw walnut slabs)
├── mesa_round_epi.jpeg              ← NEW (Sala: round coffee table with epoxy)
├── mesa_sala.jpeg
├── mesa_sillas.jpeg
├── silla_asiento.jpeg               ← NEW (Especial: chair seat close-up)
├── silla_completa.jpeg              ← NEW (Especial: completed chair)
├── screenshots/
└── screenshots_v2/
```

---

## Deploy
The site is fully static — no build step, no backend. Any static host works:
- **Netlify**: drag-drop the folder, or `netlify deploy --dir=. --prod`.
- **Vercel**: `vercel --prod` in the folder.
- **GitHub Pages**: push to `gh-pages` branch, enable Pages in repo settings.
- **Plain FTP**: upload `index.html` + the 19 JPEGs to web root.

Before deploying, sanity-check:
1. `grep "5216144579884" index.html` returns 3 hits (nav, footer, `const wa`).
2. `grep "52XXXXXXXXXX" index.html` returns 0 hits.
3. `grep -oE "src: '[^']+'" index.html | sort | uniq -d` returns nothing (no duplicate photos).
4. All 19 JPEGs are present in the deployed directory.

