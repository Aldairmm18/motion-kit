# Video de iPhone sale brillante en Premiere

**Síntoma:** al exportar, el video del iPhone se ve quemado o demasiado brillante, sobre todo las paredes blancas y el cielo.

**Causa:** el iPhone graba en HDR (Rec. 2100 HLG). Si la secuencia se crea a partir de un clip así, Premiere la deja trabajando en **Rec. 2100 HLG** y el archivo exportado sale marcado como HDR. Visto en JAC-Efe-Gomez el 2026-10-20.

**Arreglo:**
- **A mano:** Secuencia > Configuración de secuencia > Gestión del color > Espacio de color de trabajo = **Rec. 709**, con "Asignar tonos automáticamente" activado. Al exportar, revisar que el espacio de color de salida sea Rec. 709.
- **Por script (ExtendScript):** `s = seq.getSettings()`, tomar de `s.workingColorSpaceList` el que tenga `name === "Rec. 709"`, asignarlo a `s.workingColorSpace`, poner `s.autoToneMapEnabled = true` y llamar a `seq.setSettings(s)`. Para revisar los clips sirve `projectItem.getOriginalColorSpace().name`.

**Ojo:**
- Cualquier gradeo o LUT hecho mientras la secuencia estaba en HLG cambia al pasarla a 709: hay que recalibrarlo.
- Lumetri guarda en memoria la LUT del Look. Si se sobrescribe el `.cube`, no la vuelve a leer: hay que elegirla otra vez, mejor con un nombre de archivo nuevo.
