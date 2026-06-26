# berserk-arquitect

Skill de **fase de arquitectura** para Claude Code / Cowork. Antes de escribir una línea de código, interroga el proyecto hasta dejarlo blindado y cristaliza la arquitectura en dos archivos que se mantienen solos. Después, un modelo más potente en una terminal ejecuta las etapas una por una.

## Qué hace

1. **Interrogatorio sin piedad** (autocontenido) — expone huecos, supuestos no dichos y contradicciones antes de codear. Una falla estructural por ronda, adversarial, persigue respuestas esquivadas y cierra recitando el diseño antes de generar nada.
2. **Genera dos archivos núcleo sin solapamiento:**
   - `biblia.md` — la verdad completa y estable: visión, negocio, contexto, stack, arquitectura de datos, fases/etapas/work-orders, y las tres secciones vivas (BUGS / STEPS / MEJORAS TÉCNICAS).
   - `CLAUDE.md` — entrada operativa corta: regla de oro, puntero a la biblia, estado actual (fase/etapa/último paso) y protocolo de retoma.
3. **Fases reanudables** — cualquiera puede detenerse y retomar exactamente en el último paso ejecutado.
4. **Regla de modularización** — deja escrita la regla para que tareas repetibles (tipo `SKIN_GENERATOR.md`) se modularicen en su propio MD, disparadas bajo demanda.
5. **Export docx** opcional — snapshot presentable de la biblia.

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

## Licencia

MIT.
