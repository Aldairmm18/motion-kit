# After Effects a Premiere con .mogrt por script

Probado el 2026-10-17 con AE 26.5 (en español) y Premiere 26.5.2. Piloto: Nombre, Contador y Palabra detrás de Biciclick (Motion-graphics-ia). Ver también After Effects por script.

## Crear la plantilla en AE (ExtendScript vía `osascript … DoScriptFile`)
- **Exponer propiedades:** `propiedad.addToMotionGraphicsTemplateAs(comp, "Nombre visible")`. Sirve para Source Text, Color Control, Point Control y deslizadores.
- **Regiones protegidas:** `var m = new MarkerValue("Entrada"); m.duration = …; m.protectedRegion = true;` en `comp.markerProperty`. Así, en Premiere se alarga la duración sin deformar la entrada ni la salida.
- **Exportar:** `comp.motionGraphicsTemplateName = "…"` + `comp.exportAsMotionGraphicsTemplate(true, CARPETA)`. **El 2.º argumento es una carpeta** y el archivo toma el nombre de la plantilla.

## Trampas
- **Ruta con `~`:** `exportAsMotionGraphicsTemplate` no expande `~` y crea una carpeta literal llamada "~". Usar `new Folder("~/…").fsName`.
- **Proyecto sin guardar:** exportar exige el proyecto guardado. Si no lo está, AE lo guarda solo como "Proyecto sin título.aep" en Documentos. Hay que guardarlo antes en su ruta.
- **Referencias inválidas:** después de exportar, las variables `proj` y `comp` quedan inválidas ("El objeto no es válido"). Usar `app.project` cada vez y buscar las comps por nombre.
- **`saveFrameToPng`:** falla. Para vistas previas, usar la cola de render con un solo cuadro (`renderQueue.items.add`, no `renderQueue.add`).
- **Deslizadores en Premiere:** los de Gráficos esenciales van solo de 0 a 100. Una cifra como 600 queda en 100. Para números, exponer un **texto** y convertirlo con `parseFloat` en la expresión.
- **Agregar una propiedad invalida las referencias hermanas:** si guardas el rectángulo de un grupo de forma y luego le agregas un relleno, la variable del rectángulo queda inválida ("El objeto no es válido"). Hay que poner las expresiones antes de agregar más propiedades, o volver a buscarlas.
- **Orden de los grupos de forma:** el primer grupo de la capa se dibuja encima. Un grupo agregado después queda debajo. Para subirlo: `grupo.moveTo(1)`.
- **Exportar es lento la primera vez:** con AE recién abierto, 4 plantillas tardaron 9 min. Después, 6 plantillas tardaron 1 min. Conviene que el script salte las que ya tienen `.mogrt`, para poder seguir después de un error.
- **New York (fuente del sistema):** no aparece en AE. Alternativas instaladas: Baskerville, Charter, Georgia Bold Italic.

## Insertar en Premiere desde AE (BridgeTalk)
- **El puente:** `BridgeTalk.getSpecifier("premierepro")` → `premierepro-26.0`. Se envía `$.evalFile(…)` con el script de Premiere y se espera con `BridgeTalk.pump()` + `$.sleep`.
- **Insertar la plantilla:** `secuencia.importMGT(ruta, ticks, pistaVideo, pistaAudio)` (254016000000 ticks por segundo). `item.getMGTComponent().properties` lista los campos editables.
- **Duplicar una secuencia:** `secuencia.clone()`, buscar la secuencia nueva por su `sequenceID` y renombrarla.
- **Cambiar los campos por script:** `campo.getValue()` de un texto devuelve un JSON en texto. Se cambian `"textEditValue"` y `"fontTextRunLength":[largo]` con reemplazos de texto y luego `campo.setValue(texto, true)`. Los puntos van normalizados: `[x/ancho, y/alto]`. Los números se ponen directo.
- **Duración:** `var fin = new Time(); fin.ticks = "…"; item.end = fin;` alarga o acorta la instancia. Las regiones protegidas mantienen la entrada y la salida.
- **Reemplazar sin pisar:** primero se quitan todos los clips que se van a cambiar y después se insertan las plantillas. Así, una plantilla más larga que su hueco no pisa al clip siguiente. Antes de quitar un clip, se revisa que su nombre sea el esperado.
- **Exportar un cuadro:** `Sequence.exportFramePNG` no existe en esta versión. Usar `app.enableQE(); qe.project.getActiveSequence().exportFramePNG("00;00;SS;FF", ruta)`.

