# Galería de arte · Met · MoMA · Cleveland

Un screensaver de arte para el navegador: slideshow a pantalla completa con pinturas de tres museos — el [Met](https://www.metmuseum.org) (clásico europeo, Open Access), el [MoMA](https://www.moma.org) (arte moderno) y el [Cleveland Museum of Art](https://www.clevelandart.org) (CC0) — conmutable desde Ajustes.

![Met Gallery en acción](assets/preview-ficha.png)

## Características

- Más de **5.100 pinturas** de tres museos: ~2.600 del Met (Open Access, resolución original de más de 3000 px), **2.197 del MoMA** (dataset CC0, imágenes servidas por moma.org) y **310 del Cleveland** (CC0, hasta 3400 px).
- Transiciones con crossfade y efecto **Ken Burns** (zoom y paneo lentos).
- **Ficha de cada obra**: título, artista, fecha y técnica, con enlace a la página oficial del museo.
- **Modo screensaver**: los controles y el cursor se ocultan solos tras unos segundos de inactividad.
- Ajustes persistentes (localStorage):
  - Duración por obra (5–120 s)
  - Calidad de imagen: original o web (carga rápida)
  - Encuadre: obra completa (bandas negras) o llenar pantalla (modo wallpaper)
  - Museo: Met, MoMA, Cleveland o todos mezclados
  - Colección: toda la galería o solo las obras destacadas del Met
- Carga por lotes en segundo plano y precarga de las próximas imágenes: sin cortes entre obras.
- Wake Lock API: en pantalla completa, evita que el monitor se apague.
- **Ocultar pinturas religiosas**: opción en Ajustes que excluye de la galería la iconografía cristiana (Madonnas, santos, crucifixiones, natividades…), detectada por los tags de tema del Met.

![Opción "Ocultar pinturas religiosas" en Ajustes](assets/preview-religiosas.png)

- **Pantallas táctiles** (Huawei IdeaHub, tablets): deslizá horizontalmente para cambiar de obra; los controles aparecen al tocar la pantalla.

## Uso local

Abrí `index.html` en cualquier navegador moderno (con `moma-paintings.js` al lado para las obras del MoMA). No requiere servidor, build ni claves: todo corre en el cliente contra las APIs públicas (solo necesita internet).

| Atajo | Acción |
|---|---|
| `←` / `→` | Obra anterior / siguiente |
| `Espacio` | Pausar / reanudar |
| `F` o doble clic | Pantalla completa |
| `I` | Fijar la ficha de la obra |
| `Esc` | Cerrar ajustes |

## Deploy

Es un sitio **100% estático** (`index.html` + `moma-paintings.js`, sin dependencias ni build): cualquier hosting estático lo sirve.

### Render (Static Site)

Opción A — Dashboard: **New → Static Site** → este repo → Build Command **vacío** → Publish Directory `.`.

Opción B — Blueprint (este repo incluye `render.yaml`): **New → Blueprint** → este repo. Render crea el Static Site automáticamente, con HTTPS y subdominio `onrender.com`.

### GitHub Pages (alternativa)

Settings → Pages → Deploy from a branch → `main` / root. Cero configuración adicional.

## Cómo funciona

- **Met**: listado vía `/public/collection/v1.1/search?departmentId=11&hasImages=true&medium=Paintings` (endpoint v1.1 paginado; el `/v1/search` se retiró en octubre de 2026) y detalle vía `/public/collection/v1/objects/{id}` → `primaryImage` (original) y `primaryImageSmall` (web). El filtro `medium=Paintings` deja fuera marcos y dibujos. Imágenes en caliente desde `images.metmuseum.org`.
- **MoMA**: dataset abierto CC0 ([github.com/MuseumofModernArt/collection](https://github.com/MuseumofModernArt/collection)) embebido en `moma-paintings.js` (2.197 pinturas con imagen); las imágenes las sirve `moma.org` en caliente (tope 1024 px). Ojo: las imágenes del MoMA **no** son open access (derechos gestionados) — pensado para uso personal.
- **Cleveland**: API Open Access (`openaccess-api.clevelandart.org`), pinturas europeas CC0 con imagen (310), variantes web (~900 px) y print (~3400 px), servidas desde `openaccess-cdn.clevelandart.org`.
- Este repo no copia ni almacena ninguna imagen: todas se muestran en caliente desde cada museo.
- **Filtro religioso**: client-side, sobre el campo `tags` de cada obra (vocabulario del Met: *Christ*, *Virgin Mary*, *Saints*, *Nativity*, *Crucifixion*, etc.); para MoMA y Cleveland, que no tienen tags, se aplica un filtro por título. La opción *Destacadas* aplica solo a la porción del Met.
- **Compatibilidad**: tipografías con carga no bloqueante y CSS con fallbacks (funciona en motores antiguos y en redes que bloquean Google Fonts); en hardware modesto se usa la calidad Web por defecto. Si una imagen en calidad original no carga, se reintenta automáticamente con la versión Web (y tras tres reintentos la calidad pasa a Web sola).

## Créditos

- Datos e imágenes: [The Met Open Access](https://www.metmuseum.org/information/open-access) — CC0 1.0; [MoMA Collection dataset](https://github.com/MuseumofModernArt/collection) — CC0 (imágenes vía moma.org); [Cleveland Museum of Art Open Access](https://www.clevelandart.org/information/open-access) — CC0.
- Este proyecto no está afiliado a ninguno de los museos.

## Licencia

Código bajo [MIT](LICENSE).
