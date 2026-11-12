# Cortes de voz precisos en Premiere por script

Visto el 2026-10-22 con Premiere 2026 en Segundo-Taller-Efe-Gomez: clips de iPhone a 30 fps en una secuencia de 23,976. Ver también After Effects a Premiere con mogrt por script (el puente BridgeTalk desde AE).

- **Las marcas de Scribe se corren hasta ~80 ms.** "contaminación" seguía sonando 60 ms después de su `end`, y "Entonces" arrancaba 100 ms después de su `start`. Un corte que solo se fía de la transcripción se oye mochado. Hay que medir la energía del audio (RMS cada 10 ms) y cortar dentro del silencio real.
  - El umbral de silencio que funcionó: **percentil 10 de la toma + 8 dB**. Con +6 no encuentra silencios cortos; con +11 toma por silencio las partes suaves de la voz.
  - El silencio correcto es **el que termina donde Scribe marca el inicio de la palabra**, o el que empieza donde marca el final. No sirve "el último antes": a veces hay pausas de 40 ms dentro de una palabra.
  - Margen de 30 ms a cada lado del silencio, hasta 100 ms de respiración antes de la frase y hasta 120 ms de cola.
- **Premiere redondea la entrada** a su cuadrícula: la de la secuencia, desplazada por el código de tiempo del clip, y casi siempre hacia atrás, hasta 42 ms. `projectItem.setInPoint(s, 4)` + `track.overwriteClip` no da la entrada pedida. Solución: leer `trackItem.inPoint` después de poner el clip y, si no cae en su silencio, quitarlo (`remove(false, true)`) y volver a ponerlo un cuadro antes o después.
- **`trackItem.outPoint` no se actualiza al cambiar `trackItem.end`.** La salida real es `inPoint + (end − start)`. Se recorta o alarga con `end` de a un cuadro, y el audio enlazado sigue al video.
- **Fundidos de audio:** `qe.project.getAudioTransitionByName("Potencia constante")` (en la interfaz en español; "Constant Power" en inglés) + `qeItem.addTransition(tr, false, "00;00;00;02", "00;00;00;00", 0.5, false, true)`. **El fundido centrado usa ~40 ms de audio fuera del corte**, así que solo se pone donde sobra silencio a los dos lados. Si no, se cuela la palabra vecina.
- **Volumen por script:** en el componente "Volumen", la propiedad "Nivel" vale 0,1778 para 0 dB. Para G dB: `0.17782794 × 10^(G/20)`.
- **Movimiento > Escala** se copia de la secuencia original; los clips nuevos entran al 100 %.
- **Rearmar sin duplicar:** `app.project.deleteSequence(seq)` y volver a crearla con `secuencia.clone()` vaciada. Antes de importar los PNG al bin, revisar cuáles ya están.
- **zsh** al armar filtros de ffmpeg: `"$i:v"` se interpreta como el modificador `:v` de zsh. Hay que escribir `"${i}:v"`.
- **Nunca modificar `app.project.activeSequence` en un script.** La secuencia activa es la pestaña que el usuario tenga abierta, que puede no ser la tuya. Una prueba así borró clips de la secuencia original. Hay que buscar la secuencia por nombre en `app.project.sequences` y guardar antes una lectura completa (clips, entradas y salidas) para poder restaurarla. La secuencia activa solo sirve para leer, o para QE justo después de asignarla (`app.project.activeSequence = s`).
- **`overwriteClip` en V2 manda el audio enlazado a A2** (la pista con el mismo número), no a A1: el B-roll no pisa la voz.
- **`trackItem.disabled = true`** apaga un clip (los carteles de referencia quedan en la línea de tiempo sin verse).
- **Recorrer clips para borrar: de atrás hacia adelante.** Si se recorre hacia adelante mientras se borra, se salta un clip de cada dos.
- **Cámara lenta por script (QE):** `qeItem.setSpeed(0.5, "", false, false, false)` deja el clip al 50 % **sin cambiar su duración** en la línea de tiempo (muestra la mitad del tramo). `qeItem.setTimeInterpolationType(2)` = flujo óptico. Después, `trackItem.inPoint` se reporta al doble del tiempo real del archivo.
- **`trackItem.move(Time)` corre el clip por un desplazamiento, pero no mueve su audio enlazado:** hay que mover cada pista por separado, de izquierda a derecha si se corre hacia atrás.
- **Premiere ajusta a la cuadrícula de cuadros el inicio de un clip, no su final** (`end` acepta cualquier valor). Si encadenas clips con `end = inicio + d`, quedan huecos de 10–20 ms: alarga cada clip hasta el `start` real del siguiente.
- **Las transiciones no se mueven con `trackItem.move()`:** se quedan en el tiempo viejo, a mitad de un clip, y dibujan negro. Antes de correr clips, anota las transiciones (`track.transitions`: inicio, fin, nombre). Después quítalas (`transicion.remove(false, true)`) y vuelve a ponerlas en el corte con QE: `qeClip.addTransition(qe.project.getVideoTransitionByName("Disolución cruzada"), false, "00;00;02;03", "00;00;00;00", 0.5, false, true)`.
- **`qeTrack.numTransitions` cuenta también los espacios vacíos** (sin nombre); las transiciones reales son las que tienen `name`.
- **Para buscar negros:** `exportFramePNG` cada 0,5 s y medir el brillo de cada cuadro (`scale=1:1,format=gray`). Así apareció un negro que no estaba en ninguna transición: Premiere dibujaba negros los primeros ~0,85 s de un clip que arrancaba en cierto punto del archivo, aunque el mismo segundo se veía bien a mitad de otro clip. Se tapó con B-roll.
- **`.mogrt` exportado desde AE con el nombre interno "Sin título" → `importMGT` devuelve null.** Hay que poner `comp.motionGraphicsTemplateName` **apenas se crea la comp, antes de `addToMotionGraphicsTemplateAs`**.
- **`importMGT(ruta, ticks, pistaVideo, pistaAudio)`** + `trackItem.getMGTComponent().properties`. El texto es un JSON en texto: se cambian `"textEditValue"` y `"fontTextRunLength"`. La posición va normalizada `[x/ancho, y/alto]`.
- **Incrustación por pista mate (Premiere en español):** QE `getVideoEffectByName("Incrustación por pista mate")`. Sus campos son `"Mate:"` (6 = Vídeo 6), `"Compuesto usando:"` (0 = mate alfa) y uno sin nombre (Invertir). **La pista del mate tiene que estar visible:** si se oculta, la incrustación queda sin mate y el clip se ve completo. Premiere no dibuja el mate.
- **Texto detrás de la persona en Premiere:** V1 plano · V4 texto · V5 copia del mismo plano con la incrustación · V6 máscara ProRes 4444 con alfa. La copia tiene que calzar cuadro a cuadro con V1: comparar `inPoint − start` de los dos y, si no da 0, correr el inicio un cuadro.
- **`trackItem.inPoint = t` y `outPoint = t` deslizan el contenido** (slip) sin mover el clip.
- **Bloquear pistas de audio antes de `overwriteClip` no evita el audio:** Premiere crea pistas de audio nuevas y lo pone ahí. Hay que revisar las pistas al final y quitar esas copias.
- **No poner margen en `overwriteClip` (+0,2 s) cuando hay clips vecinos:** pisa el comienzo del clip siguiente, y aunque después se recorte el `end`, lo pisado no vuelve.
- **ffmpeg `alphamerge` con `-f lavfi color=…` no termina** aunque se use `-shortest` dentro de `-filter_complex`: generó 37 GB. Hay que sacar el blanco del mismo video (`split` + `lutrgb`) y poner `-frames:v N`.