## Más trampas (Financiación Territorial, 2026-10-17)
- **`var C = thisComp.layer("Controles").effect;` no funciona:** al llamarla después, `C("Nombre")` dice que el efecto no existe. Hay que envolverla en una función: `function C(n){return thisComp.layer("Controles").effect(n);}`.
- **Expresiones que nombran capas creadas después:** quedan desactivadas con error. Al final del script, vuelve a asignar las que tengan `expressionError` (`p.expression = ""; p.expression = x;`).
- **Propiedades de color (relleno, trazo, efecto Relleno):** piden 4 valores. Un Color Control devuelve 4, pero `setFillColor` de texto pide 3.
- **Hijos de un null:** las expresiones de posición van en coordenadas del padre, no de la comp.
- **Estilo por carácter (AE 26):** `style.setText(t).setFont(..).setFontSize(..).setFillColor(color, inicio, largo)` en el Texto de origen. Con esto, `*palabra*` sale en otro color, y en Premiere el campo sigue siendo editable con los asteriscos.
- **En Premiere, los menús desplegables van desde 0.**
- **Colores en Premiere:** `setValue(número)` corta el entero de 64 bits a 32 y el color sale mal (cian). Usar `prop.setColorValue(255, r, g, b, true)`.
- **Casillas en Premiere:** en la expresión, `C("casilla")(1) ? a : b` da siempre verdadero, aunque `*100` sí da 0. Usar `Number(C("casilla")(1)) > 0.5`.
- **Sin diálogo:** `app.newProject(ruta)` crea el proyecto, `importFiles([xml])` arma la secuencia y `qe…exportFramePNG` exporta cuadros para comparar.

## Dynamic Link por script: se traba (2026-10-20)
- **No usar `app.project.importAEComps(...)` ni importar un `.aep` en Premiere a través del puente de AE (BridgeTalk).** Premiere espera a AE para leer las comps (`DynamicLinkMedia::GetCompsInfoForAEP`), y AE está ocupado ejecutando el script que espera a Premiere. Las dos apps se traban y Premiere se cae (SIGABRT). Le pasó a JAC-Efe-Gomez.
- **Alternativa:** renderizar la comp en AE a ProRes 4444 con alfa ("Calidad alta con Alfa") e importar el `.mov` con `importFiles`. La comp editable sigue en el `.aep`, y si se quiere el enlace vivo, se importa a mano (Archivo > Adobe Dynamic Link).
- **Al enganchar capas a una capa animada por expresión,** primero se engancha (`layer.parent = …`) y después se pone la expresión. Si no, los hijos quedan corridos lo que la capa madre esté desplazada en el cuadro 0.


## Más trampas (collage "Expediente", Cali, 2026-10-22)
- **Enganchar capas a un null y después ponerles posición:** la posición queda en coordenadas del null, no de la comp, y todo sale corrido (con el ancla del null en [50,50], se corrió casi media pantalla). Arreglo: poner el **ancla y la posición del null en el centro de la comp** ([540,960] en 1080×1920). Así las coordenadas del null coinciden con las de la comp y se puede animar el null para mover todo el collage. Tampoco enganchar una capa a otra después de ubicarla: la foto se salió del marco por eso.
- **Relleno transparente en una forma:** el color `[0,0,0,0]` sale negro. Hay que poner `ADBE Vector Fill Opacity` en 0.
- **Aviso al exportar "Las siguientes fuentes no se sincronizaron desde Adobe":** sale con cualquier fuente del sistema (también con Arial). Se le da Aceptar y el .mogrt funciona en este Mac. Solo falla en otro equipo que no tenga la fuente. Para compartir, usar Adobe Fonts.
- **Premiere guarda en caché las plantillas por nombre:** al rehacer un .mogrt hay que cambiarle el nombre (v2, v3…) o Premiere sigue usando la vieja.
- **Máquina de escribir:** animador de texto con Opacidad 0 y un selector de rango cuyo Inicio va de 0 a 100. **Palabra por palabra al estilo AutoAE:** opacidad 0 + posición 3D [0,28,0] + desenfoque 14, con el selector "basado en palabras" (`ADBE Text Range Type2` = 3) y suavidad 100.
- **Resaltador o tachado que se pinta:** una forma cuyo tamaño sale de `sourceRectAtTime` de la línea de texto y cuya posición sale de `toComp`, sin padre, con la escala X de 0 a 100. Para que quede debajo del texto se mueve detrás de la capa de texto.
- **Persona en blanco y negro delante del collage, en Premiere:** el collage (.mogrt) va en V3, debajo de la copia de la persona con incrustación por pista mate (V5 + máscara en V6), y a esa copia se le pone "Blanco y negro". Es el look de luchhoedita sin rotoscopia.
- **Funciones comunes entre scripts de AE:** se sacan a un `.jsx` aparte y se cargan con `$.evalFile` (corre en ámbito global). Las funciones que esas funciones usan, como el registro `paso()`, se definen globales *antes* de cargar el archivo. Para agregar comps a un `.aep` existente sin rehacerlo: `app.open(aep)`, borrar solo las comps con el mismo nombre y exportar.
- **Puntos sobre un mapa recortado:** con la escala conocida de la imagen se calcula `x - ancho/2 + fx·ancho` y `y - alto/2 + fy·alto`. Los valores `fx, fy` se miden sobre el PNG con una cuadrícula y se revisan en un cuadro exportado. Si las líneas a las etiquetas se cruzan, se reordenan las etiquetas según la altura de los puntos.
- **Un texto que cambia en cada corte dentro de un .mogrt:** una capa "Lista" invisible, con las palabras separadas por "|" y expuesta como un solo campo. La capa visible lleva una expresión que busca el índice del corte con `time >= T[k]` y hace `style.setText(L[i])`, con tamaño ajustado al largo. Los tiempos de corte van en la expresión, sacados del plan de edición.
- **Animar algo en cada corte con keyframes:** si solo se ponen las claves de entrada (0 → 100), AE interpola de 100 al 0 del corte siguiente y todo se desvanece entre cortes. Hay que repetir el valor final un cuadro antes del corte siguiente, o usar HOLD.


