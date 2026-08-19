# rizoma.cursos

Sitio estático, sin build, para GitHub Pages. Todo el comportamiento vive en `index.html`; el contenido vive en Markdown dentro de `contenido/`.

## Estructura

```
index.html              shell: sidebar, ruteo, render de Markdown
contenido/
  indice.json            manifiesto — declara qué existe y dónde
  info.md
  cursos/*.md
  pasados/*.md
assets/                  imágenes referenciadas desde los .md
vendor/
  marked.js              parser de Markdown, v18.0.9 fijada, sin CDN
  fonts/                 Fira Code (Regular/SemiBold/Bold), OFL-1.1
```

## Agregar un curso o taller nuevo

1. Sube el archivo `contenido/cursos/mi-taller.md`.
2. Agrega una entrada en `contenido/indice.json`, dentro de `"cursos"`:
   ```json
   { "slug": "mi-taller", "titulo": "Mi Taller", "archivo": "cursos/mi-taller.md" }
   ```
3. El `slug` define la URL: `#/cursos/mi-taller`. Usa minúsculas y guiones, sin espacios ni tildes — es lo único que aparece en la ruta compartible.

Lo mismo aplica para `"pasados"`, con rutas `#/pasados/<slug>`.

La página **Info** es la única entrada fija (`indice.info`), no lleva slug propio: siempre vive en `#/info`.

## Imágenes

Van en `assets/` y se referencian en el Markdown con ruta relativa a la raíz del sitio, sin importar en qué subcarpeta esté el `.md` que las usa:

```markdown
![descripción](assets/foto.jpg)
```

Esto funciona porque el contenido se inyecta en el DOM de `index.html`; el navegador resuelve la ruta relativa al documento, no al archivo `.md` de origen.

## Probar en local

`fetch()` no funciona abriendo `index.html` directamente con doble clic (protocolo `file://` bloquea las peticiones). Sirve la carpeta con cualquier servidor local:

```bash
python3 -m http.server 8000
# o
npx serve .
```

y abre `http://localhost:8000`.

## Desplegar en GitHub Pages

Sube esta carpeta (tal cual, sin build) a un repositorio y activa GitHub Pages sobre la rama correspondiente (o `/docs` si prefieres esa convención). Como todas las rutas del proyecto son relativas, funciona igual si el sitio queda en la raíz del dominio o en un subpath tipo `usuario.github.io/rizoma.cursos/`.

## Qué verifiqué y qué no

**Verificado (pruebas automatizadas, no visual):**
- Ruteo por hash (`#/info`, `#/cursos/<slug>`, `#/pasados/<slug>`), incluyendo hashes vacíos, mal formados, con slash final y con caracteres codificados.
- Resolución de slugs inexistentes → 404 sin excepciones.
- Construcción del sidebar a partir de `indice.json`, incluyendo índice vacío o incompleto.
- Flujo completo simulado con jsdom: carga de índice, navegación entre secciones, apertura automática de la sección correspondiente, actualización de `<title>`, apertura/cierre del menú móvil y su cierre automático al navegar.
- `marked.parse()` contra la versión 18.0.9 real (no simulada), incluyendo el manejo de rutas de imagen.

**No verificado — requiere navegador real:**
- Aspecto visual y comportamiento del drawer móvil (transición, `translateX`, breakpoint de 760px) en dispositivos reales.
- Carga efectiva de Fira Code vía `@font-face` (el archivo está vendorizado y la regla declarada correctamente, pero no hay forma de confirmar el render tipográfico fuera de un navegador).
- Comportamiento en GitHub Pages real: sensibilidad a mayúsculas/minúsculas en rutas (Linux, a diferencia de macOS, distingue mayúsculas — si nombras un archivo `Taller.md` y el índice dice `taller.md`, fallará en producción aunque funcione en tu Mac).
