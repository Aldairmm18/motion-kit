# Escala de planos y montaje: del contexto al detalle

Guía que compartió el usuario el 2026-10-29 para editar con criterio de planos. La idea central es un **embudo de atención**: primero se responde *¿dónde estamos?* (contexto), después *¿quién está ahí?* (sujeto) y al final *¿qué pasa o qué siente?* (emoción o detalle). Se aplica en Miramar-Primer-Taller y sirve para cualquier video de recorrido o taller (Estilo reel político de recorrido, Estilo de edición talleres Efe Gómez).

## 1. Los planos, de mayor a menor

**Contexto (planos descriptivos)**
- **Gran Plano General (GPG):** escenario enorme (la ciudad, la ladera). La persona es un punto o no se ve. Función: geografía y ambiente.
- **Plano General (PG):** el entorno sigue mandando, pero la persona se ve de pies a cabeza. Muestra dónde está con respecto a su espacio.

**Acción (planos narrativos)**
- **Plano Americano / Tres Cuartos (PA):** corta en las rodillas. Bueno para interacciones físicas y gente en movimiento.
- **Plano Medio (PM):** corta en la cintura. El más natural, como se ve a alguien en una conversación. Equilibra cuerpo y gesto.

**Intimidad (planos expresivos y de acento)**
- **Primer Plano (PP):** de los hombros a la cabeza. Quita el entorno y obliga a mirar los ojos. Función emocional.
- **Primerísimo Primer Plano (PPP):** de la barbilla a la frente. Máxima tensión o emoción; invasivo y dramático. Usarlo poco.
- **Plano Detalle (PD):** un objeto o una parte del cuerpo que no es la cara (manos sembrando, la sirena, un megáfono). Da información clave.

## 2. Reglas de montaje

- **Regla de los 30 grados:** entre dos planos de la misma persona, la cámara debe cambiar al menos 30° de ángulo (o cambiar de escala de forma clara). Si no, el corte se lee como **jump cut**, un salto brusco que parece error. En entrevistas de una sola cámara: tapar los cortes internos con un plano de apoyo (B-roll), no dejarlos a la vista.
- **Motivación del corte:** no cerrar el plano "porque sí". El paso a un plano más cerrado lo justifica la historia (algo que se dice, una reacción, un gesto).
- **Continuidad (raccord):** al pasar de un plano a otro de la misma acción, el fondo, la luz y la posición de la persona y de los objetos tienen que coincidir.
- **Ley del eje (180°):** con dos personas hablando, todas las tomas quedan del mismo lado de la línea imaginaria entre ellas; si se cruza, parece que se dieron la vuelta.

## 3. El embudo en acción (ejemplo de la guía)
1. **GPG:** el bosque de noche bajo la lluvia (tono y lugar).
2. **PG:** la cabaña con una luz encendida (ubica la acción).
3. **PM:** un hombre escribe en una libreta (acción y estado).
4. **PP:** su cara sudando cuando escucha un crujido (emoción).
5. **PD:** la punta del bolígrafo rompe la hoja (tensión).

## Cómo lo aplicamos al editar por script
- **Ordenar el B-roll de cada bloque como embudo:** GPG o PG del barrio → PA/PM de la gente → PP de una cara → PD de un detalle (manos, objeto). En los respiros de música, el embudo puede ir de afuera hacia adentro y abrir otra vez en el siguiente.
- **Clasificar la escala de cada toma** con Apple Vision (`segmentar`): el alto de la persona más grande sobre el alto del cuadro da una aproximación — sin personas o < 15 % → GPG/PG; 15–55 % → PG/PA; 55–85 % → PM; > 85 % o cara cortada → PP/PPP. Un detalle (manos, objeto) se marca a mano o por la falta de cara con mucho movimiento cerca.
- **Evitar dos planos seguidos de la misma escala y del mismo lugar:** se lee como salto. Alternar escala o lugar en cada corte.
- **Movimiento de cámara:** paneos y desplazamientos suaves sí (dan vida); temblor, zooms bruscos y paneos rápidos no. Ver `movimiento.py` y `estabilidad_fx.py` en Miramar-Primer-Taller.

## Lecciones de la edición de Miramar (2026-10-29)
- No cortar la última palabra: dejar ~0,35 s de cola después del final de la palabra (tiempos de Scribe).
- Mejor un párrafo seguido de la entrevista que frases sueltas pegadas: los cortes internos se notan aunque se tapen.
- Si hay que quitar una muletilla en medio, fundido de entrada corto (0,15 s) en el audio y un plano de apoyo encima.
- B-roll solo de tramos quietos y en cámara lenta real (0,6 con material a 50 fps); si una toma se mueve mucho, mejor dejar al orador en cámara.
- Nada de tramos de cámara de ~1 s entre dos planos de apoyo: se ve como parpadeo.
- Cronología: el B-roll de cada momento sale de lo que pasó en ese momento (el número de clip ayuda a ordenarlo); el final, con el final real.
