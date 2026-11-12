# Remotion no renderiza en macOS 27

Síntoma: `npx remotion render`, `still` o `compositions` fallan con *"Waiting for root component to load"* después de 28 s, aunque Studio funciona. Pasa en todos los proyectos (probado con remotion 4.0.536), dentro y fuera del sandbox.

- Causa probable: el `chrome-headless-shell` que trae Remotion no completa el `requestAnimationFrame` que libera ese `delayRender`.
- Arreglo: usar Chrome for Testing. En `remotion.config.ts`: `Config.setChromeMode("chrome-for-testing")`. En la API de Node, pasar `chromeMode: "chrome-for-testing"` a `selectComposition` y `renderStill`/`renderMedia`. La primera vez descarga el navegador (~150 MB).
- `--gl=swangle` no lo arregla.

Otro detalle de esa versión: en los esquemas de `Interactive.create`, los campos `type: "number"` exigen `hiddenFromList` en TypeScript. Sin él, `tsc` falla.

Relacionado: JAC-Efe-Gomez, Remotion a Premiere por XML
