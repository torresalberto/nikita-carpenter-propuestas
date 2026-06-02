# Guía de Despliegue — GitHub Pages + Hosting Compartido

Esta guía cubre dos cosas:
1. **Subir las 3 propuestas a GitHub como repo público** (para compartir con el cliente).
2. **Sincronizar el repo con tu hosting compartido** (para producción real).

---

## Parte 1 — Crear el repo público en GitHub

### Paso 1. Crea el repo en github.com
1. Ve a https://github.com/new
2. **Repository name**: por ejemplo `nikita-carpenter-propuestas` o `nikita-web`
3. **Description**: "3 propuestas de sitio web para Nikita Carpenter — taller de carpintería CDMX"
4. **Visibilidad**: **Public** ✓
5. **NO** inicialices con README, .gitignore, ni license (ya los tenemos locales).
6. Click **Create repository**

### Paso 2. Sube el código

En tu terminal local (en la carpeta `Nikita_carpenter/`):

```bash
# Inicializa git (si no lo habías hecho)
git init

# Configura tu identidad (solo la primera vez en esta máquina)
git config user.name  "Tu Nombre"
git config user.email "tu@email.com"

# Añade todos los archivos
git add .

# Verifica qué se va a subir (importante: revisa que NO haya claves/passwords)
git status

# Primer commit
git commit -m "Initial: 3 propuestas de sitio estático"

# Renombra la rama a main
git branch -M main

# Conecta con el repo remoto (cambia TU-USUARIO y NOMBRE-REPO)
git remote add origin https://github.com/TU-USUARIO/NOMBRE-REPO.git

# Sube
git push -u origin main
```

> 🔐 **Te va a pedir autenticación.** Usa un [Personal Access Token (PAT)](https://github.com/settings/tokens) si tienes 2FA activado, o el helper de credenciales de Git.

### Paso 3. Habilita GitHub Pages

1. En el repo de GitHub, ve a **Settings → Pages**
2. **Source**: `Deploy from a branch`
3. **Branch**: `main`, folder: `/ (root)`
4. Click **Save**
5. Espera 1–2 minutos. Tu sitio estará en:
   ```
   https://TU-USUARIO.github.io/NOMBRE-REPO/
   https://TU-USUARIO.github.io/NOMBRE-REPO/proposal-2-modernista/
   https://TU-USUARIO.github.io/NOMBRE-REPO/proposal-3-russian-craft/
   ```

### Paso 4. Comparte con el cliente

Envíale las 3 URLs (la del root + las 2 subcarpetas) por WhatsApp/email. Cada propuesta tiene un **badge distintivo** flotante que dice `01 PROPUESTA · DIARIO DE TALLER`, etc., para que no se confunda al abrirlas.

---

## Parte 2 — Sincronizar con tu hosting compartido (producción)

Una vez que el cliente elija una propuesta, hay 2 caminos según tu hosting.

### Opción A — Hosting con Git (recomendado)

Si tu hosting compartido soporta **Git pull** (la mayoría de cPanel modernos, Render, Railway, etc.):

```bash
# En el servidor (o vía SSH):
cd ~/public_html/   # o donde esté tu web root
git clone https://github.com/TU-USUARIO/NOMBRE-REPO.git .
# Para actualizar después:
git pull
```

### Opción B — Hosting con FTP (subida manual)

Cuando el cliente elija:

1. **Limpia** la propuesta ganadora:
   - Borra el bloque del badge en el HTML (busca `PROPOSAL BADGE`).
   - Borra la regla CSS `.prop-badge`.
   - Borra las 2 carpetas de las propuestas perdedoras.
   - Opcional: mueve el `index.html` ganador a la raíz.
2. **Sincroniza** vía FTP/SFTP:
   - Sube el `index.html` y los 13 `mesa_*.jpeg` al directorio público (generalmente `public_html/` o `www/`).
   - En FileZilla / Cyberduck: arrastra y sobrescribe.
3. **Verifica** abriendo tu dominio real en un navegador incógnito.

### Opción C — Hosting que sincroniza con GitHub automáticamente

Servicios como **Netlify**, **Vercel**, **Cloudflare Pages** o **Render** se conectan a tu repo de GitHub y redespliegan en cada `git push`:

1. Conecta el servicio a tu repo.
2. Build command: *(vacío, no hay build step)*.
3. Publish directory: `.` (raíz) o `proposal-1/` si quieres servir solo una.
4. Cada `git push` redespliega automáticamente.

---

## Checklist antes de subir a producción

Antes de que el cliente vea el sitio:

- [ ] Las 3 carpetas existen y cada una tiene su `index.html` + 13 JPEGs
- [ ] El `README.md` está actualizado
- [ ] El WhatsApp `5216144579884` aparece en cada `index.html` (3 menciones por archivo: nav, footer, `const wa`)
- [ ] NO hay menciones de `52XXXXXXXXXX` (placeholder)
- [ ] NO hay fotos duplicadas entre las 5 categorías (`grep -oE "src: '[^']+'" index.html | sort | uniq -d` debe estar vacío)
- [ ] Cada propuesta tiene su badge visible
- [ ] Ningún archivo de más de ~500 KB se subió sin querer (los JPEGs son todos < 250 KB)
- [ ] El `.gitignore` excluye `_test_*.html` y `screenshots*/` si no quieres que se suban

---

## Troubleshooting

### "Mi sitio no se ve / muestra 404"
- Espera 1–2 minutos después de habilitar Pages, GitHub tarda en propagar.
- Revisa que el repo sea **público** (Settings → General → Danger Zone).
- Revisa que la rama sea `main` y la carpeta `/ (root)`.

### "Las imágenes no cargan"
- Las rutas son **relativas** (`mesa_madera.jpeg` no `/mesa_madera.jpeg`), por lo que funcionan en cualquier carpeta o subdominio.
- En GitHub Pages, las rutas relativas funcionan perfecto porque cada propuesta está en su propia carpeta y los JPEGs van al lado.

### "Quiero actualizar algo después de subir"
- Edita el archivo local → `git add .` → `git commit -m "mensaje"` → `git push`
- GitHub Pages se redesplega solo en ~30 segundos.

### "Quiero ocultar las otras propuestas para que el cliente solo vea una"
- Opción 1: crea un branch por propuesta (`propuesta-1`, `propuesta-2`, `propuesta-3`) y comparte la URL del branch específico.
- Opción 2: edita el `README.md` para que solo tenga la URL de la propuesta que quieres enseñar.
- Opción 3: sube solo la carpeta ganadora al hosting, omite las otras 2.

---

