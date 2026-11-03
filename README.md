# kaeliapps.com

Página de inicio de Kaeli (https://kaeliapps.com), publicada con GitHub Pages
desde este repo. `www.kaeliapps.com` redirige aquí sola (GitHub Pages lo hace
cuando existe el registro DNS de `www`).

Todas las páginas de Kaeli siguen las mismas reglas: HTML estático, sin
cookies, sin fuentes externas, en español e inglés
según el idioma del navegador (sin JavaScript se ven en español, con un enlace
"English" que funciona solo con CSS mediante `#en`).

## Estadísticas de visitas

kaeliapps.com y wibblo.kaeliapps.com cuentan visitas con **Cloudflare Web Analytics**
(gratis, sin cookies ni datos personales). Es el único script externo permitido:
la línea `static.cloudflareinsights.com/beacon.min.js` va antes de `</body>` en cada
página, con el token del sitio `kaeliapps.com` (Cloudflare → Análisis web).
AlojaConmigo, Sesly y CreatiPlan lo tienen activado desde su proyecto de
Cloudflare Pages (Métricas → Web Analytics) y no necesitan la línea.

## Correo de contacto

Un solo buzón con una subdirección por app: `soporte+<slug>@kaeliapps.com` (slug =
carpeta de la app en `kaeli-tools/apps`). En esta portada, «Escríbenos» / «Write to
us» va al contacto general `soporte@kaeliapps.com`, con asunto «[Kaeli] » y la firma
«—\nKaeli web · es» (o `en`) al final del cuerpo. Si una tarjeta de app lleva enlace
de soporte, usa la dirección de su app y su nombre en el asunto («[Wibblo] »,
«Wibblo web · es»). En el `href`, los espacios van como `%20` (nunca `+`), los saltos
de línea como `%0A` y el `&` entre parámetros como `&amp;`:

```html
<a href="mailto:soporte@kaeliapps.com?subject=%5BKaeli%5D%20&amp;body=%0A%0A%0A%E2%80%94%0AKaeli%20web%20%C2%B7%20es">soporte@kaeliapps.com</a>
```

Para revisar todos los `.html` (sin dependencias; código de salida 1 si hay problemas):

```bash
node tools/check-mailto.mjs
```

## Mapa del sitio

`robots.txt` y `sitemap.xml` en la raíz. Google Search Console usa la propiedad
de dominio `kaeliapps.com`, que cubre todos los subdominios.

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
| *(Firebase Hosting, proyecto `blunkoo-prod`)* | `blunkoo.kaeliapps.com` | Página de Blunkoo, privacidad, términos y borrar la cuenta. Vive en el repo de la app (carpeta `web/`), no en GitHub Pages; su CNAME en Cloudflare apunta a `blunkoo-prod.web.app` |
| *(Cloudflare Pages, proyecto `alojaconmigo-web`)* | `alojaconmigo.kaeliapps.com` | Web de AlojaConmigo: presentación, descarga (`/descargar`), páginas de reservas de cada anfitrión y legales. Vive en el repo privado de la app (`kaelistudios/alojaconmigo`, carpeta `web/`) |
| *(Cloudflare Pages, proyecto `sesly-web`)* | `sesly.kaeliapps.com` | Web de Sesly: portada, guías (`/guias/`), páginas de citas de cada profesional y legales. Vive en el repo privado de la app (`kaelistudios/sesly`, carpeta `web/`) |

GitHub Pages admite **un dominio por repo**, por eso cada app tiene el suyo.
Como este repo usa `kaeliapps.com`, cualquier otro repo con Pages **sin**
dominio propio se sirve en `kaeliapps.com/<repo>/` (y su dirección de
`kaelistudios.github.io/<repo>/` redirige ahí).

## Cómo agregar una app nueva con GitHub Pages (ej. `otraapp.kaeliapps.com`)

> Blunkoo, AlojaConmigo y Sesly son la excepción: sus páginas se publican fuera de GitHub Pages (Firebase Hosting y Cloudflare Pages). Para ellas solo se hizo el paso 4 (sumarlas a la lista de `index.html`).


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
   secciones, `#es` y `#en`) con su ícono. Si su tarjeta lleva enlace de
   soporte, agregar su dirección a `ALLOWED` en `tools/check-mailto.mjs`
   (ver «Correo de contacto»). En la web de la app, el contacto va a
   `soporte+<slug>@kaeliapps.com`.
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
