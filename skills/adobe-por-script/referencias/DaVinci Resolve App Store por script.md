# DaVinci Resolve (App Store) por script

Visto con Resolve 21.1 gratuito de la App Store, el 2026-10-21, en Taller-Efe-Gomez-Resolve.

- **Scripts desde fuera:** no funcionan. `DaVinciResolveScript.scriptapp("Resolve")` devuelve `None`: eso es solo de Studio.
- **Scripts desde el menú:** sí funcionan. Pero la versión de la App Store los lee desde su espacio aislado: `~/Library/Containers/com.blackmagic-design.DaVinciResolveLite/Data/Library/Application Support/Fusion/Scripts/Utility/`. La carpeta normal `~/Library/Application Support/Blackmagic Design/...` no la ve. Resolve solo busca scripts al arrancar, y salen en Workspace > Scripts.
- **No hay Python** si no está el de python.org (el de Homebrew no lo usa). Hay que escribir en **Lua**.
- **El Lua de la App Store no trae `io`:** no se puede leer ni escribir archivos. La salida se saca con `error("MARCA<<..." , 0)`, que queda en `Library/Application Support/logs/ResolveDebug.txt`. Resolve vuelve a leer el `.lua` cada vez, así que el trabajo se escribe directo en el script.
- **Archivos:** Resolve solo ve lo que está dentro de su contenedor. Los medios se meten con **enlaces duros** (`ln`), que no duplican espacio.
- **Imágenes con números en el nombre** (`MG01_Titulo.png`): las toma como secuencia de imágenes y les cambia el nombre. Hay que usar nombres sin números.
- **`ImportTimelineFromFile` (FCPXML):** se cancela ("Operation canceled") si la biblioteca de medios está vacía. Conviene importar antes los medios con `ImportMedia`, que no pregunta nada.
- **El selector de archivos del sistema** (File > Import) es otro proceso y no se puede controlar con el permiso de la app.
- **FCPXML con medios:** con `importSourceClips=false`, los clips quedan sin enlazar ("Media Offline"). Con `true`, la importación se cancela si el FCPXML tiene imágenes PNG. Funciona importar el FCPXML sin las imágenes y agregarlas después con `MediaPool:AppendToTimeline({{mediaPoolItem, startFrame, endFrame, trackIndex, recordFrame, mediaType=1}})`.
- **Gráficos editables con duración exacta:** un MOV transparente de soporte (qtrle con alfa, 10 s) en la pista que toque, y `TimelineItem:ImportFusionComp(ruta.comp)` + `LoadFusionCompByName`. Sirve igual para subtítulos (una composición por frase).
- **Fusion en un proyecto con gestión de color** trabaja en lineal: los colores de marca se pasan de sRGB a lineal. En Text+, "Arial Black" va como `Font="Arial Black"`, `Style="Regular"`. El ancho del texto es ≈ 0,32 × caracteres × Size (Arial Black).
- **LUT por script:** se copia a `…/Data/Library/Application Support/LUT/<carpeta>/`, se llama a `Project:RefreshLUTList()` y después a `TimelineItem:GetNodeGraph():SetLUT(1, "<carpeta>/archivo.cube")`.
- **Revisar sin la interfaz:** `Timeline:SetCurrentTimecode(tc)` + `Project:ExportCurrentFrameAsStill(ruta.png)`. Para medir el color sin gráficos: `SetTrackEnable("video", i, false)`.
- **Render:** `SetCurrentRenderFormatAndCodec("mp4","H264")` + `SetRenderSettings({TargetDir, CustomName, ...})` + `AddRenderJob()` + `StartRendering({job}, false)`.
- **Pistas de audio apagadas:** `adjust-volume` del FCPXML no se importa; para silenciar una pista se usa `SetTrackEnable("audio", i, false)`.

## Plantillas de título Fusion (editables en el Inspector)
Visto el 2026-10-21 en Taller-Efe-Gomez-Resolve. Es la forma de que un gráfico sea un título y no un clip de video, el equivalente de los gráficos esenciales de Premiere. La documentación está en `DaVinci Resolve.app/Contents/Resources/Developer/Fusion Templates/README.txt`, y hay ejemplos dentro de `Resources/Fusion/Templates/Templates.drfx` (es un zip).
- **Qué es:** un `.setting` con un `GroupOperator`. Lo que se edita en el Inspector son sus `InstanceInput` (SourceOp + Source). Un color son tres entradas (Red/Green/Blue) en el mismo `ControlGroup`. En la versión App Store se instala en `…/Data/Library/Application Support/Fusion/Templates/Edit/Titles/<carpeta>/`, y Resolve solo lo lee al arrancar.
- **Animación que aguanta cambios de duración:** keyframes (BezierSpline) y un `KeyStretcher` al final, con `StretchStart`/`StretchEnd`. Ojo con dos cosas:
  - El KeyStretcher da por hecho que la plantilla mide **120 cuadros (0..119)**, como las de Blackmagic. Las salidas hay que anclarlas al 119.
  - Resolve recorta el rango de cada capa a la duración del clip, y entonces el render falla ("Estirar cannot get Parameter… at time 111"). Por eso los Background, Text+ y Loader van con `GlobalIn = 0` y `GlobalOut = 20000`.
- **Publicar el `Clip` de un Loader** deja la instancia sin archivo. No conviene.
- **`Timeline:InsertFusionTitleIntoTimeline(nombre)`:**
  - Inserta en el cabezal y empuja (ripple) lo que venga después en la pista.
  - Entra en la **pista destino**, que es la última que tocó un script (`AppendToTimeline` o `DeleteClips`). Si esa pista está bloqueada, devuelve nil. Para elegir pista: poner y quitar un clip ancla en ella y bloquear las demás.
  - La duración es la "Standard generator duration" de Preferencias (`GeneratorDurationSecs` en `Library/Preferences/config.user.xml`; se cambia con Resolve cerrado). No hay API para cambiar la duración de un clip. Para recortar un título: insertar otro título en el cuadro donde debe terminar y borrar todo lo que quede de ahí en adelante en esa pista.
  - Para cambiar textos: `item:GetFusionCompByIndex(1):FindTool("Texto7"):SetInput("StyledText", …)`.
- **`ExportCurrentFrameAsStill`** a veces devuelve cuadros negros, sobre todo el primero o cuando falla una composición. Para comprobar algo de verdad, mejor un render.
