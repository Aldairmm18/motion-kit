# Remotion con fuentes y escenas desde JSON

Sirve para hacer una plantilla de Remotion que cambia de marca sin tocar el código.

- **Fuente por nombre:** `getAvailableFonts()` de `@remotion/google-fonts` trae todas las fuentes con un `load()` que importa su módulo. Se busca por `fontFamily`, se llama `m.loadFont("normal", { subsets: ["latin"] }).waitUntilDone()` y se espera con `delayRender` / `continueRender`. Si el nombre no existe, `cancelRender` con un mensaje claro.
- **Duración desde el guion:** `calculateMetadata` suma las escenas y resta lo que se cruza en cada transición de `TransitionSeries`.
- **Una composición por archivo JSON:** un script en Node arma un `.ts` que importa cada JSON (con `resolveJsonModule` en el tsconfig) y `Root.tsx` lo recorre. Los nombres de carpeta y de composición solo admiten letras, números y guiones.
- **Material junto al perfil:** `Config.setPublicDir("./marcas")` hace que `staticFile("marca/media/x.png")` lea de la carpeta de cada marca.
- **Transiciones mezcladas:** `fade()`, `wipe()` y `slide()` tienen tipos distintos. Para elegir una según el guion, convertirlas a `TransitionPresentation<Record<string, unknown>>`.
- **Resaltado de palabras:** se parte el texto por espacios **y** por asteriscos, así la puntuación queda pegada ("*piel*?") y un resaltado de varias palabras se une en una sola barra. El color del texto resaltado pasa con `interpolateColors` a medida que crece la barra; si no, se ve blanco sobre fondo claro antes de que llegue la barra.

Usado en Motion-graphics-ia (mini-ootto, 2026-10-28). Relacionado: Remotion no renderiza en macOS 27.