## Más trampas (Cali, 2026-10-22)
- **`projectItem.setInPoint(t, 1)` (solo video) + `overwriteClip`** pone el clip en **destino + entrada**, no en el destino. Con `4` (todos los medios) cae bien. El audio que se cuela se quita después.
- **Un .mogrt entra con su duración por defecto (~6 s)** y pisa lo que haya después en la pista, aunque luego se recorte. Hay que poner las plantillas **en orden cronológico** y recortar cada una antes de poner la siguiente.
- **`marker.end` pide segundos (número)**, no un `Time`: con `Time` da "Illegal Parameter type".
- **El "Tamaño (%)" de las plantillas no pasa de 100** (los deslizadores de Premiere van de 0 a 100). Para un texto más grande se usa otra plantilla, o se calcula el tamaño para que quepa (ancho medido con PIL).
- **Secuencia vertical desde un clip 4K HLG:** `createNewSequenceFromClips` y luego `setSettings` con 1080×1920, `videoFrameRate.ticks = "8467200000"` (30 fps), Rec. 709 y asignación de tonos automática. Los clips van con Escala 50.
- **Cada `addTracks` de QE y cada copia con audio suman pistas de audio vacías**, que se acumulan al repetir el script.
- **Cambios puntuales cuando el usuario ya editó a mano:** antes de tocar, guardar una lectura de la secuencia. Rehacer solo el tramo (quitar los clips cuyo inicio cae dentro y volver a ponerlos) y después comparar todo lo que está fuera del tramo. Ojo: **una imagen fija entra con su duración por defecto (~5 s)** y pisa lo que haya después en la pista antes de recortarse; así se perdió un destello que estaba fuera del tramo.
- **Reemplazar una plantilla conservando el ajuste a mano:** antes de quitarla, leer todas las propiedades de Movimiento (`AE.ADBE Motion`) y aplicarlas al clip nuevo.
- **Claves en propiedades de un clip:** el tiempo va en el tiempo del clip, `inPoint + (t − start)`. En un .mogrt el `inPoint` arranca en 3600 s.
- **"Desenfoque gaussiano" en Premiere 2026** es `AE.Impact_Blur_FX`, y la intensidad se llama **"Cantidad"** (no "Desenfoque").
- **`exportFramePNG` trae canal alfa:** donde no hay imagen sale transparente (por ejemplo, durante un fundido a negro). Para revisar hay que componer sobre negro, porque si se convierte a RGB sin más, el fundido parece no oscurecer.
- **Abrir espacio en medio de una secuencia ya editada:** recorrer todas las pistas de atrás hacia adelante y hacer `move()` a cada clip que empiece en el punto o después; correr también los marcadores (`marker.start` y `marker.end` en segundos). Las claves de los efectos viajan con su clip. Ojo con los clips que arrancan un poco antes del punto a propósito (un obturador 20 ms antes del corte): hay que moverlos a mano. Después, comparar contra una lectura previa: lo anterior idéntico y lo posterior corrido exactamente lo pedido.
- **Siluetas de personas reales** para gráficos (en vez de IA): un cuadro, la máscara de Apple Vision (`segmentar`), quedarse solo con la pieza más grande (`scipy.ndimage.label`) y rellenar con tinta plana. Lo que la toma tapa (los pies) se esconde detrás de una tira de papel.
- **Quitar un efecto de un clip por script:** QE solo tiene `qeItem.removeEffects()`, que quita **todos** los efectos agregados al clip (Movimiento y Opacidad quedan). Si el clip tenía otro efecto que se quiere conservar (una incrustación por pista mate), hay que volver a ponerlo después con `addVideoEffect`. Antes de quitar, revisar qué efectos tiene cada clip y no tocar los que tengan efectos puestos por el usuario. Así se quitó el look Night Shift en un reel político: el color va en una capa de ajuste arriba, nunca en los clips.
- **Una carpeta de Premiere en iCloud que se renombra a `.NOSYNC`** deja de sincronizarse. Los scripts que buscan el proyecto por una parte del nombre (`p.path.indexOf("CALI TALENTO VISIBLE")`) siguen funcionando. Los que tienen la ruta completa escrita quedan apuntando a la vieja.


