# motion-kit

Criterio de diseño y métodos para hacer videos, motion graphics, color y presentaciones con Claude Code.

## Instalar

En Claude Code:

```
/plugin marketplace add Aldairmm18/motion-kit
/plugin install motion-kit@motion-kit
```

Para probarlo desde una carpeta local: `/plugin marketplace add /ruta/a/motion-kit`.

Recomendado además, para Remotion: `npx skills add remotion-dev/skills`.

## Qué trae

| Skill | Para qué |
|---|---|
| `principios` | El criterio común: historia primero, voz sin cortes, un acento, color en capa de ajuste, todo editable. |
| `linea-de-historia` | Armar la historia antes de editar: gancho, 6 pasos, "pero / por eso", huecos de B-roll, embudo de planos. |
| `video-taller` | Talleres, comunidad, fundaciones: entrevistas, rótulos, subtítulos, respiros, cierre con logos. |
| `reel-recorrido` | Reels de recorrido político o de territorio: gente, subtítulos mínimos, cortes al ritmo. |
| `motion-editorial` | Motion graphics de estilo editorial, subtítulos dinámicos, texto detrás de personas, foley. |
| `color` | Corrección y LUTs: S-Log3, iPhone, Lumetri, teal & orange. |
| `adobe-por-script` | After Effects, Premiere, Resolve y Blender por script en Mac. |
| `ppt-animado` | PowerPoint con animaciones y música por código. |

Las skills se activan solas según lo que pidas ("edita este taller", "ponle motion graphics", "dale color"). Cada una tiene una carpeta `referencias/` con el detalle técnico, que Claude lee solo cuando lo necesita.

## Notas
- Está pensado para Mac (osascript, Apple Vision) y con los programas en español.
- Algunas referencias nombran scripts de proyectos originales que no vienen en este repo: sirven como descripción del método, no como archivos para ejecutar.
