# Color y After Effects según tutoriales

Sacado de las transcripciones de YouTube, el 2026-10-31. Se bajan sin descargar el video: `yt-dlp --skip-download --write-auto-subs --sub-langs "es-orig,en-orig" --sub-format vtt`. Si responde 429, reintentar un video a la vez. Visto en Miramar-Primer-Taller.

## Color de S-Log3
- Orden: conversión a Rec. 709, exposición y contraste, balance de blancos, piel, y al final el look creativo.
- Corrección y look van separados: el LUT creativo no debe mover exposición, contraste ni blancos.
- Forma de onda: algo realmente negro toca 0; si hay luces en cuadro, llegan cerca de 100. La forma debe quedar expandida. Si no, la imagen se ve lavada.
- Blancos: en el desfile RGB los tres canales a la misma altura, con Temperatura y luego Tinción.
- Piel: en el vectorscopio, sobre la línea de piel. Para medirla, enmascarar la frente.
- Piel con Tono vs. tono, o con HSL secundario.
- Separar la piel: HSL secundario sobre la piel, invertir y mover el fondo a azul verdoso en medios y altas. La piel resalta sin saturarla.
- Sombras de la piel con un toque de rojo.
- Si una toma log está oscura, subir exposición antes del LUT.
- Clips en la misma luz comparten ajuste; no hace falta tratar cada uno aparte.
- En Lumetri, Preferencias: "Mostrar gestión de color" hace que el monitor muestre Rec. 709 correcto.

## After Effects con Claude (Mauro Arredondo)
- Claude escribe scripts; no trabaja dentro de AE. Conviene un script puente para no correrlos a mano cada vez.
- Trabajar en una composición aparte y dejar un nulo con los valores editables.
- Pedir con lenguaje técnico y una imagen de referencia. Hacer a mano lo que se sabe y delegar lo difícil.

## Fluidez en After Effects
- Curvas de velocidad: entrada más lenta para que no golpee; rebote elástico bajo, cerca de 15.
- Desenfoque de movimiento en todas las capas.
- Encadenar: lo siguiente entra antes de que termine lo anterior, con un desfase de pocos cuadros. Dejar un respiro para leer.
- Texto: animador por palabras con posición, desenfoque y opacidad, Offset de −100 a 100 con Ease High −50 y Low 100.
- Eco en una copia de color debajo del texto, y Desenfoque radial rápido para el barrido.
- Formas: animar Tamaño, no Escala, para no deformar las esquinas.

## Generar con IA (Mirko)
- Primero imagen base, luego animación con esa imagen como referencia. Mantener estilo subiendo la imagen anterior.
- Si la duración es muy larga para la animación, sale lenta y desincronizada.
- Hoja de producto con varias perspectivas evita que invente lados.
- Combinar IA y edición: poner el video real dentro de la animación generada.

## LUT creativo sobre S-Log3 (Christian Maté Grab, Cinem8; visto 2026-11-05 en Miramar-Segundo-Taller)
- Grabar S-Log3 1–2 pasos sobreexpuesto: cebra 1 en 94 % (altas), cebra 2 en 55 % ± 5 sobre la cara.
- El LUT va **de último**, en una capa de ajuste sobre todos los clips, directo sobre el log (sin convertir antes a Rec. 709).
- Las correcciones van **antes** del LUT: en el clip o en una capa de ajuste debajo del LUT. Casi siempre basta bajar o subir la Exposición; luego sombras, medios y altas, y ruedas (sombras a cian, medios a naranja). Corregir después del LUT rompe la imagen.
- Su LUT "CMG Vision 3" además corrige la exposición, para usarlo como LUT de monitoreo en cámara.
- Lo comprobado aquí: con la corrección antes del LUT, el contraste del LUT actúa sobre una imagen ya bien expuesta y las tomas lechosas dejan de verse lavadas.