## Más trampas (Miramar, 2026-10-24)
- **`overwriteClip` en una pista de audio también pone el video** del mismo clip en la pista de video con el mismo número (voz en A2 → su imagen en V2, encima del montaje). Después de poner voces "solo audio", quitar lo que haya caído en esa pista de video. Y poner después las capas propias de esa pista (fondos), o las parte.
- **`exportFramePNG` de QE en una secuencia creada desde un clip de 50 fps** (FX3) aunque se pase a 30 fps: lee los **segundos del código de tiempo a 50 cuadros y los cuadros a 30**. "00:00:13:10" mostraba el segundo 22,0. Cuadro N → `SS = N div 50`, `FF = N mod 50`. Se calibra con un destello de 2 cuadros en un tiempo conocido.
- **`sequence.exportAsMediaDirect(ruta, preset.epr, 0)`** exporta la secuencia entera sin abrir Media Encoder. Sirve para revisar audio y video juntos. Preset: `…/Adobe Media Encoder 2026.app/Contents/MediaIO/systempresets/4E49434B_48323634/00 - Match Source - High bitrate.epr`.
- **Claves de opacidad en una imagen fija o un .mogrt:** el `inPoint` arranca en 3600 s; la clave va en `inPoint + (t − start)`. En un clip con `setSpeed` las claves no caían donde se pedía: el fundido final se hizo con un PNG negro arriba.
- **Voces igualadas con tope de pico:** ganancia = mín(−18 LUFS − sonoridad, −3 dBFS − pico). Solo con la sonoridad, un golpe de micrófono pasó de 0 dBTP.
- **Rampa de velocidad por script:** no hay remapeo de tiempo en ExtendScript; se parte el plano en dos tramos (`setSpeed(1.6)` y `setSpeed(0.5)`) sobre el tiempo de la música.
- **Claves en un clip con velocidad cambiada (`setSpeed`):** `inPoint` y `outPoint` se reportan divididos por la velocidad, pero las claves de los efectos van en **tiempo real del archivo**: `(inPoint + (t − start)) × velocidad`. Con `inPoint + (t − start)` las claves caían después del final del clip y el efecto no se veía (pasó con el fundido y con el desenfoque del último plano lento de Miramar). Se comprueba midiendo la nitidez (varianza del laplaciano) de cuadros exportados.
- **"Desenfoque gaussiano" (AE.Impact_Blur_FX):** "Cantidad" 4 ya desenfoca mucho en 1080×1920; el ciclo de entrada y salida del reel usa 4 en 0,75 s.
- **Color de Lumetri por script (Premiere 2026 en español):** se agrega a la capa de ajuste con QE `addVideoEffect(getVideoEffectByName("Color de Lumetri"))` y se ajusta por `displayName` en el componente: Corrección básica → "Saturación" (100 = normal; el primer "Saturación" de la lista es el de Corrección básica, el segundo es el de Creativo), "Exposición", "Contraste", "**Resaltados**" (no "Iluminaciones"), "Sombras", "Blancos", "Negros". La capa de ajuste la crea el usuario (Archivo > Nuevo > Capa de ajuste, con el panel Proyecto activo); el script la encuentra con `projectItem.isAdjustmentLayer()`.


