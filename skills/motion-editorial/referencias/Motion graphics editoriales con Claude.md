# Motion graphics editoriales con Claude

Estudiado el 2026-10-22 en el navegador integrado, sin descargar nada: transcripción leída de los subtítulos del reproductor (a 4×) y cuadros capturados con un `<canvas>` sobre el `<video>`. Sirve para contenido político o institucional. Aplicado en un reel político.

Referencias:
- **Tao Prompts**, *Claude Opus 5.5 Changed Motion Graphics Forever* (youtu.be/5XSwP0DaHF4, 20 min). Es el autor del skill "Claude AI Motion Graphics" (el zip que el usuario bajó).
- **kaye.creatives**, *How To Make Insane Motion Graphics With Opus 5.5 (Claude Motion)* (youtu.be/AsvN_feasyY, 12 min). Compara a Claude con un diseñador: 34 min contra 5 h.
- Short *Motion Design is Now Easier than Ever* (youtube.com/shorts/gslMMfs5SjM, 37 s): es una promoción de **Higgsfield Vibe Motion**.

## El proceso de Tao Prompts (paso a paso)
1. **Transcripción con tiempos (SRT):** se transcribe con whisper.cpp (Claude lo instala) y se marca cada frase con su tiempo. En nuestro caso, Scribe ya lo hace.
2. **Una carpeta de proyecto** con el video "a cámara" limpio y **un video de referencia** del estilo que se quiere copiar.
3. **Instrucción por voz:** "transcribe, quita las pausas raras y usa la referencia como estilo". Luego se le añade el skill de motion graphics.
4. **Primer borrador (~30 min):** queda bien sincronizado pero es "simplón": solo texto y formas. **Claude anima bien, pero dibuja mal las imágenes realistas.**
5. **Las imágenes se generan con IA:** Claude escribe el prompt de una **hoja de recursos** (asset sheet 4×3 en blanco y negro, grabado o medio tono, con un solo acento rojo) y de los **fondos verticales**. Se generan en OpenArt (GPT Image, 2K o más). Después Claude recorta cada recurso del fondo blanco.
6. **Se agregan recursos reales:** logos oficiales y fotos de prensa de las personas, nunca caras hechas con IA.
7. **Iteraciones por voz:** más gráficos, subtítulos, pantalla dividida (la cabeza abajo y el gráfico arriba) y un avatar más desaturado.
8. **Perspectivas difíciles:** cuando Claude no entiende un ángulo, se le dibuja un boceto encima del cuadro (Paint) y lo calca.
9. Para los videos hechos con IA (Seedance y similares) también se pueden poner gráficos encima con seguimiento: los HUD de "Iron Man".

