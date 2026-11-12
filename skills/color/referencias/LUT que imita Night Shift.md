# LUT que imita Night Shift

Night Shift no sale en las capturas de pantalla, así que para copiar su look hay que leerlo y calcularlo.

- **Leer el estado:** CoreBrightness es privado (`CBBlueLightClient`). `getBlueLightStatus:` dice si está activo, `getStrength:` da la intensidad (0–1) y `getCCT:` la temperatura en kelvin. Al 50 % son 4100 K. Hay una herramienta en Objective‑C en `segundo-taller-efe-gomez/edit/herramientas/nightshift.m`. Se compila con `clang -fobjc-arc -framework Foundation`.
- **Qué hace:** multiplica cada canal de la luz que sale de la pantalla para que el blanco quede en el color de un cuerpo negro a esa temperatura. Las ganancias salen del lugar de Planck (la aproximación de Kim et al.) pasado a sRGB lineal, divididas por las de 6500 K, con el canal mayor en 1. A 4100 K quedan R 1 · G 0,709 · B 0,405 en lineal.
- **En video Rec. 709 (gamma 2.4)** una ganancia lineal g es una ganancia g^(1/2.4) sobre el valor codificado. A 4100 K eso da R 100 % · G 86,6 % · B 68,6 %. Es solo una multiplicación por canal, así que el LUT y el efecto **Niveles** (salida blanco por canal) dan lo mismo: en una prueba la diferencia fue de menos de 0,3/255.
- **En Premiere por script:**
  - No se puede elegir un LUT en Lumetri > Creativo > Aspecto. Cambiar el índice del menú no hace nada.
  - "Equilibrio de color (RGB)" y "Mezclador de canales" no se dejan poner con `addVideoEffect` en una capa de ajuste. **"Niveles" sí** (propiedades 8, 13 y 18 = salida blanco de R, G y B).
  - Tampoco hay API para crear una capa de ajuste: hay que hacerlo con el menú Archivo > Nuevo > Capa de ajuste, con el panel del proyecto enfocado (Ventana > Proyectos > <proyecto>). Si no, la opción sale desactivada.
- **Para Lumetri a mano:** si los `.cube` se copian a `~/Library/Application Support/Adobe/Common/LUTs/Creative/`, aparecen en Aspecto, a veces después de reiniciar Premiere.
- **Ojo:** para revisar el look hay que **apagar Night Shift**. Si no, se ve doble de cálido.

Se usó en Segundo-Taller-Efe-Gomez. Script: `edit/lut_nightshift.py`, que lee Night Shift y saca 3 temperaturas.