## B-roll sin huecos (2026-10-25)
- `overwriteClip` con material de más pisa el inicio de la toma siguiente. Si después se recorta la toma nueva, queda un hueco de 1–2 cuadros (destello de la entrevista o negro). Hay que insertar con el material exacto, o devolver el inicio de la siguiente.
- Los tiempos deben caer en cuadros exactos de la secuencia (23,976: múltiplos de 1001/24000). Si se suman duraciones desde un tiempo que no está en la grilla, quedan micro huecos entre los clips de una ráfaga o de una rampa.
- Después de cada script, se lista V2 y se comprueba que las tomas seguidas queden pegadas.
- **Detectar tomas temblorosas o con zoom sin OpenCV:** correlación de fase (numpy FFT) entre cuadros a 12,5/s y 480×270, en el cuadro entero y en sus mitades. Temblor = desviación del movimiento menos su media móvil; zoom = las mitades izquierda/derecha (y arriba/abajo) se separan. Sirve para elegir el tramo más quieto de cada clip (ver Miramar-Primer-Taller).
- **Sony FX3 en S-Log3:** Premiere 2026 la detecta (`projectItem.getColorSpace()` → "Sony S-Log3/S-Gamut3.Cine") y la convierte sola a Rec. 709. Un LUT de Sony encima la convierte dos veces.
- **Plantillas .mogrt verticales en una secuencia horizontal:** el lienzo de la plantilla queda centrado y lo que sale de él se recorta. Con un lienzo 1080×1920 en 1920×1080: posición normalizada y = (420 + y_deseada)/1920 y x en [0, 1080] → x del cuadro = 420 + x.
- **LUT de entrada de Lumetri no se puede poner por ExtendScript** ("Illegal Parameter type" con la ruta) y un .cube nuevo no sale en la lista hasta reabrir Premiere. Se pone con clics: Lumetri > Corrección básica > LUT de entrada > Examinar… (en el diálogo, Cmd+Shift+G y la ruta). Para que el LUT haga la conversión de log: `projectItem.setOverrideColorSpace(<Rec. 709 de workingColorSpaceList>)` en los clips, si no Premiere ya los convirtió.


