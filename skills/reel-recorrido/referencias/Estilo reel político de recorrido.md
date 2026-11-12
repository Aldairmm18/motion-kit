# Estilo reel político de recorrido

Referencia: reel de @carolinamejiagomez en el Malecón Deportivo (Instagram, 11 oct 2026, 60,8 s, 1440×2560). Medido el 2026-10-20 en el navegador, sin descargar: cortes cuadro a cuadro (precisión 0,1 s) y audio por bandas.

## Qué hace
- **Cero motion graphics.** Solo subtítulos y personas. La política aparece caminando entre la gente, no en una tarima.
- **Subtítulos mínimos:** 1–3 palabras en MAYÚSCULAS, sans heavy pequeña (~45 px en 1080), color crema/amarillo pálido con sombra suave, a ~70 % de la altura. Corte seco palabra a palabra, sin pop ni barras.
- **30 planos en 61 s** (unos 2 s de media):
  - Abre rápido: 3 planos en 2,5 s.
  - Se calma cuando alguien habla a cámara (testimonios de 5–6 s, abrazo de 6 s).
  - Cierra con una ráfaga de 4 planos de ~0,7 s.
- **Arco de luz:** la tarde dorada pasa al atardecer y termina en la noche. Da la sensación de "un día completo".
- **Mezcla de planos:**
  - Ella caminando con su equipo, saludando y abrazando.
  - Vecinos que dicen una frase corta (los protagonistas son ellos).
  - B-roll de la vida del lugar: niños, perro, bici, patines, fútbol, columpios, bandera, skatepark.
- **Look:** luz natural de hora dorada con destellos de sol, gradeo cálido y contrastado, todo cámara en mano y a ras de piso.
- **Audio:** música de cama continua, suave en los primeros ~10 s. A los ~10 s entra el bajo y ahí los cortes van al ritmo (cada ~1,1 s). Debajo se oyen las voces de la gente. El bajo baja en los últimos ~7 s.

## Color (medido en 15 cuadros)
- **Negros apenas levantados** (p1 ≈ 0,07) y **luces que nunca se queman** (p99 ≈ 0,92, con hombro suave).
- **Sombras teal** (verde y azul por encima del rojo), **piel cálida pero no roja** (R 0,60 · G 0,48 · B 0,39) y saturación media (≈ 0,34).
- Lo que más se nota es **la luz dorada y el contraste**, no el balance medio: en exteriores, el cielo y el mar empujan los medios hacia teal.

## Llevarlo a otras tomas (LUT)
- LUT `Recorrido_Teal_Calido.cube` (y la versión `_suave` al 60 %), hecha con numpy en `efe-gomez-reel/scripts/crear_lut_recorrido.py`. Pasos:
  - **Curva:** medios densos (gamma 1,18) más una S de contraste, y hombro hasta 0,955.
  - **Color por zonas:** sombras teal y luces crema cálidas.
  - **Saturación:** +20 % en general y −8 % en la piel; el naranja de marca se protege con una máscara.
- **Ojo:** si las tomas están quemadas (las paredes blancas de un interior), solo con bajar las luces quedan grises y apagadas. Lo que hace que se parezcan es la curva densa con luces crema.
- **Instalación para Lumetri:** `~/Library/Application Support/Adobe/Common/LUTs/Creative/`. Aparecen en Creativo > Look al reiniciar Premiere, o se cargan con "Examinar…".
- **Premiere no permite crear capas de ajuste por ExtendScript:** solo existe `isAdjustmentLayer`. Se crean desde el menú (Archivo > Nuevo > Capa de ajuste), que solo se habilita con el panel Proyecto activo. Después, por script: `overwriteClip` en la pista, `clip.end` para estirarla y QE `addVideoEffect("Color de Lumetri")`. El Look se elige a mano con Examinar….

## Cómo aplicarlo
- Menos gráficos y más gente. El mensaje lo dicen los vecinos.
- Un subtítulo discreto en vez de títulos grandes.
- Montar los planos del lugar al golpe de la música y dejar respirar los testimonios.

Usado en JAC-Efe-Gomez (reel de Territorio Tech). Relacionado: Estilo de edición Sueños Urbanos, Subtítulos dinámicos y texto detrás de personas.
