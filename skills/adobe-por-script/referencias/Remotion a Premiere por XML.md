# Remotion a Premiere por XML

Cómo pasar un video armado en Remotion a una secuencia editable de Premiere Pro (probado con Motion-graphics-ia → `biciclick-vertical`).

- **Formato:** XML de Final Cut Pro 7 (`<xmeml version="4">`). Premiere lo importa con *Archivo > Importar*. Cada `<clipitem>` lleva `start/end` (secuencia), `in/out` (archivo) y un `<file>` con `pathurl` absoluta (`file://localhost` + ruta codificada con `quote`).
- **Leer el proyecto tal como esté:** parsear el TSX con el compilador de TypeScript (`ts.createSourceFile`, ScriptKind TSX) y evaluar los atributos (números, `fps`, `staticFile`, `interpolate`). Así entran los cambios hechos en Remotion Studio.
- **Gráficos con transparencia:** una composición auxiliar (`ExportPieza`) con `calculateMetadata` que toma la duración de los props. Se renderiza cada pieza con `--sequence --image-format=png` y ffmpeg la pasa a **ProRes 4444** (`prores_ks -profile:v 4444 -pix_fmt yuva444p10le -qscale:v 9`). Sale liviano: ~770 MB para 108 s con 5 capas.
- **Video VP9 con alfa → ProRes:** hay que decodificar con `-c:v libvpx-vp9` antes de `-i`. El decodificador nativo de ffmpeg descarta el alfa.
- **Lo que no pasa:** filtros CSS (el B/N se hornea en un clip aparte) y curvas de volumen (las bajadas se hornean en una pista de música mezclada; la música cruda va apagada).
- **Subtítulos editables:** exportar un SRT con las mismas páginas que el componente. Premiere lo importa como pista de subtítulos con el texto editable.