## El estilo "Editorial Explainer" (el más útil para lo político)
- **Paleta:** carbón (#222) y papel (#ECEBE7) con grano y viñeta. Blanco, negro y gris, y **un solo rojo** (#C8181F) para la pérdida, la alerta o el énfasis.
- **Tipografía:** serif de alto contraste para cifras, fechas y titulares. La frase final lleva la palabra clave en **rojo cursiva** ("Funding *dries up.*"). Sans media para las etiquetas.
- **Fondos:** fotos en blanco y negro oscurecidas al 30–45 % (salas, oficinas, bolsa) y empujadas despacio (~1,2 %/s).
- **Recursos visuales:**
  - Gráfico de línea roja que se dibuja, con relleno degradado y punto que late ($555B → $78B).
  - Contadores circulares que se dibujan como un barrido de reloj.
  - Nombres tachados en rojo con trazo de pluma.
  - Grifo que suelta la última moneda, billetes que se queman, reloj de arena ("17 years").
  - Tarjeta-oración bajo la cabeza: [A] → flecha roja "compra" → [B], con "with BORROWED money" debajo.
- **Pantalla dividida:** un panel baja desde arriba (0,6 s) y empuja a la persona a la mitad de abajo. Puede seguir creciendo hasta pasar a pantalla completa.
- **Transiciones:** sobre todo cortes secos, más unas pocas "motivadas" usadas una vez cada una: iris, hoja que cae, página que empuja, línea que barre y panel que crece. **Nunca la misma dos veces seguidas.**
- **Sonido:** dron grave de documental y **foley táctil**: página, pluma, moneda, sello y obturador en las fotos. Los "whoosh" se rechazaron por genéricos.
- **Ritmo:** cada texto se lee al menos 1–1,5 s. Los gráficos caen sobre la palabra dicha (~0,05 s antes) y no se repite en el subtítulo lo que ya dice el gráfico.

## kaye.creatives (Claude Motion): proceso y estilo
Transcripción completa leída del reproductor. La autora (Karima) hace contenido viral sobre IA.
- **Dónde:** en el chat normal de Claude, **Artifacts > Motion**, con el modelo **Opus 5.5**. No hace falta Claude Code ni After Effects. Claude renderiza animaciones 2D y 3D en HTML y entrega un archivo para bajar.
- **Regla de oro:** "el prompt es el 90 % del trabajo". Con una frase vaga ("ponle motion graphics") sale algo básico. Hay que darle referencias, recursos de marca y dirección.
- **Paso 1: gráficos sobre una toma cruda (a cámara)**
  1. Conecta el **MCP de Higgsfield** a Claude, para que genere imágenes y B-roll dentro del chat. Su secreto: mezclar la toma real con generaciones de IA.
  2. Sube el video crudo **y el guion**, para que Claude ubique cada frase.
  3. Sube **varias** referencias en video: "es el secreto para que salga en tu dirección visual".
  4. Adjunta su skill propia, que funciona como generador de prompts: le pasa a Claude reglas de motion design y estructuras de código.
  5. Describe en frases simples lo que quiere, sin nombrar herramientas ni términos técnicos, "como se lo diría a una persona".
  6. **Itera sin empezar de cero:** "bloquea lo que ya funciona y cambia solo esto" (el grano, unificar el estilo de los gráficos, la animación del texto), y manda más referencias.
  7. Resultado: subtítulos en su sitio, degradados según su estilo y B-roll generado. **Claude tardó 34 min y su diseñador 5 h.**
- **Paso 2: intro de lanzamiento desde cero (sin video)**
  1. Junta referencias de lanzamientos de marca (Pinterest).
  2. Sube su skill y describe "la vibra".
  3. En la v2 pide ritmo más rápido y texto que "salte" más.
  4. Sale con look de **recortes de papel de revista**, transiciones limpias, profundidad de cámara y acentos que brillan. Tardó 20 min.
- **Veredicto:** Claude gana en velocidad y precio (una suscripción frente a cientos o miles de dólares por pieza). El diseñador aporta **gusto**: conoce la marca y el público, y propone cosas que uno no pondría en un prompt, como destellos de luz y microdetalles.
- **Look de su video:** serif cursiva terracota sobre la toma ("Let's find out", "This is why we test"), cifras grandes ("10%"), fondos crema de papel con tarjetas terracota que forman flujos ("Your idea → …"), barras de onda de audio y comparaciones en pantalla dividida.

## El short: Higgsfield Vibe Motion
- **Lo que dice:** "el motion design ya no necesita humanos: todo sale de un solo prompt". Los pasos: entrar a Higgsfield, elegir **Vibe Motion**, subir una imagen o escribir el prompt ("anima este logo") y generar. Después se ajustan fuentes, colores y estilos de animación sobre el resultado.
- **Cómo se ve:**
  - Abre con una tipografía de revista (serif cursiva + grotesca negra) sobre un computador viejo.
  - Pantalla dividida: arriba tarjetas tipo vidrio con datos que se mueven (71–77 % con curva, contador "$236,824 → $241,327", carpeta, interruptor de interfaz) y abajo la persona.
  - Blanco a pantalla completa con titulares que entran en desenfoque: "Animation · Motion Design" con los íconos de After Effects, CapCut y DaVinci, y "NO LONGER NEED HUMANS" en negro y lima, con el ícono de AE cayendo a una papelera.
  - Tarjetas de teléfono con interfaz oscura, el logo animado en neón en tres colores, y cierre con un robot (gancho de humor).
- **Para nosotros:** Vibe Motion sirve para animar rápido un logo o una tarjeta. Para un video político conviene la sobriedad del estilo editorial, no el neón.

## Cómo lo llevo a Premiere
- Los gráficos se marcan como **huecos** en la línea de tiempo (cartel apagado más un marcador rojo con duración y descripción). Así la historia queda armada y los gráficos se hacen después, con la referencia al lado.
- Por coherencia, en un video político va **un solo color de acento** y no dos.
- Los datos y cifras tienen que salir de la fuente (lo que dice la persona o un documento), nunca inventados. Los logos van de archivos oficiales.

Relacionado: Subtítulos dinámicos y texto detrás de personas, Estilo reel político de recorrido, Método de la línea de historia.

## El zip "Claude AI Motion Graphics" (skill de Tao Prompts), a fondo
Revisado el 2026-10-22: extraído en una carpeta temporal aparte y leído completo antes de usar nada. **No se instaló.** Son 55 archivos (1,2 MB): `SKILL.md`, `references/`, `scripts/`, `music-video/` y fuentes con licencia libre (Poppins, Space Mono, Luckiest Guy y Permanent Marker).
- **Cómo funciona:** Claude escribe Python que dibuja cada cuadro (skia, OpenCV, PIL), lo guarda en JPG y lo codifica con ffmpeg junto con el audio. No usa After Effects ni APIs de pago.
- **Seguridad:**
  - `setup_env.py` instala con pip numpy, opencv, scipy, pillow, skia-python, faster-whisper y librosa, y baja Poppins de Google Fonts si falta.
  - `word_srt.sh` clona whisper.cpp y baja el modelo `small.en` de Hugging Face.
  - Nada más sale a la red, y no encontré instrucciones ocultas.
- **Cinco estilos:**
  1. *Editorial Explainer*: el de los shorts tipo Vox, ver arriba.
  2. *AI Film FX*: gráficos dentro de videos hechos con IA, con seguimiento y carteles anclados en 3D.
  3. *2D Animated Explainer*: caricatura plana 2.5D hecha desde una voz en off.
  4. *3D Voxel Animation*: zoom continuo "Powers of Ten" con cubos, en Three.js.
  5. *Cartoon Music Video*: personajes pintados, sincronizados con una canción de Suno.
- **Flujo común (12 pasos), lo más útil:**
  - Hacer inventario con ffprobe y hojas de contacto, y buscar cortes ocultos dentro de las tomas.
  - Analizar la referencia: palabras por minuto, bloques, cómo entra el texto y espectrograma del sonido. Las tiras a 5 fps muestran *cómo* se mueve cada cosa, que con cuadros sueltos no se ve.
  - Sacar los tiempos por palabra.
  - Quitar las pausas por **nivel de audio, no por los huecos de la transcripción**: silencio = pico − 42 dB durante más de 0,42 s, dejar 0,10 s antes y 0,14 s después, con fundidos de 20 ms.
  - Esconder los cortes debajo de los gráficos.
  - Hacer una tabla de palabra → gráfico, y una lista de transiciones en la que ninguna se repite seguida.
  - Probar con 6–18 cuadros antes de renderizar todo.
  - Entregar siempre una pista aparte solo con los efectos de sonido.
- **Reglas de estilo que salieron de correcciones reales** (`style-rules.md`):
  - Texto blanco y grueso con sombra suave, más un degradado oscuro abajo.
  - Tamaños: subtítulos ~56 px y frase clave ~96 px. "Muy pequeño" fue la queja más repetida.
  - Nada importante en el borde de arriba.
  - **No cortar de plano mientras hay un subtítulo en pantalla.**
  - Nada de cámara lenta mezclando cuadros (se ve borrosa).
  - Máximo 2–3 anotaciones, una a la vez.
  - **Cero whooshes**: para que aparezca algo, un pop o un acorde suave.
  - Los logos solo de archivos oficiales.
- **Técnicas reutilizables** (`techniques.md`):
  - Recorte de persona sin IA (GrabCut + filtro guiado).
  - Seguimiento de cámara con homografías y de objetos con LK.
  - Carteles anclados al mundo, pantallas curvas y capas apiladas en 3D.
  - Deslizador antes/después con su curva de movimiento.
  - Partículas y disoluciones con campo de ruido.
  - Ilustraciones de tiza a partir de una hoja generada.
  - Capa de solo gráficos sacada de restar dos renders.
  - Lista de pasos con un resaltado que se desliza.
- **Para nosotros:** como nuestro flujo es Premiere con .mogrt, el skill sirve como **guía de estilo y de recetas**, más que para renderizar con Python. Si algún gráfico conviene hacerlo así, se renderiza en ProRes 4444 con alfa y se pone en la pista de los huecos.

## Higgsfield (generar imágenes y video con IA)
Revisado el 2026-10-22 sin instalar nada: npm, el paquete descargado sin ejecutarlo y el repo de GitHub.
- **Qué es:** una plataforma que junta más de 30 modelos:
  - Imagen: GPT Image 2.5, Nano Banana 2/Pro, Seedream 5, Soul 2.0, Recraft V4.1 (vectores y logos) y Flux 2.
  - Video: Seedance 2.5 (4–30 s, hasta 1080p; Seedance 2.0 para 4K), Kling 3.0 y Veo 3.1.
  - Audio: Seed Audio 1.0.
  - 3D (GLB).
  - Además, "Marketing Studio" (anuncios con avatar y producto) y "Virality Predictor", que califica el gancho y la retención de un video terminado.
  - Cada trabajo **gasta créditos** de la cuenta.
- **CLI `@higgsfield/cli`** (1.2.2, npm, mantenido por gente de higgsfield.ai, licencia MIT):
  - Al instalarse baja el binario `hf` desde los releases de GitHub (higgsfield-ai/cli) y **verifica su SHA-256** contra sumas que trae el paquete. Si no coinciden, no instala.
  - Comandos: `higgsfield auth login` (OAuth en el navegador), `higgsfield account status` (plan y créditos), `higgsfield generate create <modelo> --prompt … --wait` y `higgsfield generate cost …` (para saber el costo antes).
- **Skills (`npx skills add higgsfield-ai/skills`):**
  - Usa el CLI `skills` de Vercel, que copia las skills a la carpeta de skills del agente. Otras vías: `claude plugin marketplace add higgsfield-ai/skills` o `./setup --host claude`.
  - Son 8: generate, soul-id (identidad facial entrenada con 5–20 fotos), product-photoshoot, brandkit (logos SVG, paletas y brandbook), marketplace-cards, websites (despliega en Cloudflare), video-explainer y youtube-thumbnail.
  - **Ojo:**
    - La skill `higgsfield-generate` instala el CLI por su cuenta con `curl … install.sh | sh` si no lo encuentra.
    - Pide permiso de Bash.
    - Su regla 5 dice "no estimar el costo antes". Conviene pedirle siempre `higgsfield generate cost` antes de generar, para no gastar créditos sin querer.
- **Vibe Motion** (en la web de Higgsfield): anima un logo o una imagen desde un prompt y deja editar fuentes, colores y estilo. Es lo que muestra el short.
- **MCP de Higgsfield:** se conecta a Claude (chat o Claude Code) para generar dentro de la conversación. Es lo que usa kaye.creatives.
- **Video explainer** (skill `higgsfield-video-explainer`): arma un explicativo narrado (no fotorrealista) en bloques de 10 s: voz con Seed Audio, clip con Gemini Omni y ensamblado en el servidor (`explainer_video`). No sirve para personas a cámara.
- **Para qué nos sirve:** la hoja de recursos del estilo editorial (prompt de Claude → GPT Image 2.5 en 2K), fondos verticales en blanco y negro, y como mucho clips cortos de apoyo con Seedance. Nunca para caras de personas reales.

### Higgsfield en uso (2026-10-22)
- **El plan gratuito trae 10 créditos** de registro. Antes de nada hay que elegir el workspace: `hf workspace list` y `hf workspace set <id>`. Sin eso, ni `account status` ni `generate cost` funcionan.
- **GPT Image 2.5 exige el plan Basic (de pago):** `generate create` devuelve `job_minimum_basic_plan_required` y **no cobra**. `generate cost` no avisa de esto, y `model get` tampoco dice qué plan pide cada modelo.
- **Costos por imagen (3:2):**
  - GPT Image 2.5: 1 crédito en calidad media 2K, 2,75 en alta 2K y 4,25 en alta 4K.
  - Seedream 4.5: 1 crédito, en básica o alta.
  - Nano Banana 2: 1,5 a 1K y 2 a 2K.
  - Nano Banana Pro: 2.
  - Seedream 5 Pro: 2,5.
  - Recraft V4.1: 1,25 a 1K y 8 a 2K.
  - GPT Image 2: 6,5.
- Script con el costo revisado antes de cada imagen: `cali-jhonier-politico/edit/generar_imagenes.py`.
- **Seedream 4.5 también exige el plan Basic** por el CLI (mismo error y sin cobro). Al parecer el plan gratuito no genera imágenes por CLI con los modelos que valen la pena: los 10 créditos de registro no sirven ahí. Para generar hay que pasar a Basic o usar la web.

### OpenArt en uso (2026-10-22)
- **El plan gratuito trae 40 créditos.** GPT Image 2.5 cuesta lo mismo a 1K que a 2K: 5 créditos en calidad baja. En media cuesta 30 a 1K y 40 a 2K. El precio se ve en el botón "Generate" y cambiar los ajustes no gasta nada.
- **En el plan gratuito las imágenes salen con marca de agua** (`cdn.openart.ai/watermarked_images/…`): una franja blanca semitransparente a lo ancho del centro, que tapa los dibujos de la fila del medio de una hoja de recursos. Los planes de pago la quitan. No la quito a mano: es una condición del plan.
- **Calidad baja a 2K basta** para grabados en blanco y negro: en la Hoja A salieron los 12 dibujos bien.
- El editor del prompt es un ProseMirror: hay que hacer clic en él antes de escribir. Con el panel angosto, la app se ve en modo móvil.
- **Higgsfield gratis no genera ninguna imagen por CLI**: ni siquiera Nano Banana 2 Lite (1 crédito) pasa (`job_minimum_basic_plan_required`, sin cobro).
- **Descargar de OpenArt sin el diálogo de descarga:** la API `suite/api/resources?folderIdNull=true…` (con la sesión iniciada) devuelve cada generación con `url` a resolución completa (por ejemplo 2016×1344). La que dice `thumbnailUrl` es la miniatura de 512 px. Se bajan con curl.
- **Recorte de hojas de recursos por cuadrícula** (`cali-jhonier-politico/edit/recortar_hojas.py`):
  - En cada celda, tinta = lo que se aleja del blanco (canal mínimo < 0,93) o tiene saturación, para que el terracota también cuente.
  - Caja por percentiles y alfa = tinta.
  - El color se "despremultiplica" contra el blanco, así el grabado conserva la trama sobre cualquier fondo.
  - 28 dibujos salieron limpios. La marca de agua deja restos solo donde cruza un dibujo.

## Referencias de Instagram que eligió el usuario (2026-10-22)
Vistas en el navegador integrado con la versión "embed" de cada reel (`/reel/<id>/embed/captioned/`). La página normal no carga el video sin sesión, pero el embed sí, y con un `<canvas>` se sacan tiras de cuadros.
1. **diego.nxt**, "Esta web te ahorrará horas de animación" (DZBBmyLxmD_, 28 s). Muestra **Swishy**, un "AI motion designer".
   - Del talking head pasa a fondo blanco con un **barrido desenfocado**.
   - Titular condensado **solo en contorno** ("HORAS", "MOTION") detrás de un **objeto 3D realista** (un reloj) y una palabra en sans negra delante ("editando", "graphics").
   - Una **línea roja que se dibuja y parte el objeto en dos** para revelar lo siguiente.
   - Titulares con textura de fotocopia ("EDICIÓN DE IMPACTO") y subtítulos de una palabra en letra manuscrita blanca a la altura del pecho.
2. **romel.media**, "editar estilo Motion Graphic en CapCut" (DNQtkX0NL88, 70 s).
   - Tarjeta oscura con grano, ilustraciones planas con brillo violeta y titulares blancos en mayúsculas ("SI TIENES", "ES PARA TI").
   - Flecha punteada curva que se dibuja, globo de "like 2K" y "100 millones" en cursiva dorada.
   - Una escena cada ~0,8 s con desenfoque y curvas de velocidad (ease 0,5).
3. **jere.edicion**, "Cómo hacer animaciones sin After Effects" (DRyK8pHjc6W, 33 s). Muestra **AutoAE** (autoae.online: "Create Viral Animations Online in Seconds", con marca de agua en el plan gratis). **Es la que más le gustó al usuario.**
   - Fondo gris claro con viñeta.
   - Titular que mezcla **minúscula fina con MAYÚSCULA gruesa** ("como hacer estas ANIMACIONES sin after effects"), y cada palabra entra con desenfoque.
   - **Tarjetas 9:16 que giran en 3D** desde un punto y se abren en abanico. Las plantillas son fichas de perfil: persona recortada en blanco y negro sobre un skyline, con el nombre en serif ("Elon Musk"), y un mapa antiguo con foto.
   - Destello de luz azul como transición.
4. **luchhoedita**, "$50 Edit" (DTzQUvmjt9M, 8,5 s). Es un **collage editorial de periódico**:
   - Persona recortada en blanco y negro sobre un **bloque de color de acento**.
   - Texto serif a **máquina de escribir** con **resaltador** que se pinta detrás, flecha punteada y nota a mano.
   - Cinta adhesiva, sello circular (el logo de Vox), estatua en blanco y negro, post-it y pluma.
   - Lista de **efectos de sonido** que muestra en pantalla: papel arrugado, clic de engranaje, papel, máquina de escribir, papel que se desliza, marcador y lápiz.

**Lenguaje común para llevar a los gráficos:**
- Collage de papel y periódico, con la persona recortada en blanco y negro sobre un bloque de acento.
- Máquina de escribir más serif con resaltador.
- Flechas punteadas y notas a mano.
- Tarjetas que giran en 3D.
- Un objeto que se parte con una línea.
- Sonido de papel, máquina y marcador.

### Google AI Studio (2026-10-31)
- Nano Banana 2 Lite (`gemini-3.1-flash-lite-image`) generó sin pedir pago con la sesión de Google del usuario, en unos 10 s. Nano Banana 2 y 2.1 aparecen como de pago. La imagen sale con un destello visible en la esquina.
- Creen dice "sin registro", pero pide iniciar sesión al generar.
- Para el estilo de papel gris, un solo objeto con un color y fondo liso sale muy bien. Para personas siguen siendo mejores los recortes reales.

## Nano Banana 2 Lite en AI Studio (visto 2026-11-04 en Miramar-Segundo-Taller)
- El modelo se elige en Run settings: "Nano Banana 2 Lite", id `gemini-3.1-flash-lite-image`; la URL queda `aistudio.google.com/prompts/new_chat?model=gemini-3.1-flash-lite-image`.
- Si se escriben dos prompts seguidos sin esperar a que el primero se envíe, se juntan en un mensaje y salen los dos objetos en una sola imagen lado a lado: enviar uno por uno y comprobar.
- En el panel angosto, el tooltip del botón Run tapa la caja; `form_input` llena el texto pero no habilita Run hasta escribir un carácter más.
- Las imágenes están en el DOM como `data:image/png;base64`; la lista es virtual (solo están las que se ven). Varias descargas seguidas con `a.download` en el navegador integrado dejan archivos ocultos `.Q6L2SF6YDW.com.anthropic.claudefordesktop.*` sin renombrar en Descargas: son los PNG completos.
- Las imágenes traen la marca de Gemini en la esquina inferior derecha: borrarla al quitar el fondo.