## Más trampas (tercer taller, 2026-10-31)
- **Clips verticales de la FX3:** ffprobe dice 3840×2160 con `rotation=90` y ffmpeg los entrega ya rotados. Un `scale=480:270` fijo los aplasta y deforma las medidas de movimiento: revisar la rotación clip por clip antes de medir. En una misma tarjeta puede haber uno horizontal.
- **`setOutPoint(t, 4)` + `overwriteClip` + `clip.end`:** alargar `end` del video no siempre alarga el audio enlazado: en 59 de 158 clips A1 quedó un cuadro más corto. Después de armar, comparar V1 y A1 e igualar `end` de atrás hacia adelante.
- **Abrir una secuencia en la línea de tiempo:** `app.project.openSequence(s.sequenceID)`; `sequence.openInTimeline` no existe en la 26.5.

## Más trampas (reel del tercer taller, 2026-11-01)
- **`getOverrideColorSpaceList` es una propiedad, no un método**, y sus elementos son objetos `ColorSpace` (se comparan por `.name`). `setOverrideColorSpace` pide el objeto, no el texto: con la cadena no hace nada y no da error.
- **Slow ramp que se ve igual al FX3 original:** ProRes con los códigos S-Log3 sin tocar (`setparams=range=pc`) y en Premiere `setOverrideColorSpace(clipOriginal.getOriginalColorSpace())`.
- **Las marcas de Scribe pueden durar 0:** filtrar palabras por duración se come "a", "de", "la". Revisar el texto final de los subtítulos contra lo dicho.
- **Cortar "al silencio más cercano" hacia adelante puede arrastrar palabras** ("…la barrera de buena"). Leer la lista de subtítulos sirve para detectarlo.
- **`exportFramePNG` en una secuencia clonada a 30 fps** usa código de tiempo normal: `SS = N div 30`, `FF = N mod 30`. Calibrar con un plano conocido antes de confiar.
- **Una voz más corta que el plano visible deja negro**: al final, recorrer los huecos de V2 y alargar el video de V1 hasta cubrirlos.


