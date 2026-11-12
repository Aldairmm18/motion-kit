# LUT teal and orange con neones protegidos

Con los filtros de color de ffmpeg (`colorbalance`, `selectivecolor`) un look teal & orange vuelve **grises** los chalecos reflectivos (verde-amarillo neón), porque los trata igual que el pasto o los medios tonos.

**Solución:** generar un LUT 3D (`.cube`, 33³) con numpy, donde cada color se decide por su tono, saturación y brillo:
- máscara de piel (tono ~28°, saturación media) → se calienta y queda fuera del tinte teal;
- máscara de neón (tono ~78°, saturación > 0,25, brillo > 0,55) → sin tinte y saturación ×1,6;
- pasto (tono ~100°, más oscuro) → un poco hacia teal;
- sombras → teal, luces altas → cálidas.

Se aplica con `lut3d=file=...:interp=tetrahedral` **después** del tonemap HDR→SDR (`zscale` + `tonemap=hable`).

Contexto: Video-use (tráiler Biciclick, 2026-10-16).
