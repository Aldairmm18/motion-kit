# After Effects por script

- Ejecutar un script desde la terminal: `osascript -e 'tell application "Adobe After Effects 2026" to DoScriptFile "/ruta/script.jsx"'`. Envolver en `with timeout of 900 seconds` si renderiza.
- `DoScript` no devuelve el resultado a la terminal (siempre da 0): para leer datos, que el script escriba un archivo de log.
- AE en español: los nombres visibles de efectos y propiedades cambian con el idioma. Usar **matchNames** (`ADBE Turbulent Displace`, `ADBE Vector Shape - Ellipse`...) o índices de propiedad. Las plantillas de salida también van en español: "Calidad alta con Alfa" = ProRes 4444 con transparencia.
- El ProRes de AE viene con alfa **premultiplicado**: si no se corrige, los colores salen grises. `unpremultiply` de ffmpeg no funcionó; sí funcionó dividir a mano con `geq` (r*255/alpha).
- Las expresiones (`seedRandom`, `wiggle`, `time*...`) permiten animar decenas de capas sin keyframes.
- Agregar una propiedad a un grupo invalida las referencias guardadas antes ("El objeto no es válido"): buscarlas después de agregar todo.
- `comp.saveFrameToPng` no bloquea: el script termina antes de que aparezcan los PNG, hay que esperarlos.
- Resortes editables: dejar los keyframes como objetivos y que la expresión sume un resorte por keyframe, usando `key(i).value` y `key(i).time`. Moviendo keyframes cambia el video sin tocar la expresión. Ver Motion con código estilo Prompt Motion.

Usado en Motion-graphics-ia. Ver también Blender por script.

## Expresiones que apuntan a otra capa (2026-11-11)
- Si una expresión nombra una capa (`thisComp.layer("Texto")`) y esa capa todavía no existe cuando se pone, AE la evalúa, falla y **la deja deshabilitada** aunque después se cree la capa. Crear primero la capa a la que se apunta y luego la que lleva la expresión (y reordenar con `moveToEnd()`).
- Fondo que se ajusta solo al texto: rectángulo de forma con Tamaño = `sourceRectAtTime()` del texto + relleno, y Posición del rectángulo = la mitad del tamaño. Así, si se cambia el texto en AE, la ficha crece sola. Lo usan `Portada` y `Ficha` del taller 5 v2.
