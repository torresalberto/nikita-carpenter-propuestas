# Nikita Carpenter — Portfolio Website (Cuaderno de Taller)

## Status: ✅ COMPLETE & DELIVERY-READY
Cuaderno de Taller (workshop sketchbook) variant. All 4 polish phases finished. No placeholder content, no duplicate photos, real WhatsApp number wired up. Last reviewed: 2026-06-03.

---

## Architecture
- **Single file**: `index.html` (~1385 lines, inline CSS + JS). No framework, no build step.
- **15 JPEG photos** in this directory, referenced by filename from inline `.photo-card` divs.
- **5 room categories**: `sala`, `comedor`, `oficina`, `exterior`, `especial` — hardcoded in HTML.
- **NO JS `rooms` object** in this variant (unlike the workshop journal proposal). All photos are static `.photo-card` divs in HTML with `data-photo-src` and `data-photo-name` attributes. Lightbox reads these attributes on click.
- **Lightbox**: opens on photo card click; close button + WhatsApp "ME INTERESA" button.
- **Reveal animations**: most sections use the `.reveal` class with an `IntersectionObserver`. Both start at `opacity: 0` and only animate when in viewport.

---

## Preview
```bash
python3 -m http.server 7777 -d /home/alb/projects/free-websites/Nikita_carpenter/proposal-2-cuaderno
```
Open `http://localhost:7777/`.

---

## Key Data
- **WhatsApp number**: real `5216144579884` (digits only, no `+`).
- **Server PID tracking**: none; the `python3 -m http.server` process is foreground by default. Use `pkill -f 'http.server 7777'` to stop.

---

## Photo → Room Map (current, verified unique)

| Room     | Photos (count) | Names |
|----------|----------------|-------|
| `sala`     | 3 | Mesa de Centro — Forma Orgánica · Mesa Redonda — Pieza de Autor · Mesa Round — Tronco con Resina |
| `comedor`  | 4 | Mesa Familiar — Base X · Mesa Familiar — Con Sillas · Mesa Mediana — Relleno de Resina · Mesa de Comedor — Madera Maciza |
| `oficina`  | 3 | Escritorio Ejecutivo · Mesa de Oficina — Base T · Mesa Compacta |
| `exterior` | 3 | Mesa de Jardín · Mesa de Patio · Banco de Jardín — Para el Atardecer |
| `especial` | 5 | Mesa River — Resina Azul · Mesa Raw — Madera en Bruto · Láminas de Nogal en Bruto · Silla Artesanal — Detalle del Asiento · Silla de Madera — Pieza de Autor |
| **Total**  | **18** | matches the `18 PIEZAS` stat in the house-scale section |

**Rule enforced:** no photo appears in more than one room.

---

## Conventions
- **Spanish content**: hero quote, labels, room names, photo names. Cyrillic and decorative Spanish copy throughout for "taller/cuaderno" feel.
- **Design language**: Cuaderno de Taller (workshop sketchbook) — paper/sanguine palette, Caveat + Patrick Hand handwritten fonts, polaroid/photo-tape aesthetic, scroll animations with `@keyframes rise`.
- **No duplicate photos across rooms** (each photo appears in exactly one room).
- **Photo cards** use `data-photo-src` and `data-photo-name` attributes — lightbox and WhatsApp button read from these.

---

## File Map
```
/home/alb/projects/free-websites/Nikita_carpenter/proposal-2-cuaderno/
├── index.html                       (~1385 lines, the whole site)
├── AGENTS.md                        (this file)
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
└── banco_jardin.jpeg                ← NEW (Exterior: outdoor wooden bench)
```

---

## Deploy
The site is fully static — no build step, no backend. Currently deployed via GitHub Pages at:
`https://torresalberto.github.io/nikita-carpenter-propuestas/proposal-2-cuaderno/`

Before deploying, sanity-check:
1. `grep "5216144579884" index.html` returns the expected number of hits.
2. `grep -oE 'data-photo-src="[^"]+"' index.html | sort | uniq -d` returns nothing (no duplicate photos).
3. All 15 JPEGs are present in the deployed directory.
