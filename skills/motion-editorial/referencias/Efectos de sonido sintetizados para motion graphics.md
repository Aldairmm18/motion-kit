# Efectos de sonido sintetizados para motion graphics

Hecho el 2026-10-22 en un reel político para el collage de papel. Los sonidos se sintetizan con numpy, sin bancos de sonido ni whooshes. Ver también Motion graphics editoriales con Claude y After Effects a Premiere con mogrt por script.

## La idea
- **Un WAV por gráfico, del largo del gráfico**, con cada sonido en el segundo exacto de su keyframe en AE. Así se pone en una pista propia (A4) justo donde empieza el .mogrt y queda sincronizado sin tocar nada más.
- Los tiempos de los eventos salen de los mismos números que usa el script de AE (entrada de cada capa, letras de la máquina, trazo del resaltador). Si se cambia la animación, se cambia el evento y se vuelve a generar.
- **Pista aparte a −8 dB:** no se mezcla con la voz (A1) ni con el obturador de las ráfagas (A3).
- **Formato:** 48 kHz, estéreo, 24 bits.

## Recetas
Se filtra ruido blanco por bandas con FFT y se le da forma con una envolvente de ataque y caída.
- **Papel que se desliza:** ruido de 0,9 a 7,5 kHz con aleteo, más un golpe seco al final.
- **Máquina de escribir:** por cada letra, un clic agudo (2–6 kHz, 4 ms) y un golpe grave (180 Hz). Se varía un poco la fuerza y el tiempo de cada letra para que no suene a máquina perfecta.
- **Marcador o resaltador:** ruido de 1 a 5 kHz modulado a unos 8 Hz (el trazo), con un chillido suave de fieltro.
- **Lápiz (flecha punteada):** ruido de 2,2 a 7 kHz modulado a 6–14 Hz.
- **Sello:** golpe de 95 Hz más una palmada de papel.
- **Moneda:** campana inarmónica con rebotes.
- **Cinta adhesiva:** ráfaga de granitos de ruido (se despega y se pega).
- **Golpes suaves** (por ejemplo, edificios que suben): un pop grave corto, en lugar de la máquina de escribir.

## Trampas
- **Normalizar la mezcla completa al final** (pico a −6 dBFS) y bajar el nivel en Premiere, debajo de la voz.
- **Al cambiar un WAV ya puesto en Premiere,** hay que refrescar el medio o volver a poner el clip, porque Premiere guarda el audio en caché. Si el clip tiene un fundido o un ajuste a mano, se reemplaza solo ese clip; no hay que rehacer toda la pista.
- **Hay que escucharlos en Premiere**: generarlos no prueba que suenen bien.
