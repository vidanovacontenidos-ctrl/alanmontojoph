# Alan Montojo PH — sitio

Portafolio de fotografía de combate. Sitio estático de una sola página:
HTML, CSS y JavaScript en un solo archivo, más las fotos.

- **Peso total:** 5,3 MB
- **Primera carga:** ~750 KB (portada + tres primeros planos). El resto se
  carga a medida que se baja.
- **Dependencias externas:** Google Fonts (Anton, Space Grotesk, JetBrains
  Mono), GSAP + ScrollTrigger desde cdnjs, e icono de WhatsApp desde
  Simple Icons. Si el CDN de GSAP falla, la página sigue siendo legible: las
  galerías se convierten en fotos apiladas.
- **Sin compilación, sin dependencias de Node, sin base de datos.**

---

## Archivos

```
index.html                 todo el sitio: marcado, estilos y guion
robots.txt                 permiso de indexado + ubicación del sitemap
sitemap.xml                una sola URL
_headers                   caché y cabeceras de seguridad (Netlify / Cloudflare)
assets/images/hero/        portada
assets/images/about/       retrato de la sección "Quién dispara"
assets/images/gallery/     photo-01 … photo-26
```

---

## ANTES DE PUBLICAR

Cuatro cosas sin resolver. Las dos primeras son obligatorias.

1. **Reemplazar `TU-DOMINIO.com`.** Aparece en `index.html` (canonical y
   og:url), en `robots.txt` y en `sitemap.xml`. Buscar y reemplazar por el
   dominio real, con `https://` y barra final.

2. **Autorizaciones de imagen de los menores.** Las fotos de judo son de
   competición juvenil en España. Publicar imágenes de menores identificables
   requiere consentimiento de sus tutores, aunque el evento sea público y las
   fotos sean propias. Confirmarlo con el club u organización antes de subir.

3. **Instagram y Behance apuntan a `#`.** Están en el pie. Poner las URL
   reales o borrar los enlaces; un enlace que no lleva a ningún lado es peor
   que no tenerlo.

4. **Dos fotos tienen el fondo reconstruido por IA** (el derribo y una de las
   celebraciones): el gimnasio del fondo no es el de la toma original. Decidir
   si se publican como registro de una sesión o se reemplazan por las
   originales sin editar.

---

## Publicar

Cualquiera de estas opciones sirve. Están ordenadas de más simple a más
control.

### Netlify Drop — sin cuenta, dos minutos

1. Entrar a `app.netlify.com/drop`.
2. Arrastrar **la carpeta completa** (no el zip, no solo el `index.html`).
3. Queda publicado en una URL tipo `nombre-al-azar.netlify.app`, con HTTPS.
4. Para usar el dominio propio: *Domain settings → Add custom domain*, y
   apuntar el DNS donde indique.

Respeta el archivo `_headers`, así que la caché queda bien configurada sola.

### Cloudflare Pages — gratis, rápido en Argentina

1. Subir la carpeta a un repositorio de GitHub.
2. En Cloudflare: *Workers & Pages → Create → Pages → Connect to Git*.
3. **Build command:** dejar vacío. **Output directory:** `/`.
4. Dominio propio desde *Custom domains*.

También respeta `_headers`.

### GitHub Pages — gratis, si ya se usa GitHub

1. Repositorio nuevo, subir todos los archivos a la raíz de la rama `main`.
2. *Settings → Pages → Source: Deploy from a branch → main / (root)*.
3. Queda en `usuario.github.io/repositorio`.

No lee `_headers`; la caché queda en los valores por defecto de GitHub, que
son aceptables.

### Hosting propio por FTP

Subir el contenido de la carpeta a `public_html` o `www`. Requisitos:
certificado HTTPS activo y que `index.html` sea el documento por defecto.
`_headers` no funciona acá; el equivalente se configura en `.htaccess` o en
el panel del hosting.

---

## Mantenimiento

### Cambiar el número de WhatsApp

Aparece tres veces en `index.html`, siempre con el formato
`5491158975177` (código de país + 9 + número sin el 15). Buscar y reemplazar.

### Agregar o cambiar fotos

1. Exportar en JPEG, lado largo **1900 px**, calidad 84, progresivo.
2. Guardar en `assets/images/gallery/` con el nombre `photo-NN.jpg`.
3. Si es una foto nueva y no un reemplazo, subir el tope del bucle en el
   guion: `for (var s = 1; s <= 26; s++)`.
4. Colocarla en una composición: cada pieza del collage está posicionada a
   mano con `left`, `top`, `width` en porcentaje y `aspect-ratio` con la
   proporción real de la foto. **La altura de una pieza, en porcentaje del
   lienzo, es `ancho × relación_del_lienzo ÷ relación_de_la_foto`.** Hay que
   comprobar que `top + esa altura` no pase de 100, o la foto se sale.

### Ajustes rápidos

| Qué | Dónde | Valor actual |
|---|---|---|
| Círculo revelador de la portada | `heroLens`, variable `R` | `150` |
| Reloj de la secuencia de judo | atributo `data-clock` en `#arena-judo` | `240` segundos |
| Ritmo de scroll de la secuencia | `window.innerHeight * total * 0.66` | `0.66` |
| Acercamiento de cada par | campo `match` en `GALLERIES` | 1,28 a 1,62 |
| Colores | variables CSS en `:root` | `--crimson`, `--cream`, `--ink` |

### Textos que dependen del cliente

El sitio no afirma años de trayectoria, cantidad de eventos ni plazos de
entrega, porque esos datos no se aportaron. Si más adelante se agregan, tienen
que ser verificables: un dato inventado en un portafolio es un pasivo, no un
adorno.

---

## Accesibilidad y rendimiento

Implementado: enlace para saltar al contenido, foco visible en todo lo
interactivo, jerarquía de encabezados sin saltos, texto alternativo
descriptivo en las 26 fotos, contraste mínimo 4,5:1 en texto, objetivos
táctiles de 44 px, `prefers-reduced-motion` respetado (sin rastro del cursor,
sin animaciones de entrada), y funcionamiento sin JavaScript.

Pendiente de comprobar en dispositivo real: el rendimiento de las
composiciones superpuestas en teléfonos de gama baja.
