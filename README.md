# berserk — marketplace de skills

Marketplace de Claude Code / Cowork con herramientas para el flujo de trabajo de desarrollo.

Plugin disponible:

- **berserk-arquitect** — fase de arquitectura para proyectos nuevos. Interroga la lógica de negocio y de software sin piedad y genera dos archivos núcleo sin solapamiento (`biblia.md` + `CLAUDE.md`), con fases reanudables y secciones vivas de bugs, steps y mejoras técnicas.

## Qué resuelve

La mayoría de los proyectos no se rompen por mal código, sino por decisiones de negocio y de datos que nunca se cuestionaron a tiempo. `berserk-arquitect` mete toda esa fricción al principio —cuando cambiar de idea es gratis— y deja la arquitectura documentada de forma que sobreviva cortes de contexto, cambios de modelo y el paso del tiempo. En concreto:

- **Interrogatorio adversarial** que expone huecos y contradicciones antes de codear.
- **Dos archivos que se mantienen solos**: la biblia (verdad completa) y el CLAUDE.md (operativo + estado), sin contenido duplicado, así nunca hay que revisar si están sincronizados.
- **Fases reanudables**: cualquiera retoma exactamente en el último paso ejecutado.
- **Registros vivos** de bugs, steps y mejoras técnicas, separados para no mezclar roadmap con defectos.
- **Modularización** de tareas repetibles en su propio MD, y export `.docx` presentable bajo demanda.

## Instalación

En Claude Code o Cowork, agregá este marketplace y después instalá el plugin:

```
/plugin marketplace add lucascastro29/berserk-arquitect-marketplace
/plugin install berserk-arquitect@berserk
```

Para actualizar a una versión nueva:

```
/plugin marketplace update berserk
```

## Cómo se usa el skill

Una vez instalado, el skill se dispara solo cuando arrancás un proyecto y decís cosas como "armemos la biblia", "fase de arquitectura", "generá el CLAUDE.md" o "auditá el diseño antes de codear". También sirve para formalizar un proyecto que ya existe.

Ver el detalle completo en [`plugins/berserk-arquitect/README.md`](plugins/berserk-arquitect/README.md).

## Contribuir

Issues y PRs bienvenidos. Si probás el skill y encontrás que falta cubrir un caso, abrí un issue contando qué proyecto estabas armando y dónde se quedó corto.

## Inspiración y créditos

La fase de interrogatorio está inspirada en **grill-me** y su sesión `/grilling` — un interrogatorio implacable para afilar un plan o diseño. `berserk-arquitect` toma ese espíritu, lo incorpora de forma autocontenida y le suma la generación y el mantenimiento de los dos archivos núcleo.

Repo original de grill-me (por Matt Pocock): https://github.com/mattpocock/skills

## Licencia

MIT — ver [LICENSE](LICENSE).