## Más trampas (Miramar, 2026-10-24)
- **Premiere no dibujó las capas negras de pantalla completa de un .mogrt** (ni sólidos ni rectángulos de forma, con o sin "Barrido lineal"), aunque en AE se veían y el texto, las formas pequeñas y los PNG de la misma plantilla sí salían. El oscurecido detrás de los textos y el fundido a negro se hicieron en Premiere con PNG (degradado con alfa hecho en PIL, negro) con claves de opacidad.
- **Campos de tiempo en una plantilla que se estira:** un deslizador "Dato N (s)" en segundos de la comp. Como Premiere solo estira lo que queda entre las regiones protegidas, el script lo convierte: `t_comp = entra + (t − entra)·(D − entra − sale)/(duración − entra − sale)`. Así un bullet cae sobre la palabra dicha.
- **Un texto oculto con varias palabras separadas por `|`** ("Paradas") alimenta las etiquetas de la ruta con `String(thisComp.layer("Paradas").text.sourceText).split("|")`. Hay que pasarlo a `String` antes de `split`.
- **La cola de render de AE salió en .mp4** aunque se pidió la plantilla "PNG Sequence" (en AE en español no existe con ese nombre). Para revisar sirve: se saca el cuadro con ffmpeg.
- **Un campo de posición en píxeles no sirve como deslizador:** "Altura (y)" = 1705 quedó en 100 en Premiere (los deslizadores van de 0 a 100) y la barra salió arriba. Se expone en % del alto (`C("Altura (%)")*19.2`). Ya estaba la regla general; esta vez pasó con una coordenada.
- **No colgar capas de un nulo que ya está movido por expresión** (aunque sea para agruparlas): AE compensa y se van al otro lado del cuadro. Para una barra fija abajo: la altura y el fundido van en una expresión en cada capa.
- **Barra de progreso sincronizada con el video:** comp del mismo largo que el reel y sin regiones protegidas (no se estira). La fracción se calcula por tramos con los tiempos de cada parada como deslizadores: `if(t<T(i+1)) return (i+(t-T(i))/(T(i+1)-T(i)))/5`, y alimenta el Trim de la línea y la posición de la punta.


## Más trampas (cierre JAC, 2026-10-25)
- **Enganchar una capa a un null escalado le cambia la escala:** AE compensa para que no cambie de tamaño en pantalla. Con un null al 38 %, la hija queda al 260 %. Después de `layer.parent = null` hay que volver a poner su escala en 100 (y su posición), o todo sale corrido o al doble.
- **Dibujar un trazo con degradado exacto de inicio a fin:** se usa el PNG del trazo con un mate alfa (`setTrackMatte(capa, TrackMatteType.ALPHA)`). El mate es una capa de forma con un trazo blanco más ancho por la línea central y Trim Paths de 0 a 100. La línea central sale del PNG: el centro de la franja opaca en cada columna.
- **`$.global` se queda vivo en la sesión de AE:** una bandera como `EXPORTAR=false` sigue valiendo en la próxima corrida. Hay que ponerla siempre explícita.
- **Cuadros de prueba:** la cola de render con la plantilla "Secuencia TIFF con alfa" funciona (no hay plantilla PNG en AE en español). Si AE está recién abierto y el render no sale (estado 3015, en cola), hay que volver a correrlo.
- **Logo de Illustrator que llega como `.ps`:** puede ser en realidad un PDF de Illustrator (`%PDF` al inicio). Con `pdftocairo -svg` y cairosvg se separa en piezas PNG con su caja exacta.

- **Una instancia de .mogrt movida con `move()` puede dejar de dibujarse** (2026-10-29): el clip, la opacidad y los parámetros están bien, pero no sale ni en el monitor ni en `exportFramePNG`. Arreglo: quitarla e insertar la plantilla de nuevo con `importMGT` en el mismo tramo, copiando textos y Movimiento. Después de mover plantillas, revisar siempre un cuadro.
- **Animar un nulo padre después de colgarle las capas**, no antes: si el nulo ya tiene claves y está desplazado en t=0 cuando se cuelgan los hijos, AE los compensa y todo queda corrido (pasó con las tarjetas de Miramar, 800 px a la derecha).
- **Gráficos encima de un LUT de log:** si la capa de ajuste con el LUT está arriba, los .mogrt tienen que ir en una pista por encima de ella o el LUT les cambia los colores.
