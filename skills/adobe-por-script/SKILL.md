---
name: adobe-por-script
description: Controla After Effects, Premiere Pro, DaVinci Resolve y Blender por script en Mac (ExtendScript por osascript, plantillas .mogrt, cortes de voz precisos, XML desde Remotion, render en Blender). Úsala cuando haya que armar o cambiar una secuencia, crear plantillas de gráficos, exportar de AE a Premiere o automatizar cualquiera de esos programas.
---

# Adobe, Resolve y Blender por script

Parte de `principios`. Claude no trabaja dentro de los programas: escribe scripts y los ejecuta.

## Cómo se corre
- **After Effects:** `osascript -e 'tell application "Adobe After Effects <año>" to DoScriptFile "/ruta/script.jsx"'` (con `with timeout of 900 seconds` si renderiza). `DoScript` no devuelve nada: el script escribe un log en un archivo y se lee después.
- **Premiere:** ExtendScript por un puente BridgeTalk desde AE. Buscar la secuencia por nombre.
- **Programas en español:** usar matchNames (`ADBE …`), no los nombres visibles.

## Reglas de seguridad con el proyecto de la persona
- No guardar el proyecto ni exportar sin preguntar.
- Antes y después de cada cambio, sacar el estado de la línea de tiempo (pistas, clips, entradas y salidas) y compararlo. Nunca mover lo que la persona editó a mano.
- Revisar exportando cuadros, no suponer.

## Trampas que más cuestan
- Las marcas de la transcripción se corren hasta ~80 ms: cortar dentro del silencio real medido (RMS cada 10 ms, umbral percentil 10 + 8 dB).
- Premiere redondea la entrada a su cuadrícula: leer `trackItem.inPoint` después de poner el clip y corregir.
- `.mogrt`: exponer propiedades con `addToMotionGraphicsTemplateAs`, regiones protegidas con marcadores, y guardar el proyecto de AE antes de exportar. Los deslizadores en Premiere van solo de 0 a 100: para cifras, exponer texto.
- Agregar propiedades invalida referencias guardadas: volver a buscarlas.
- El ProRes de AE viene con alfa premultiplicado: corregirlo o los colores salen grises.
- Premiere no hace rampas de velocidad por script: generarlas con ffmpeg en ProRes.

## Referencias
- `referencias/After Effects por script.md`
- `referencias/After Effects a Premiere con mogrt por script.md`
- `referencias/Cortes de voz precisos en Premiere por script.md`
- `referencias/Remotion a Premiere por XML.md`
- `referencias/DaVinci Resolve App Store por script.md`
- `referencias/Blender por script.md`
- `referencias/Remotion no renderiza en macOS 27.md`
