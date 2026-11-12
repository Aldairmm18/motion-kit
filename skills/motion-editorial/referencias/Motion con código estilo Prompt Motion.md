---
tipo: aprendizaje
fuente: https://prompt-motion.com
fecha: 2026-10-31
---

# Motion con código estilo Prompt Motion

prompt-motion.com es una galería de 230+ videos hechos con Claude Opus 5.5, cada uno con su prompt. No es una técnica secreta: es código que dibuja cada cuadro más un encargo bien dirigido. Mismo prompt corto repetido 33 veces: de 0 a 17 000 likes, porque deja todo al azar.

## Las cinco partes de un buen encargo
1. **Un solo concepto**, nombrado en una línea ("una forma, nunca se corta"). Cada plano nace del anterior; sin eso sale un catálogo de efectos.
2. **Lista de prohibidos**, con la alternativa: brillos, partículas, crossfades, rebote exagerado, textos decorativos en las esquinas, tiempo muerto, "cualquier cosa que parezca plantilla".
3. **Rejilla de tiempo**: BPM y compases, algo pasa en cada golpe. 120 BPM es lo habitual.
4. **Método de render** explícito: todo es función pura del tiempo, sin timers ni estado, y números aleatorios con semilla.
5. **Revisión**: renderizar un cuadro por golpe, mirarlo y corregir antes del render completo.

## Trucos técnicos
- Resortes en forma cerrada. Un valor que cambia de objetivo varias veces es la suma de un resorte por cambio, así sigue siendo función del tiempo.
- Bordes de un indicador en resortes distintos: el borde delantero se estira antes que el trasero.
- Desenfoque de movimiento: 3-4 subcuadros por cuadro mezclados con `tmix` de ffmpeg.
- El texto que cambia dentro de un contenedor que se transforma necesita entrada y salida propias, o se pisa.
- Nada de `will-change` en lo que la cámara escala: el texto queda borroso.
- Para loop, el último cuadro debe igualar al primero, incluida la velocidad del cursor.

## Aplicado a Remotion
`useCurrentFrame` ya da el `seek(t)` que esos prompts piden a mano, así que no hace falta cambiar de herramienta. Plantillas en la web: [showreel](https://prompt-motion.com/templates/showreel) y [3D](https://prompt-motion.com/templates/realtime).

Ver Motion-graphics-ia y Método de la línea de historia.
