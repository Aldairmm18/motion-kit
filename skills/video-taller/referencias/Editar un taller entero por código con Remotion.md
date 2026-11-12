# Editar un taller entero por código con Remotion

Aprendido en Talleres-5-6-Efe-Gomez (2026-11-08), cuando el usuario pidió no ver ventanas ni diálogos.

- **Transcribir sin Scribe:** `mlx-whisper` con `mlx-community/whisper-large-v3-turbo` y `word_timestamps=True` corre local en la Mac (≈ 47 min de audio en unos 20 min, junto con otros procesos). Alucina el `initial_prompt` en clips sin voz y repite "¡Gracias!": filtrar esos segmentos.
- **Elegir voces por relación señal/ruido**, no solo por lo que dicen: p90 de la voz contra p10 del clip. Con < 8 dB no sirve; con 13–17 dB sirve con `afftdn` y música debajo.
- **Ventanas quietas** con `phaseCorrelate` en proxies de 288×512 a 25 fps: velocidad media < 120 px/s (escala 1080) y sin picos > 260. En cámara lenta de 100 fps el movimiento visible baja 3×.
- **Tomas pre-renderizadas** (recorte, color con LUT por toma, velocidad con `setpts` + `fps=30`) y Remotion solo compone. La caché de tomas necesita una firma de parámetros: con el mismo id y otra toma, se quedaba la vieja.
- **Color por toma como LUT:** exposición en log (1 paso = 0,077 de código S-Log3) + balance de blancos en lineal + creativo, todo cocinado en un `.cube` de 33³ y aplicado con `lut3d` tetraédrico. No saturar los casi grises: los blancos se van a lila.
- **Bajar varias imágenes del navegador sin el aviso de "varias descargas":** dibujarlas todas en un `<canvas>` y descargar un solo PNG; recortar después. En Brave la descarga queda como `.com.brave.Browser.*` en Descargas mientras espera; el archivo ya está completo.
- **`renderStill` repetido** se cae con "Visited … but got no response": abrir un navegador con `openBrowser` y pasarlo como `puppeteerInstance`.
- Música más corta que el video: repetir un tramo del medio cortado en pulsos (librosa), múltiplo de 8 pulsos, con fundido de 30 ms.

Relacionado: Remotion no renderiza en macOS 27, Método de la línea de historia, Motion graphics editoriales con Claude.
