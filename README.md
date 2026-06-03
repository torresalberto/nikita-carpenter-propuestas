# Nikita Carpenter — 3 Propuestas de Diseño Web

Tres propuestas de sitio web para Nikita Carpenter, el carpintero de CDMX. Cada una es **completamente estática, autocontenida e independiente** — puede desplegarse sola o junto con las otras.

---

## Las 3 Propuestas

| # | Carpeta | Concepto | URL Local (preview) |
|---|---------|----------|---------------------|
| 01 | `/` (raíz) | **Diario de Taller** — estética de cuaderno artesanal, tonos cálidos de madera, tipografía editorial, casa 3D isométrica | `http://localhost:7777/` |
| 02 | `/proposal-2-cuaderno/` | **Cuaderno de Taller (Handmade Sketchbook) — see design notes in each subfolder. 
| 03 | `/proposal-3-russian-craft/` | **Russian Craft** — *(celebración de la herencia rusa + oficio manual, con tipografía cirílica, paleta inspirada en iconografía ortodoxa, motivos Khokhloma o Gzhel)* | `http://localhost:7777/proposal-3-russian-craft/` |

> Las propuestas 02 y 03 son **clones** de la propuesta 01 por ahora. Cada una tiene un **badge distintivo en la parte superior** que dice `02 PROPUESTA · CUADERNO DE TALLER` o `03 PROPUESTA · РУССКОЕ РЕМЕСЛО`, para que el cliente pueda identificarlas al verlas lado a lado. Cuando definas el diseño de cada una, edita el `index.html` correspondiente y borra la sección del badge.

---

## Estructura del Repositorio

```
Nikita_carpenter/
├── index.html                       ← Propuesta 01 (Diario de Taller)
├── mesa_*.jpeg                      ← 13 fotos compartidas
├── AGENTS.md                        ← Documentación de la propuesta 01
├── proposal-2-cuaderno/           ← Propuesta 02
│   ├── index.html
│   ├── AGENTS.md
│   └── mesa_*.jpeg
├── proposal-3-russian-craft/            ← Propuesta 03
│   ├── index.html
│   ├── AGENTS.md
│   └── mesa_*.jpeg
├── screenshots/                     ← Capturas de referencia (propuesta 01)
├── screenshots_v2/                  ← Capturas post-pulido (propuesta 01)
├── README.md                        ← Este archivo
├── DEPLOY.md                        ← Cómo subir a GitHub Pages
└── .gitignore
```

---

## Preview Local (3 propuestas a la vez)

```bash
python3 -m http.server 7777
```

Luego abre en el navegador:
- `http://localhost:7777/`
- `http://localhost:7777/proposal-2-cuaderno/`
- `http://localhost:7777/proposal-3-russian-craft/`

Cada propuesta tiene un **badge** flotante en la parte superior que la identifica, para que el cliente no se confunda al verlas una junto a otra.

---

## Datos del Cliente

- **Carpintero**: Nikita (originario de Moscú, aprendió el oficio en CDMX en 2014)
- **WhatsApp**: `+52 1 614 457 9884` → `5216144579884` (formato para wa.me)
- **Idiomas del sitio**: Español (principal), con marcas tipográficas en cirílico (`Москва`) como gesto narrativo
- **13 piezas** en 5 categorías: sala, comedor, oficina, exterior, especial

---

## Desplegar en GitHub Pages

Ver [`DEPLOY.md`](./DEPLOY.md) para el paso a paso completo (crear repo público, push, habilitar Pages, compartir URL con el cliente).

**Resumen rápido:**
```bash
git init && git add . && git commit -m "Initial: 3 proposals"
git branch -M main
git remote add origin https://github.com/<tu-usuario>/<repo>.git
git push -u origin main
# En GitHub: Settings → Pages → Branch: main, Folder: / (root)
```

Las 3 propuestas estarán en:
- `https://<user>.github.io/<repo>/`
- `https://<user>.github.io/<repo>/proposal-2-cuaderno/`
- `https://<user>.github.io/<repo>/proposal-3-russian-craft/`

---

## Después de que el cliente elija

1. Abre la carpeta de la propuesta ganadora.
2. Borra el bloque del badge:
   ```html
   <!-- PROPOSAL BADGE (remove this block once a winner is picked) -->
   <div class="prop-badge" ...> ... </div>
   ```
3. Borra la regla CSS `.prop-badge` correspondiente.
4. Borra las otras 2 carpetas de propuestas.
5. Mueve el `index.html` ganador a la raíz.
6. Sincroniza con tu hosting compartido (ver `DEPLOY.md`).

