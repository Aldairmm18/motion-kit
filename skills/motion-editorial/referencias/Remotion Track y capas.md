# Remotion Track y capas

Trampas vistas al armar el piloto del reel de Territorio Tech (JAC-Efe-Gomez, 2026-10-18), con Remotion 4.0.536.

- **Una `<Track>` no acepta ítems que se solapan.** El B-roll que tapa una toma larga va en su propia pista ("B-roll"), encima de "Imagen". Lo mismo con los crossfades (pistas A/B), como en Motion-graphics-ia.
- **Orden de pintado:** `<Track>` usa `layout: "none"`. Un `<Sequence>` con posición absoluta (el grupo toma → palabra → persona recortada) se pinta **encima** de un `<Video>` normal de una pista posterior, aunque esa pista vaya después en el código. Solución: darle al B-roll `style={{ position: "absolute", width: "100%", height: "100%" }}`.
- **`Interactive.create`:** todas las props del componente tienen que estar en el `schema` (si no, TypeScript da "not assignable to type never"). Y **no se puede llamar `trimBefore`** a una prop propia porque choca con la de Sequence: usar otro nombre (`desde`).
- El arreglo de render en macOS 27 (`Config.setChromeMode("chrome-for-testing")`) sigue haciendo falta: ver Remotion no renderiza en macOS 27.
