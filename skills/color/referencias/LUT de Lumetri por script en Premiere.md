# LUT de Lumetri por script en Premiere

No cambies la ruta del LUT de entrada con `setValue`. La propiedad acepta la ruta y el panel muestra el nombre nuevo, pero Premiere deja de aplicar el LUT y la imagen queda en log. Volver a poner la ruta anterior no lo arregla, y el cambio no aparece en el Historial, así que tampoco se puede deshacer.

Arreglo: en Color de Lumetri, Corrección básica > LUT de entrada > Examinar, y elegir el `.cube`. Cargarlo desde ahí sí vuelve a aplicarlo.

Visto el 2026-10-30 en Miramar-Primer-Taller.

## Otros límites vistos en Miramar
- La capa de ajuste vertical no sirve con Transformar: deforma la imagen. Hay que crear una capa de 1920×1080. Por script no se puede; en la interfaz, Archivo > Nuevo > Capa de ajuste exige el foco en el panel de proyecto.
- Las claves de Movimiento en clips con velocidad cambiada no caen en `inPoint + tiempo`. Para animar sobre esos clips, usar una capa de ajuste.
- Después de un diálogo, tomar otra captura antes de hacer clic en la misma coordenada: si ya se cerró, el clic cae en lo que hay debajo.

## Exportación que se ve lavada en Mac
Si un H.264 Rec. 709 se ve lavado en QuickTime pero sus píxeles miden igual que Premiere, no es la exportación: es el gamma con que el reproductor interpreta la etiqueta. Se puede cambiar la etiqueta de transferencia sin recomprimir: `ffmpeg -i in.mp4 -map 0 -c copy -bsf:v h264_metadata=transfer_characteristics=13 out.mp4`.
