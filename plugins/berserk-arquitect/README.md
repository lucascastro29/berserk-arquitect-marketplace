# berserk-arquitect

Skill de **fase de arquitectura** para Claude Code / Cowork. Antes de escribir una línea de código, interroga el proyecto hasta dejarlo blindado y cristaliza la arquitectura en dos archivos que se mantienen solos. Después, un modelo más potente en una terminal ejecuta las etapas una por una.

## Por qué existe

Codear sobre una arquitectura floja es la forma más cara de descubrir que estaba floja. La mayoría de los proyectos se rompen no por mal código sino por decisiones de negocio y de datos que nunca se cuestionaron a tiempo. Este skill mete toda esa fricción al principio —cuando cambiar de idea es gratis— y deja la decisión documentada de forma que sobreviva cortes de contexto, cambios de modelo y el paso del tiempo.

## Utilidades

### 1. Interrogatorio de arquitectura (autocontenido)
Antes de generar nada, somete el proyecto a un interrogatorio adversarial: una falla estructural por ronda, ataca primero lo más load-bearing (borde del sistema, modelo de datos, modelo de negocio), persigue las respuestas esquivadas y confronta con la evidencia de repos o docs previos cuando existen. Cierra recitando el diseño final y pidiendo confirmación literal: no escribe un archivo con un borde abierto. No depende de ningún skill externo —la lógica de grilling vive adentro.

### 2. Dos archivos núcleo sin solapamiento
- `biblia.md` — la verdad completa y estable: visión, modelo de negocio, contexto del usuario/equipo, stack y su porqué, arquitectura de datos e invariantes, alcance (qué entra y qué **no**), fases/etapas/work-orders, y las tres secciones vivas.
- `CLAUDE.md` — entrada operativa corta: regla de oro, puntero a la biblia, estado actual (fase/etapa/último paso) y protocolo de retoma.

### 3. Fases reanudables con estado de fuente única
El desarrollo se corta en fases → etapas → work-orders atómicos. `CLAUDE.md` mantiene un único puntero al último paso ejecutado y el próximo paso exacto, así cualquiera (vos, otro dev o un modelo más potente en una terminal) puede detenerse y retomar sin releer todo ni reescribir lo que ya funciona.

### 4. Tres registros vivos separados
- **STEPS de etapas** — el plan en ejecución con sus casillas.
- **BUGS** — defectos fuera del flujo normal, con estado, etapa, fecha y duración de corrección.
- **MEJORAS TÉCNICAS** — cambios de calidad técnica (ni feature ni bug), con impacto medible.

Mantenerlos separados evita que un bug se confunda con una etapa planificada o que una mejora ensucie el roadmap.

### 5. Regla de modularización
Deja escrita la regla para que las tareas repetibles con workflow propio (tipo `SKIN_GENERATOR.md`) se modularicen en su propio MD, con formato definido y referenciado de vuelta a la biblia. El skill no los genera solo: los disparás bajo demanda ("generá un md del flujo X reutilizable") y salen consistentes.

### 6. Export docx (snapshot)
Genera bajo demanda una versión presentable de la biblia en `.docx`, para devs o para leer cómodo. Es una foto con fecha, no un archivo vivo —así no agrega otra cosa que mantener sincronizada.

### 7. Mantenimiento sin drift
Se reusa en cada avance: cuando completás un paso, cerrás un bug o registrás una mejora, actualiza **solo el archivo que corresponde**. Como no hay contenido duplicado entre biblia y CLAUDE.md, nunca tenés que revisar a mano si quedaron sincronizados.

## La regla de oro: cero solapamiento = cero drift

`biblia.md` y `CLAUDE.md` nunca repiten contenido. Lo estable vive solo en la biblia; lo operativo y el estado mutable viven solo en CLAUDE.md. Como no hay duplicación, nunca tenés que revisar a mano si quedaron sincronizados.

## Cuándo se dispara

Cuando arrancás un proyecto y decís cosas como "armemos la biblia", "fase de arquitectura", "definamos la lógica de negocio", "generá el CLAUDE.md", "auditá el diseño antes de codear", o querés dejar un proyecto listo para ejecutarlo por etapas. También para formalizar un proyecto existente.

## Contenido

```
skills/berserk-arquitect/
├── SKILL.md
└── assets/
    ├── biblia.template.md
    ├── CLAUDE.template.md
    └── modulo.template.md
```

## Inspiración y créditos

La fase de interrogatorio de este skill está inspirada en **grill-me** y su sesión `/grilling` — la idea de un interrogatorio implacable para afilar un plan o diseño antes de ejecutarlo. `berserk-arquitect` toma ese espíritu y lo extiende: incorpora el grilling de forma autocontenida y le suma la generación y el mantenimiento de los dos archivos núcleo.

Repo original de grill-me (por Matt Pocock): https://github.com/mattpocock/skills

## Licencia

MIT.
