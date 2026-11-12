# Subtítulos dinámicos y texto detrás de personas

Análisis de 5 TikToks de referencia (2026-10-16), cuadro a cuadro en el navegador, sin descargar:

| # | Creador | Qué muestra |
|---|---|---|
| 1 | @jeronimoesquivel1 | Tutorial de subtítulos dinámicos en CapCut (talking head, luz cálida) |
| 2 | @studioznak | Preset de kinetic typography en After Effects (fondo blanco) |
| 3 | @higinio.ediciondevideo | Antes/después para una clínica odontológica (Premiere) |
| 4 | @migs.visuals | Captions cinematográficos con texto detrás de la persona |
| 5 | @pradesvisuals | "3 pasos" para subtítulos profesionales (estudio oscuro, glow morado) |

## 1. Posición
- **A la altura del pecho, no abajo:** el bloque va entre el 50 % y el 65 % de la altura (en 1080×1920, y ≈ 950–1250), centrado o alineado al lado del hablante. El ojo no viaja lejos de la cara y queda dentro de la zona segura de Reels/TikTok.
- **Alternativa cinematográfica (4):** tercio superior, en el espacio vacío junto a la cabeza.
- **Sin cajas ni barras de fondo.** La legibilidad sale de un gradeo oscuro, una viñeta, un degradado inferior (3) o glow y sombra.

## 2. Cómo se arma cada bloque
- **1–4 palabras por bloque, 2–3 líneas.** Las palabras aparecen **justo cuando se dicen** y se acumulan; al cerrar la idea se borra el bloque entero y empieza otro.
- **Jerarquía de dos niveles:**
  - **Conectores** pequeños: sans bold, en minúscula o tipo oración ("¿Eres de", "y te dijeron que hay que", "why do many").
  - **Palabra clave** 2–3 veces más grande, en una de dos variantes:
    - **Sans heavy en mayúsculas** (CHILLÁN, SACAR, NO, CREATORS, BIG, DYNAMIC).
    - **Serif italic o script en color** (*subtítulos*, *palabras*, *muelita?*, *crazy text*, *DM Serif*).
- **Interlineado muy apretado:** las líneas casi se tocan y a veces se montan.
- **Alineación escalonada** (5): línea 1 serif italic grande hacia la izquierda, línea 2 bold al centro, línea 3 pequeña hacia la derecha. Forma una "escalera" diagonal.
- **Números gigantes** para estructurar listas ("3 simples pasos", "1 El texto principal").

## 3. Tipografías (máximo 2 por video)
- **Parejas que nombran ellos mismos:**
  - Montserrat + Vivaldi.
  - Coolvetica + Simphonie Pro.
  - Poppins / Montserrat / Helvetica + DM Serif.
- **Patrón:** sans geométrica o grotesca **bold** + serif italic **elegante** o script.
- **En macOS sin descargar nada:**
  - **Sans:** Avenir Next Heavy/Bold, Helvetica Neue Bold.
  - **Serif italic:** New York Italic, Didot Italic, Bodoni 72 Italic.
  - **Condensada:** DIN Condensed, Futura Condensed ExtraBold.

## 4. Color
- **Base blanca y un solo color de acento**, elegido por relación con la imagen:
  - **Complementario al gradeo:** teal sobre luz cálida (1).
  - **Análogo al gradeo:** naranja y amarillo sobre luz cálida (4), con **rojo** solo para el "problema".
  - **De marca:** rosa de la clínica (3), como **barra resaltadora** detrás de la palabra en script.
  - **Neón sobre fondo neutro oscuro:** morado (5).
- Uno de los videos (1) **advierte contra usar muchos colores**: lo muestra como mal ejemplo.

