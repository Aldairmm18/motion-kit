# Prompt para editar el siguiente taller

Prompt para empezar un video nuevo en otro chat con el mismo flujo de la historia FX de Miramar-Primer-Taller (oct–nov 2026). Llenar lo que está entre [corchetes].

```
Vamos a editar el video de [NOMBRE DEL TALLER / BARRIO] para la JAC [NOMBRE], igual a como hicimos la historia FX de Miramar.
Antes de empezar, lee las referencias de la skill video-taller y de color. Trabaja en una carpeta nueva para este video.

MATERIAL
- Videos ya descargados en: [RUTA]. Cámara: Sony FX3 en S-Log3 / S-Gamut3.Cine, 50 fps (confírmalo con la metadata). [Si hay iPhone, dilo.]
- Proyecto de Premiere: [RUTA .prproj]. Formato: [horizontal 1920×1080 a 30 fps, como Miramar | vertical 1080×1920, como Efe Gómez]. Duración máxima: 2 minutos.
- Personas que hablan (nombre completo y cargo para banners): [NOMBRE · CARGO], [NOMBRE · CARGO].
- Banner y logo de la JAC: [RUTA del PDF/AI]. Mensaje legal de cierre: "Producto adquirido con recursos públicos otorgados por la Alcaldía de Medellín, en el marco de Premios Comunales 2026."
- Música: [RUTA del .wav, por ejemplo de mi pack Envato]. Sin créditos de sonidos ni música.

CÓMO TRABAJAMOS
- Premiere se maneja con ExtendScript por el puente de After Effects (premiere/premiere.sh script.jsx). Busca la secuencia por nombre.
- No guardes el proyecto ni exportes sin preguntarme. Yo edito a mano en paralelo: antes y después de cada cambio saca una foto de toda la línea de tiempo y compárala; nunca muevas mis ediciones manuales. Revisa en cuadros exportados, no supongas.
- No repetir tomas, no dejar cuadros negros, no cortar la última palabra de nadie. Nada de tomas temblorosas ni zooms bruscos.
- Escríbeme en español, corto, y actualiza la nota del vault con cada decisión.

PASOS
1. Transcribir las entrevistas (Scribe) y armar la historia: inicio con planos de pies y subida, cada entrevistado con su idea completa, B-roll del recorrido en orden cronológico, llamado final. Oradores con buen tiempo en cámara.
2. Montaje: entrevistas en V2, B-roll encima, cortes del inicio sobre los golpes de la música. Slow ramps (normal → lenta → normal) generadas con ffmpeg en ProRes y conservando los códigos S-Log3, porque Premiere no deja hacer rampas por script.
3. Música en A3 con niveles por tramo: baja bajo las voces y sube 0,6 s después de que la persona termina, con 0,8 s de subida. La cola de la canción llega hasta el final.
4. Color, en este orden de pistas: capas de ajuste por toma ENCIMA del LUT, y LUT debajo. LUT base: Miramar_Base_Sony.cube (Sony LC-709 + negros, altas suaves y saturación) en una capa de ajuste horizontal. El LUT se carga desde Lumetri > LUT de entrada > Examinar, nunca por script. Una capa por toma con exposición, temperatura, tinción y saturación medidas; piel cuidada; tomas bajo árboles con Sombras, no con exposición. En Premiere, Preferencias > Color > "Administración de color de la pantalla" ENCENDIDA, para que lo que veo sea lo que se exporta.
5. Subtítulos (TT · Subtítulo) en la pista más alta, encima del LUT, para que no cambien de color.
6. Banners de las personas animados como el de Efe Gómez (círculo con logo que rebota, barra, nombre, cargo, puntos), generados en Python como ProRes 4444 con alfa, sobre el LUT, a la altura y el tamaño que yo les ajuste.
7. Motion graphics: 3 o 4 estilo noticia (video encogido a la izquierda con una capa de ajuste horizontal y Transformar, tarjeta de papel con grabado a la derecha), más una línea aparte en After Effects estilo referencias (papel gris, personas reales recortadas en blanco y negro con Apple Vision, círculo rojo, palabra en Impact y Didot cursiva, objetos generados en Google AI Studio con Nano Banana 2 Lite, que es gratis con mi cuenta). No borres los que ya estén; yo apago los que no quiera.
8. Cierre: disolución de 0,8 s desde la última toma, logo animado de la JAC sobre blanco (onda que se dibuja, mapa que rebota, letras), desenfoque y disolución cruzada al mensaje de la Alcaldía solo. Música hasta el final.
9. Antes de exportar: comparar cuadros de la exportación con Premiere. Si se ve lavada en el Mac pero mide igual, es el reproductor, no la imagen.
```

## Lecciones del segundo taller de Miramar (2026-11-07)
- Con luz fluorescente, grabar con obturador a 1/60 o 1/120. Si no, toca quitar el flicker por código: Quitar flicker de fluorescentes por código.
- No estabilizar con software: agrandar un poco y poner lenta, o cambiar la toma.
- Al cambiar la duración del video, mover la música por compases para que el golpe final siga cayendo en el logo.
- Portada: Gemini con fotos ya graduadas y un prompt detallado, personas sin contorno. Ver Miramar-Segundo-Taller.