## Código de tiempo de QE en secuencias creadas desde un clip de 50 fps
Si la secuencia se crea con `createNewSequenceFromClips` desde un clip de 50 fps y luego se pasa a 30 fps, QE sigue escribiendo el código de tiempo como segundos ×0,6 con cuadros de 50 (56,2 s aparece como `00:00:33:36`). Para `exportFramePNG`, armar el código así: `v = t·0,6; ss = floor(v); ff = round((v − ss)·50)`. Con cuadros de 30 los PNG salen hasta 0,5 s antes. La API normal (`start.seconds`) sí da los segundos reales. Visto el 2026-11-04 en Miramar-Segundo-Taller.

## Más trampas (talleres 5 y 6 de Efe Gómez, 2026-11-08)
- **`setSettings` con `audioSampleRate = 48000` deja la secuencia muda** al exportar. Es un `Time` (ticks por muestra): 48 kHz = `t.ticks = "5292000"`.
- **"Eliminar ruido" agregado con QE `addAudioEffect` tumba Premiere 26.5** (falla al crear el filtro de Accusonus). Se pone a mano.
- **Ganancia de más de +15 dB por script:** "Volumen > Nivel" llega a +15; "Volumen del canal" (Izquierda/Derecha, misma escala 0,1778 = 0 dB) suma otros +15, y un segundo efecto "Volumen" otros +15. "Amplificación" no toma dB (sus controles van normalizados).
- **Subclips solo video o solo audio:** `item.createSubClip(nombre, ini, fin, 0, video, audio)` con `(…,1,0)` o `(…,0,1)`; así `overwriteClip` no deja audio ni video sueltos. Las claves de un clip con velocidad van en `inPoint + (t − start)` (el `inPoint` ya viene dividido por la velocidad).
- **`importAEComps` desde el puente de AE:** si AE espera la respuesta, se traban. Mandar el BridgeTalk **sin esperar** y que Premiere escriba la respuesta en un archivo. Las comps entran como "Nombre/proyecto.aep".
- **Si se cierra el servidor de Dynamic Link de AE** (`aerendercore -livelink`), Premiere ve las comps "sin conexión" hasta que se reinicia Premiere.
- **Claves de volumen de la música:** sacarlas de la curva mínima de todas las bajadas; si se ponen bajada/subida por voz y dos voces quedan cerca, se cruzan y la música sube encima de la segunda voz.
- **Color por toma igual a una referencia:** exportar el cuadro del medio de cada toma (QE) y corregir en Lumetri Exposición (log2 de la mediana), Temperatura (R−B), Blancos/Negros (p97/p3) y Saturación; converge en 4–6 pasadas.

## Más trampas (reel político, 2026-11-10)
- **Colgar capas de un null en AE por script cuando el null ya tiene escala animada:** AE compensa a las hijas con la escala del cuadro actual (quedaron al 80 %) y el bloque terracota salió corrido y angosto. Primero se cuelgan las capas y después se anima el padre.
- **Premiere guarda en caché un .mogrt por su nombre interno:** si cambias la plantilla y la exportas con el mismo nombre, puede entrar la vieja. Exportarla con otro nombre (v2).
- **`$.global` de Premiere sobrevive entre scripts:** una variable de filtro (`JF_SOLO`) de una prueba anterior dejó armar solo una secuencia. Hay que limpiarla (`= undefined`) al empezar.
- **`DoScript`/`DoScriptFile` de AE por osascript devuelve 0 si salió bien y 1 si el script falló** (sin mensaje): mirar el log que escribe el propio script.
- **Una palabra pegada a la siguiente (sin silencio) no se puede cortar "al silencio más cercano":** el mínimo de energía cae dentro de la palabra y se oye mochada. Hay que cortar fijo al final de la palabra con un fundido de 50 ms y dar la pausa con la imagen (el video sigue, el audio no). Premiere además corre la entrada ~27 ms hacia atrás: sumar un cuadro al audio.
