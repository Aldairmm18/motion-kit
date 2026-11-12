---
tipo: aprendizaje
fuente: https://youtube.com/shorts/sAxKyPgnBJY
fecha: 2026-10-31
---

# Explicadores 3D verticales

Referencia: "Tus AirPods no tapan el ruido. Lo fabrican." de TrendLumEsp, 2 min, vertical.

## Estructura
Título que contradice una creencia → pregunta sin resolver → mecanismo por dentro → analogía física → segundo giro → límite del truco, que cierra la pregunta → acción útil → pedir que lo envíen a alguien.

## Por qué es viable
Pocos objetos 3D reutilizados con cámaras distintas: persona, audífono, oído y escena. La riqueza sale de la cámara, los cortes por rayos X y la capa de datos encima, no de modelar cada plano.

## Cómo hacerlo con nuestras herramientas
Guion y voz primero, porque la voz manda el tiempo. Blender por script para el 3D, After Effects o Remotion para ondas, etiquetas y subtítulos, y sonido al final. Empezar con un piloto de 15 s antes de un video completo.

## Lo que dejó el piloto del termo
- En Cycles a 1080×1920 y 96 muestras, unos 19 s por cuadro en planos abiertos y más de 30 s en acercamientos con partículas. Total: 2 h 46 min por 15 s.
- Medido en M5: Eevee rinde unas 4-10 veces más rápido y queda muy parecido en superficies mate, pero el acero espejo sale con grano y reflejos distintos. Cycles con 32 muestras, adaptativo y menos rebotes ahorró 17-45 % y casi no cambió la imagen. Lo razonable es combinar por plano.
- Un solo plano continuo evita rehacer escenas, y dejar textos y etiquetas en After Effects permite corregirlos sin volver a renderizar el 3D.
- Sobre partículas o metal, el subtítulo necesita contorno negro; un color solo no alcanza.
- Las partículas con posición como función pura del cuadro permiten retomar un render cortado sin cambios.

Ver Motion-graphics-ia, Blender por script y Método de la línea de historia.
