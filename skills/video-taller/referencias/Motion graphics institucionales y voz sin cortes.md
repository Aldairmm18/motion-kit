# Motion graphics institucionales y voz sin cortes

Aprendido al hacer el vertical de Biciclick v2 (2026-10-16). Complementa Estilo de edición Sueños Urbanos.

## Motion graphics mientras alguien habla
- **Solo en datos y palabras clave**, y que entren **en la palabra exacta** (tiempo de la transcripción): contador "600" cuando dice "seiscientas", anillo que se llena hasta 100 % en "cien por ciento gratuitos", pines que caen en "dieciséis puntos", fichas que aparecen en "yoga", "nutrición" y "salud mental".
- **Un gráfico a la vez.** En pantalla 2–5 s y fuera en cuanto termina la idea.
- **Nunca sobre caras.** En planos de entrevista van en la franja del pecho (y ≈ 900–1250 en 1080×1920) o arriba, en el cielo (y ≈ 330–560).
- **Sistema fijo:**
  - **Tipografía:** cifras en sans heavy blanca; etiquetas en monoespaciada (la misma de los subtítulos).
  - **Color:** acento amarillo `#EDE13B`.
  - **Fondos:** paneles oscuros translúcidos o un halo oscuro suave.
- **Animación:** ease-out de 0,25–0,4 s, escritura tipo máquina para etiquetas mono, sin rebotes exagerados.
- **Formatos útiles para contenido institucional:**
  - Etiqueta de ubicación con pin.
  - Rótulo de nombre.
  - Cifra con contador.
  - Anillo de porcentaje.
  - Lista de fichas.
  - Cita en dos líneas con subrayado.
  - Tarjeta comparativa (p. ej. EnCicla público vs Biciclick privado).
  - Minimapa con ruta animada para los respiros.
- **Zona segura de Reels (1080×1920):**
  - Arriba ~270 px y abajo ~670 px tapados por la interfaz.
  - Riel de botones a la derecha (~230 px) en la parte baja.
  - Subtítulos con MarginV ≈ 520 y ≤ 26 caracteres por línea.

## Que la voz no se sienta cortada
- **Tomas largas continuas** en vez de frases sueltas: la respuesta completa de una persona en un solo bloque, con sus pausas naturales. El B-roll tapa la imagen, no el audio.
- **Cortes solo entre oraciones**, con **cola y pre-roll** de ambiente del mismo micrófono, y **crossfade de igual potencia de 0,12–0,2 s** que solape los dos trozos.
- **J-cut:** la voz entra 1–3 s antes de ver a la persona.
- **Sin denoiser agresivo** (afftdn suena procesado). Basta con un highpass a 80 Hz y compresión suave (2,5:1).
- **Revisar la forma de onda en cada unión:** un hueco plano de 0,1–0,2 s se oye como corte, y se arregla adelantando el siguiente trozo para que solape.
- **Corregir tartamudeos** en el subtítulo ("o un, un vehículo" → "o un vehículo") pero dejarlos en el audio.

## Mezcla
- Música bajo la voz −15 dB (rampas de 0,5 s).
- En los respiros, al nivel de la voz.
- En el cierre, +2–3 dB.
- Master −14 LUFS / −1 dBTP.
