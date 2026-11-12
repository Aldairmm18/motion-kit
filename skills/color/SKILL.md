---
name: color
description: Corrige y da look al color de un video (S-Log3, iPhone, LUTs, Lumetri, capas de ajuste, teal & orange, piel) en Premiere, Resolve o por código con ffmpeg. Úsala cuando pidan gradear, poner un LUT, igualar tomas, arreglar una imagen lavada o quemada, o un look de referencia.
---

# Color

Parte de `principios`.

## Reglas fijas
- **LUTs y color siempre en una capa de ajuste arriba**, nunca en los clips.
- Orden de pistas: capas de corrección por toma → LUT creativo encima → subtítulos y gráficos por encima de todo.
- La corrección va **antes** del LUT (debajo). Corregir después del LUT rompe la imagen.
- Ningún look por defecto (Night Shift, teal & orange) sin preguntar.

## Orden de corrección
1. Conversión a Rec. 709 (o LUT de entrada de la cámara).
2. Exposición y contraste: lo negro toca 0, las luces cerca de 100, forma de onda expandida.
3. Balance de blancos: en el desfile RGB los tres canales a la misma altura (Temperatura y luego Tinción).
4. Piel sobre la línea de piel del vectorscopio; medir en la frente.
5. Look creativo al final, sin mover exposición ni blancos.

## Trucos
- Tomas bajo árboles: subir Sombras, no Exposición.
- Separar la piel: HSL secundario sobre la piel, invertir y llevar el fondo a azul verdoso en medios y altas.
- Clips con la misma luz comparten ajuste.
- No saturar los casi grises: los blancos se van a lila.
- Tomas quemadas: bajar las luces las deja grises; lo que funciona es una curva densa con luces crema.

## Trampas en Premiere
- No cambiar la ruta del LUT por script (`setValue`): deja de aplicarse sin aviso. Cargar desde Lumetri > LUT de entrada > Examinar.
- Las capas de ajuste no se crean por script: Archivo > Nuevo > Capa de ajuste con el panel Proyecto activo.
- Activar Preferencias > Color > Administración de color de la pantalla.
- Si un MP4 se ve lavado en QuickTime pero mide igual que Premiere, es la etiqueta de gamma, no la imagen.
- Video de iPhone que sale brillante: ver la referencia.

## Referencias
- `referencias/Color y After Effects según tutoriales.md`: color de S-Log3 y LUT creativo sobre log.
- `referencias/LUT teal and orange con neones protegidos.md` y `referencias/LUT que imita Night Shift.md`: cómo fabricar LUTs con numpy.
- `referencias/LUT de Lumetri por script en Premiere.md` y `referencias/Video de iPhone sale brillante en Premiere.md`.
