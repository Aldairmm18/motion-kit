# Estabilizar tomas por código (cámara bloqueada)

Visto el 2026-11-02 en Tercer-Taller-Efe-Gomez, con material FX3 en S-Log3, sin tocar el color.

- **Método:** OpenCV en un venv propio. Puntos del fondo de cuadro en cuadro (`goodFeaturesToTrack` + `calcOpticalFlowPyrLK` a 1/4 del 4K), semejanza con `estimateAffinePartial2D` y RANSAC, trayectoria acumulada y corrección hacia su promedio. El CLAHE ayuda: el log es plano.
- **La matriz acumulada ya lleva el cuadro al primero.** La corrección es `inv(objetivo) @ acumulada`. Con `objetivo @ inv(acumulada)` el movimiento sale exactamente al doble: si lo residual da 2× lo original, es el signo.
- **Medir siempre en el archivo ya renderizado**, con el mismo seguidor. Así salió ese error, que a ojo en una hoja de miniaturas no se ve.
- **Primero escoger, después estabilizar.** Bloquear solo sirve si el recorrido cabe en el margen. Una toma de 7 s con paneo y zoom no se bloquea; una de 1,2 s con 50 px queda clavada casi con 51 % de escala. Toma quieta + 60 % + bloqueo da resultados que el estabilizador solo no da.
- **Conservar S-Log3:** decodificar a `yuv444p10le` con `setparams=range=pc`, deformar Y, U y V por separado en `float32` y sacar ProRes. En Premiere: `setOverrideColorSpace` con el `getOriginalColorSpace()` de un bruto.
- **Escala mínima:** llevar las esquinas de la ventana visible (con posición y escala de Premiere) a la imagen original y subir la escala hasta que queden dentro, en todos los cuadros.
- **Warp Stabilizer en Premiere** no acepta velocidad cambiada ni tamaño distinto a la secuencia sin anidar, así que no sirve para muchas tomas por script.
- **Máscaras alineadas al 4K:** se les aplica la misma matriz con la traslación a la mitad, si están a la mitad de resolución.
- **Una hoja de miniaturas que recorta no sirve para revisar el encuadre:** una toma horizontal parecía mal encuadrada solo por el recorte de la hoja.

Relacionado: Cortes de voz precisos en Premiere por script.