## 5. Animación
| Tipo | Cómo se ve | Duración |
|---|---|---|
| Corte seco palabra a palabra (1, 4) | la palabra aparece entera en su sílaba | 1 cuadro |
| Pop + glow (4, 5) | entra al ~108 % y baja a 100 %; el glow empieza intenso y se asienta | 0,10–0,15 s |
| Blur + fade + subida (2) | cada palabra o letra entra desenfocada, gris y 10 px abajo hasta quedar nítida; la línea se re-centra deslizándose | ~0,2 s por palabra, 0,05 s entre letras |
| Ola de color (2) | cada letra entra en tinte (rosa) y vira al color base | 0,05 s entre letras |
| Latigazo con motion blur (3) | la palabra llega con desenfoque direccional | 2–3 cuadros |

## 6. Texto e imágenes detrás de la persona
- La palabra clave grande va **detrás de la cabeza o el cuerpo** ("crazy text", "BIG", "this" en 4; "subtítulos" detrás del micrófono en 1). Da profundidad y se ve "producido".
- **Técnica:** máscara de persona (CapCut "quitar fondo", AE Rotobrush). La capa va: fondo → texto → persona recortada.
- **Regla:** que la persona tape como máximo ~30 % de la palabra, para que siga legible. Funciona mejor con palabras gigantes y fuentes heavy.
- **Local en Mac:** Apple Vision `VNGeneratePersonSegmentationRequest` (Swift, sin descargar modelos) genera la máscara por cuadro.

## 7. Otros recursos gráficos vistos
- **Stickers 3D** que hacen pop (diente en 3) junto a la palabra.
- **Círculo a mano en rojo** alrededor de la palabra clave (5).
- **Zoom-out:** el plano se encoge a tarjeta y el texto aparece al lado sobre un degradado del color de acento (5).
- **Mockup de teléfono** sobre blanco para mostrar pasos de un tutorial (1).
- **Logo pequeño** de marca de agua arriba al centro (3).

Relacionado: Motion graphics institucionales y voz sin cortes, Estilo de edición Sueños Urbanos, Video-use.

## Aplicado (Biciclick v3, 2026-10-16)
- **Máscara de persona en Mac:** el script Swift con `VNGeneratePersonSegmentationRequest` (`.accurate`) toma el video y entrega cuadros grises crudos. Luego ffmpeg aplica `tmix` (3 cuadros) y `gblur` 1,6 y lo guarda en ffv1.
- **Composición:** `[base][texto]overlay` y luego `[base][máscara]alphamerge` encima, así la persona vuelve a quedar delante del texto.
- **Posición automática:** se recorre y = 330–920 y se elige el punto donde la máscara tapa ~22 % de la palabra.
- **Ojo con el color de acento:** si coincide con el fondo (rojo sobre adoquín rojo) se pierde. Solución: la palabra en blanco sobre una barra del color.
- **Palabras blancas sobre cielo o paredes claras:** contorno oscuro translúcido (3 px, 42 %) más sombra suave.
- **No duplicar énfasis:** si un gráfico ya muestra la cifra (contador 600), en el subtítulo va normal.

## Aplicado (Talento Visible Cali, 2026-10-17)
- **Posición automática como script:** `cali-recap/scripts/ubicar_palabras.py` lee el alfa del recorte (VP9 con alfa, decodificado con `-c:v libvpx-vp9` + `alphaextract`) en el tramo de la palabra, dibuja la palabra con PIL (Avenir Next Heavy = índice 8 de `Avenir Next.ttc`; New York con `set_variation_by_name("Bold Italic")`) y busca la y con ~22 % tapado.
- **En primeros planos no hay hueco en el pecho:** el cuerpo tapa más del 30 % en cualquier altura bajo la cara. La palabra va arriba, detrás del pelo, y lo demás (la pregunta, los subtítulos) baja al pecho.
- **Títulos de dos líneas:** solo la línea grande va detrás; la segunda va delante, porque el cuello y los hombros la tapan entera.
- **Datos personales en escarapelas y certificados:** tapar la cédula con `boxblur` sobre la zona (crop + overlay en ffmpeg) y usar solo el tramo en que la tarjeta está quieta.
