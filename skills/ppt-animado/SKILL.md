---
name: ppt-animado
description: Crea presentaciones de PowerPoint animadas por código (pptxgenjs más edición del XML para animaciones de entrada, énfasis en bucle, transiciones y música de fondo). Úsala cuando pidan una presentación o PPT con animaciones, que arranque sola o que tenga música.
---

# PowerPoint animado por script

Parte de `principios`: la misma paleta de un acento, dos tipografías y jerarquía clara que en video.

## Flujo
1. Armar las diapositivas con pptxgenjs. Nombrar cada forma con un código de orden y efecto en `objectName` (por ejemplo `A3-rise-titulo`).
2. pptxgenjs no escribe animaciones: abrir el .pptx (zip) y editar `ppt/slides/slideN.xml`, insertando `<p:transition>` y `<p:timing>` justo después de `</p:clrMapOvr>`.
3. Para que todo arranque solo: grupo con `delay="indefinite"` + `evt="onBegin"`, y efectos `withEffect` con `delay` escalonado.
4. Verificar en PowerPoint de verdad (AppleScript: contar efectos de la secuencia principal y exportar a PDF para ver las diapositivas).

## Referencia
- `referencias/Animaciones de PowerPoint por script.md`: el XML exacto de cada efecto, `bldP`, música en todas las diapositivas y animaciones de énfasis en bucle.
