# kaeliapps.com

Página de inicio de Kaeli (https://kaeliapps.com), publicada con GitHub Pages
desde este repo. `www.kaeliapps.com` redirige aquí sola (GitHub Pages lo hace
cuando existe el registro DNS de `www`).

Todas las páginas de Kaeli siguen las mismas reglas: HTML estático, sin
cookies, sin analítica, sin fuentes ni scripts externos, en español e inglés
según el idioma del navegador (sin JavaScript se ven en español, con un enlace
"English" que funciona solo con CSS mediante `#en`).

## Textos: no encasillar el público de las apps

En ningún texto publicado (textos visibles, `<title>`, meta description, Open
Graph, `alt` de imágenes, README) se describe una app de Kaeli como algo para
el aula, la escuela, maestros o docentes, ni *classroom*, *school* o *teachers*
en inglés. Descripción de referencia de Wibblo:

- ES: "Fichas imprimibles: bingo, sopa de letras, crucigramas y más."
- EN: "Printable activities: bingo, word search, crosswords and more."

Al publicar una página nueva, revisar con:

```bash
grep -rniE "aula|escuela|escolar|maestr|docente|classroom|school|teacher" --include="*.html" --include="*.md" --exclude="README.md" .
```

## Organización: un repo público por sitio

| Repo | Dominio | Contenido |
|---|---|---|
| `kaelistudios.github.io` (este) | `kaeliapps.com` | Inicio de Kaeli con la lista de apps |
| `wibblo-web` | `wibblo.kaeliapps.com` | Página del QR (`/`) y política de privacidad (`/privacidad/`) |
| `kaeli-legal` | sin dominio propio | Solo redirige la dirección vieja de la política de Wibblo |

GitHub Pages admite **un dominio por repo**, por eso cada app tiene el suyo.
Como este repo usa `kaeliapps.com`, cualquier otro repo con Pages **sin**
dominio propio se sirve en `kaeliapps.com/<repo>/` (y su dirección de
`kaelistudios.github.io/<repo>/` redirige ahí).

## Cómo agregar una app nueva (ej. `blunkoo.kaeliapps.com`)

1. **DNS en Cloudflare** (zona `kaeliapps.com`): un registro `CNAME`
   `blunkoo` → `kaelistudios.github.io`, en **DNS only** (nube gris). Con
   proxy, GitHub no puede emitir el certificado HTTPS.
2. **Repo público nuevo** `blunkoo-web`, copiando la estructura de
   `wibblo-web`:
   - `CNAME` con una sola línea: `blunkoo.kaeliapps.com`
   - `index.html`: página del QR/tienda (cambiar el ID del paquete de Google
     Play, el nombre, la descripción y el ícono)
   - `privacidad/index.html`: su política de privacidad
   - `.nojekyll` (vacío)
3. **GitHub Pages** del repo nuevo: Settings → Pages → Source "Deploy from a
   branch", rama `main`, carpeta `/`. En "Custom domain" debería aparecer
   `blunkoo.kaeliapps.com` (sale del archivo `CNAME`). Cuando el certificado
   esté listo, marcar **Enforce HTTPS**.
4. **Este repo**: sumar la app a la lista de `index.html` (en las dos
   secciones, `#es` y `#en`) con su ícono.
5. **En la app**: usar `https://blunkoo.kaeliapps.com` como enlace de tienda o
   QR y `https://blunkoo.kaeliapps.com/privacidad/` como política de
   privacidad (también en Google Play Console / App Store Connect).

El dominio `kaeliapps.com` está verificado en la cuenta de GitHub
(Settings → Pages → Verified domains, registro TXT
`_github-pages-challenge-kaelistudios`), así que ningún otro usuario de GitHub
puede tomar sus subdominios. No borrar ese registro TXT.

## Cuando una app llegue al App Store

En su `index.html`, poner la URL del App Store en el `href` de los dos botones
`.appstore` (español e inglés) y quitarles el atributo `hidden`. Con eso, en
iPhone/iPad la página redirige al App Store y en computadora muestra el botón.
