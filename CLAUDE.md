# Visor · Roadtrip por la Puglia

Visor web de un viaje familiar por la Puglia, publicado en GitHub Pages. Es un único `index.html` que contiene el CSS, el JavaScript y los datos. No tiene build ni dependencias locales.

## Contexto (decidido en el chat, no está en el código)

- Viaje del 3 al 12 de octubre de 2026. Siete viajeros y dos coches.
- Los días 3 y 4 son solo de Alejandro y María (Trani). En el código se marcan con `pre:true`. El viaje familiar va del 5 al 12.
- El contenido sale de un Excel definitivo y está **cerrado**: itinerario, horarios, reservas y textos. No se cambia sin pedirlo expresamente.
- **Diseño:** limpio y profesional.
  - Colores: blanco de cal (`--paper`), tinta azul pizarra (`--ink`) y un único acento azul Adriático (`--accent`).
  - Tipografías: Newsreader para los títulos (`--serif`) y Figtree para el resto (`--sans`).
  - Sin categorías de colores por tipo de actividad y sin emojis.
  - Modo oscuro automático (`prefers-color-scheme`, con `data-theme` para forzarlo). Todo lo nuevo debe verse bien en los dos modos.
- **Tiempo:** previsión de eltiempo.es consultada el 28-09-2026, escrita a mano en `wx` y `sun` de cada día. Solo se actualiza si se pide.
- **Fotos:** van en local, en `img/`, dentro del repo. Nunca se enlazan imágenes de webs de terceros, por derechos y para que no se rompan.
  - Límites: banner ≤ 1600 px de ancho y ~250 KB; fotos por día ≤ 900 px y ~120 KB.
  - Formato JPEG. Todo lo que no esté en la primera pantalla lleva `loading="lazy"`.
  - Si falta una foto no se pone ninguna de relleno: ese día se ve sin foto.

## Navegación (no romper)

- Barra de días arriba, botón "Hoy" (`todayKey()`) y botón flotante "Mapa" (`fab()`).
- Pantalla de mapa con Leaflet 1.9.4 (cdnjs) sobre satélite de Esri. Las rutas se calculan con OSRM y se cachean en `localStorage` con la clave `rutas_v1_*`. Si OSRM falla, se usa el respaldo `ROAD`. Si falla el satélite, se usa el mapa vectorial `REG`.
- Enlaces "Día anterior / siguiente" (`.pager`).
- Rutas por hash: `#inicio`, `#5`, `#mapa`, `#mapa/5`.

## Mapa de `index.html`

| Líneas aprox. | Qué hay |
|---|---|
| 10–671 | CSS de Leaflet incrustado (no tocar) |
| 672–830 | CSS propio: tokens en `:root`, modo oscuro, cabecera, `.hero`, `.day-head`, `.plan`, mapa, `@media` |
| 833–842 | HTML: `header.top` (marca, "Hoy", `nav#days`), `main#main`, `footer#foot`, `button#fab` |
| 846–847 | `MAP` (contornos en SVG del mapa esquemático) y `REG` (GeoJSON de las regiones). Son líneas enormes: no abrirlas enteras |
| 848 | `P`: coordenadas y nombre de cada parada |
| 857 | `ROAD`: trazados de respaldo por carretera; `seg()` y `pathOf()` |
| 879 | `DAYS`: todo el contenido del viaje (`key`, `title`, `sum`, `wx`, `sun`, `plan`, `tonight`, `park`, `ctx`…) |
| ~971 | `todayKey()` |
| ~983 | `mapSVG()`: mapa esquemático en SVG (portada y cada día) |
| ~1008 | `nav()`, delegación de clics, `mark()`, `fab()` |
| ~1041 | Vista `home()`: portada con `.hero`, cuenta atrás, mapa, "Quién llega" y la lista de días |
| ~1073 | Vista `day(key)`: `header.day-head`, `.day-grid` (datos + mapa) e itinerario |
| ~1124 | Pantalla de mapa: `osrm()`, `restyle()`, `selectDay()`, `vectorBase()`, `mapView()` |
| ~1199 | `route()`: router por hash |

Las vistas se generan con plantillas de texto (`main.innerHTML = ...`).

## Probar en local

```sh
python3 -m http.server 8000
# abrir http://localhost:8000/#inicio, #5, #mapa/5
```

Comprobar:
- a 390 px y a 1280 px de ancho;
- en modo claro y en modo oscuro;
- la portada, un día con foto, un día sin foto y la pantalla de mapa;
- que la consola no muestra errores.

No hay tests.

## Publicar

Hacer commit y dejar que el usuario revise antes de hacer `git push`. Sin push no se publica en GitHub Pages.
