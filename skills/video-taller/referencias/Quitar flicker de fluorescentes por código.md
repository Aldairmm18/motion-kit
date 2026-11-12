---
tipo: aprendizaje
tags: [video, color, python, ffmpeg]
---
# Quitar flicker de fluorescentes por código

**El problema.** Tubos fluorescentes con red de 60 Hz (parpadean a 120 Hz) grabados a 50 fps (o 25) con obturador de cortinilla: salen **bandas horizontales que suben o bajan** por el cuadro. Los usuarios lo llaman "stuttering"; no es un problema de cuadros. Un LUT con contraste lo hace más visible. Premiere no trae deflicker nativo (el plugin Flicker Free es de pago).

**Cómo evitarlo al grabar.** Obturador a 1/60 o 1/120 (múltiplo de la red), o 30/60 fps en Colombia.

**Cómo se corrige (Python + numpy + scipy + ffmpeg), probado en el Miramar-Segundo-Taller:**
1. Decodificar el tramo con ffmpeg a `yuv444p10le` crudo. Con S-Log3 de la FX3 (`color_range=pc`) se lee con `setparams=range=pc`. Se puede bajar a 1920×1080 para ahorrar disco si el plano va a escala 50 en una secuencia 1080.
2. Para cada cuadro, la **media de cada fila del Y** (perfil vertical de luz).
3. Una referencia sin flicker: la **mediana temporal de ±3 cuadros** de esos perfiles (`median_filter`, tamaño 7 en el tiempo).
4. Diferencia = perfil − referencia, **suavizada a lo largo de las filas** con una gaussiana (σ = 24 px a 2160 de alto, proporcional a la altura). Así solo se quita la banda ancha y no el detalle.
5. Restar esa diferencia a cada fila del Y. U y V no se tocan, así que el color queda igual.
6. Codificar a ProRes 422 HQ (`prores_ks -profile:v 3`), 10 bit, misma cadencia (50 fps). Se convierte el rango con `scale=in_range=pc:out_range=tv` y se marca `-color_range tv`.

**Resultados.** El parpadeo por fila bajó de 1,6–3,6 niveles a 0,3–0,7, con la luz media igual (±0,5/255). En Premiere, el archivo corregido y el original con el mismo LUT dan cuadros idénticos (diferencia media 0,002).

**Para ponerlo en Premiere.** Se reemplaza cada clip copiando escala, posición, rotación y anclaje (con escala ×2 si el archivo es de 1080p), la misma velocidad por QE y la fuente desde el inicio del archivo. Hay que interpretarlo con el mismo espacio de color que el bruto. Un archivo de 1080p agrandado más allá del 100 % pierde nitidez: los planos que van agrandados se sacan en 4K.

**Costo.** ~200 MB por segundo de 4K en ProRes HQ; 49 s de fuente a 1080p ocuparon 3,5 GB.
