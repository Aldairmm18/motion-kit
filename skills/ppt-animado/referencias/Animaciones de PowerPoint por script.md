# Animaciones de PowerPoint por script

pptxgenjs no escribe animaciones. Se agregan después editando `ppt/slides/slideN.xml`:

- Nombrar cada forma con `objectName` codificado (ej. `A3-rise-...`: orden y efecto) y luego leer `cNvPr id` para obtener el `spid`.
- Insertar `<p:transition>` y `<p:timing>` justo después de `</p:clrMapOvr>` (ese orden es obligatorio).
- Para que todo arranque solo: un grupo de clic con `<p:cond delay="indefinite"/>` + `<p:cond evt="onBegin" delay="0"><p:tn val="2"/></p:cond>`; dentro, efectos `withEffect` con `delay` escalonado.
- Efecto = `p:set` (visibility) + `p:animEffect` (fade o `wipe(left)`) + `p:anim` sobre `ppt_y` o `ppt_w/ppt_h` para subir o hacer zoom.
- `p:bldP` solo para `p:sp` (con `animBg="1"` si no tiene texto), nunca para imágenes.
- Verificación real en Mac: AppleScript a PowerPoint, `count of effects of main sequence of timeline of slide i`, y exportar PDF con `save ... as save as PDF`. PDF→PNG con un script Swift de PDFKit cuando no hay poppler.

Relacionado: JAC-Efe-Gomez

## Música de fondo y animaciones en bucle (2026-10-27)
- `addMedia({type:"audio"})` de pptxgenjs escribe `<a:videoFile>` y repite un id de forma: hay que cambiarlo a `<a:audioFile>` y darle un id único.
- Para que suene en todas las diapositivas y en loop: `p:cmd playFrom(0.0)` dentro de la secuencia principal y un `<p:audio><p:cMediaNode numSld="999">` con `repeatCount="indefinite"` al lado de la secuencia.
- Animaciones de énfasis en bucle (girar, tambalear, latir): `presetClass="emph"` con `repeatCount="indefinite"` y `autoRev="1"`; cada forma necesita su `bldP grpId="1"`.
- Ejemplo: Competencia absurda (`competencia-absurda-ppt/animate.py`).
