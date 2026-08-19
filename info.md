# Rizoma

Plataforma de cursos y talleres. Este texto vive en `contenido/info.md` — edítalo directamente, no hace falta tocar el HTML.

## Cómo funciona

Cada entrada del menú (Info, cada curso, cada registro en Pasados) corresponde a un archivo `.md` dentro de `contenido/`. El menú de la izquierda se arma a partir de `contenido/indice.json`: para agregar un curso nuevo, agrega ahí una entrada con su `slug`, `titulo` y ruta de `archivo`, y sube el `.md` correspondiente.

## Imágenes

Las imágenes van en `assets/` y se referencian desde el Markdown con ruta relativa a la raíz del sitio, así:

![Ejemplo de imagen](assets/ejemplo.svg)

## Formato soportado

Encabezados, listas, **negrita**, *cursiva*, enlaces, citas, tablas, bloques de código y líneas horizontales — el estándar de Markdown, vía [marked.js](https://marked.js.org/).

---

Reemplaza este texto por la descripción real de la plataforma cuando estés listo.
