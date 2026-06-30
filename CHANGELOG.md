# Changelog — berserk-arquitect

Formato basado en "issue detectado → solución". Las versiones siguen [SemVer](https://semver.org/lang/es/).

## [0.3.0] — 2026-06-30

### Issue detectado — La biblia no se conectaba con el tablero donde el equipo ejecuta

**Síntoma:** el roadmap (etapas/work-orders) y las secciones vivas (BUGS, MEJORAS TÉCNICAS)
vivían solo en `biblia.md`. Para llevar el avance a un gestor de tareas (Notion / ClickUp)
había que copiar tareas a mano y, peor, el estado de ejecución del tablero no volvía a la
biblia. Eso o desincronizaba el plan, o convertía al tablero en una segunda fuente de verdad.

**Causa raíz:** el skill no tenía ningún puente con herramientas externas, y un puente naíf
(sync simétrico) habría roto el principio de fuente única que sostiene todo el método.

### Solución — Integración bidireccional asimétrica con Notion / ClickUp (opcional)

- **`SKILL.md`:** nuevo **Paso 7 — Integración con gestor de tareas (Notion / ClickUp)**.
  Bidireccional pero **asimétrica**: la biblia es dueña de estructura y contenido (push
  biblia → tablero); el tablero es dueño del estado de ejecución (pull tablero → biblia).
  Cada dato tiene un solo dueño, así no hay drift. Mapeo etapa→tarea / work-order→subtarea;
  las tres secciones vivas se distinguen por **label** (`steps`/`bugs`/`mejoras`);
  reconciliación por **ID de correlación** estable, no por título. Atado a la guardia de
  frescura. Parches a la descripción, "Qué produce" y "Qué NO hace".
- **`assets/biblia.template.md`:** nueva sección **"Integración con gestor de tareas"** con
  la config estable del vínculo (conector, destino, label, mapeo de estados).
- **`assets/CLAUDE.template.md`:** nuevo bloque **"Estado de sincronización con tablero"**
  con el estado mutable de la sync. Cero-solapamiento con la config de la biblia.
- **`README.md`:** nueva §11.5 documentando la integración + línea en el cheat sheet.

### Cambios

- Bump de versión del plugin: `0.2.0` → `0.3.0` (`plugin.json` y entrada en `marketplace.json`).

## [0.2.0] — 2026-06-26

### Issue detectado — Drift de artefactos derivados de `biblia.md`

**Síntoma:** al retomar un proyecto, un artefacto derivado de la biblia (un brief para
otra herramienta, un módulo de tarea, el export docx) se tomaba como vigente sin
verificar que la biblia no hubiera avanzado después de generarlo. En el proyecto real
(`berserk`), un `CLAUDE_DESIGN_BRIEF.md` quedó desactualizado porque dos mejoras técnicas
(MT-002 orden reordenable y MT-003 rutinas personalizadas) agregaron superficies de UI
**después** de generado el brief, y casi se manda esa foto vieja a la herramienta de diseño.

**Causa raíz:** el skill garantizaba la no-duplicación entre `biblia.md` y `CLAUDE.md`,
pero no tenía ninguna regla para los **derivados** de la biblia, que son fotos con fecha y
envejecen apenas la biblia cambia.

### Solución — Guardia de frescura

- **`SKILL.md`:** nueva sección **"Guardia de frescura: nunca actuar sobre un derivado
  viejo"**. Antes de ejecutar/mandar/entregar/cerrar una acción sobre un derivado, se lee
  su **sello** (`Sincronizado con biblia.md hasta: AAAA-MM-DD — última entrada: <ref>`) y se
  escanean STEPS / BUGS / MEJORAS TÉCNICAS por entradas con fecha posterior; si hay algo más
  nuevo que toca su alcance, se reconcilia primero. Punteros añadidos en los pasos de
  módulos, docx y uso recurrente para que todo derivado lleve el sello.
- **`assets/CLAUDE.template.md`:** la guardia incorporada a "CÓMO TRABAJAR" para que todo
  proyecto nuevo nazca con ella.
- **`mtime` como señal secundaria:** se documenta explícitamente que la fecha de
  modificación del archivo es frágil ante `git`/copias; el sello embebido es la fuente
  primaria de frescura.

### Cambios

- Bump de versión del plugin y del marketplace: `0.1.0` → `0.2.0`.
