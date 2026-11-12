---
name: motion-editorial
description: Diseña motion graphics, subtítulos dinámicos, texto detrás de personas y efectos de sonido con estilo editorial (papel, grabado, un solo rojo, collage de periódico) o institucional sobrio. Úsala para gráficos sobre entrevistas o voz a cámara, explicadores, reels con datos, intros, subtítulos animados o cuando pidan "ponle motion graphics".
---

# Motion graphics editorial

Parte de `principios`.

## Flujo
1. **Huecos primero:** con la línea de historia armada, marcar cada gráfico como hueco en la línea de tiempo (marcador con duración y descripción).
2. **Tabla palabra → gráfico:** qué gráfico cae en qué palabra (tiempo de la transcripción) y una lista de transiciones donde ninguna se repite seguida.
3. **Referencias:** pedir 1–3 videos de referencia y estudiarlos en el navegador (cómo entra el texto, ritmo, sonido) sin descargarlos.
4. **Recursos:** logos oficiales y fotos reales de las personas (recortadas en B/N). Objetos o fondos con IA solo como hoja de recursos (4×3, fondo blanco, grabado B/N con un acento) que luego se recorta. Antes de generar con un servicio de pago, preguntar y revisar el costo.
5. **Piloto:** 6–18 cuadros clave antes del render completo.
6. **Entrega editable:** .mogrt, comp de AE, Remotion o ProRes 4444 con alfa, más una pista aparte solo con los efectos de sonido.

## Estilo editorial (el de base para lo político o institucional)
- Papel (#ECEBE7) y carbón (#222) con grano y viñeta; **un solo rojo** (#C8181F) para el énfasis o la pérdida.
- Serif de alto contraste para cifras y titulares; palabra clave en **rojo cursiva**. Sans media para etiquetas.
- Collage: persona recortada en B/N sobre un bloque de acento, máquina de escribir con resaltador, flecha punteada, nota a mano, cinta, sello.
- Transiciones: casi todo corte seco, y unas pocas motivadas, una vez cada una.
- Foley táctil sintetizado (papel, máquina, marcador, lápiz, sello). Cero whooshes.

## Subtítulos dinámicos
- Bloques de 1–4 palabras que aparecen cuando se dicen y se acumulan; al cerrar la idea se borra el bloque.
- Dos niveles: conectores pequeños en sans y palabra clave 2–3× más grande (sans heavy MAYÚSCULAS o serif cursiva de color).
- A la altura del pecho (50–65 % de alto) o en el tercio superior; sin cajas.
- **Texto detrás de la persona:** máscara de persona (en Mac, Apple Vision `VNGeneratePersonSegmentationRequest`), capas fondo → texto → persona, que tape como máximo ~30 % de la palabra.

## Animación
- Ease-out de 0,25–0,4 s, entrada más lenta que no golpee, rebote bajo. Desenfoque de movimiento.
- Encadenar: lo siguiente entra antes de que termine lo anterior, pero con respiro para leer.
- Por código: todo como función pura del tiempo (sin estado), aleatorios con semilla, resortes en forma cerrada.

## Referencias
- `referencias/Motion graphics editoriales con Claude.md`: proceso completo, estilo Editorial Explainer, Instagram de referencia, generación de imágenes (Higgsfield, OpenArt, AI Studio) y recorte de hojas.
- `referencias/Subtítulos dinámicos y texto detrás de personas.md`: posición, tipografías, animación y la técnica de máscara.
- `referencias/Efectos de sonido sintetizados para motion graphics.md`: recetas de foley con numpy.
- `referencias/Motion con código estilo Prompt Motion.md`: cómo escribir un encargo de motion por código.
- `referencias/Explicadores 3D verticales.md`: estructura y costos de render en Blender.
- `referencias/Remotion con fuentes y escenas desde JSON.md` y `referencias/Remotion Track y capas.md`.
