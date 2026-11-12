# Blender por script

- Blender corre sin abrir la ventana: `blender -b -P script.py -- args`. Todo (modelos, materiales, luces, cámara, animación) se arma con `bpy`.
- En Mac usar Cycles con GPU: `prefs.compute_device_type = "METAL"`, `scene.cycles.device = "GPU"`. **El primer render tarda minutos** porque compila los kernels de Metal; después baja a segundos por cuadro.
- Fondo transparente: `render.film_transparent = True` + un plano con `is_shadow_catcher = True` para conservar la sombra del piso.
- Para colores de marca exactos: `view_settings.view_transform = "Standard"` (AgX los desatura) y convertir los hex de sRGB a lineal.
- **Blender 5.2 + Metal se cae a mitad del render** (crash en `MetalKernelPipeline::compile`, compila kernels optimizados en segundo plano). Solución: `prefs.kernel_optimization_level = "OFF"`. Con eso los renders largos terminan sin cortes.
- Un render largo se puede cortar: conviene poder retomar con `scene.frame_start = N`.
- Con `use_persistent_data` y Cycles en Metal salieron bloques de cuadros con el acero en negro, sin ningún error en el log. Sin ese ajuste no se repitió. Después de cada render, comparar todos los cuadros con SSIM (`ffmpeg ssim=stats_file`) o revisarlos: un vistazo no lo detecta.
- Render híbrido por tramos: Eevee en planos mate, Cycles en metal. En el piloto del termo bajó de 2 h 46 min a unos 50 min sin cambio visible en cuadros sueltos.
- Velocidad: compite con otras apps pesadas abiertas (p. ej. Remotion Studio). Eevee para pruebas, Cycles para el final.

Usado en Motion-graphics-ia.
