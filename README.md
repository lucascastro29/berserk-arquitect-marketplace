# README — Workflow de arquitectura (`berserk-arquitect`)

> Este repo se diseña y documenta con el skill **`berserk-arquitect`**. Antes de escribir código, se interroga el proyecto hasta dejarlo blindado y se cristaliza esa arquitectura en dos archivos que se mantienen solos. Después, un modelo potente en una terminal ejecuta las etapas una por una.
>
> Este README explica **cómo funciona el skill y cómo usarlo**. No es la verdad del proyecto (esa vive en `biblia.md`) ni el estado de avance (ese vive en `CLAUDE.md`). Es el manual del método.

---

## 1. La idea en una frase

El skill se llama "arquitecto" por una razón: **su trabajo no es codear, es diseñar y documentar tan bien que codear sea casi mecánico.** Codear sobre una arquitectura floja es la forma más cara de descubrir que estaba floja.

El skill hace tres cosas:

1. **Interroga sin piedad** el proyecto para exponer huecos, supuestos no dichos y contradicciones *antes* de escribir nada.
2. **Cristaliza** la arquitectura confirmada en dos archivos núcleo sin contenido duplicado.
3. **Mantiene** esos archivos sincronizados a medida que el proyecto avanza.

---

## 2. Los dos archivos núcleo

| Archivo | Qué es | Contiene |
|---------|--------|----------|
| **`biblia.md`** | La verdad completa y estable. El *qué* y el *por qué*. | Visión, negocio, contexto, stack, arquitectura de datos, fases con etapas y work-orders, y las tres secciones vivas (STEPS, BUGS, MEJORAS TÉCNICAS). |
| **`CLAUDE.md`** | La entrada operativa, corta. El *cómo trabajar* y el *dónde quedamos*. | Reglas para quien codee, puntero a la biblia, el **estado actual** (fase/etapa/último paso) y el protocolo de retoma. |

Y, **bajo demanda** (no automáticamente):

- **Módulos de tarea** — archivos MD tipo `SKIN_GENERATOR.md` para flujos repetibles. El skill deja escrita la *regla* de cuándo crearlos; vos los disparás después.
- **Export docx** — un snapshot presentable de la biblia para leer cómodo o pasar a alguien. No es un archivo vivo: es una foto con fecha.

---

## 3. La regla de oro: cero solapamiento = cero drift

Los dos archivos están **siempre sincronizados sin que tengas que revisarlo a mano**, y la única forma de garantizarlo es que **no repitan contenido**. Dos archivos que dicen lo mismo siempre terminan divergiendo.

Por eso la separación es tajante:

- Todo lo estable y completo vive **solo en `biblia.md`**.
- Todo lo operativo y el estado mutable viven **solo en `CLAUDE.md`**.
- El estado que cambia seguido (qué etapa, qué paso, qué bug abierto) tiene **una sola fuente**: `CLAUDE.md` lo lleva; `biblia.md` no lo repite, solo lo referencia.

Cuando algo cambia, se actualiza **el archivo que corresponde**, no los dos. Como no hay duplicación, no hay nada que "chequear si quedó sincronizado".

**Regla práctica para decidir dónde va algo:** ¿es verdad estable del proyecto? → `biblia.md`. ¿Es instrucción de cómo trabajar o estado de dónde vamos? → `CLAUDE.md`.

---

## 4. Cómo invocarlo

Es un skill de plugin. Se invoca con su nombre y un argumento que describe qué querés hacer:

```
/anthropic-skills:berserk-arquitect <qué querés hacer>
```

Ejemplos reales de invocación:

```
/anthropic-skills:berserk-arquitect armemos la biblia de un proyecto nuevo
/anthropic-skills:berserk-arquitect auditá el diseño antes de codear
/anthropic-skills:berserk-arquitect trabajemos sobre este repo
/anthropic-skills:berserk-arquitect avancé la etapa 2.1, actualizá el estado
/anthropic-skills:berserk-arquitect generá un md reutilizable del flujo X
/anthropic-skills:berserk-arquitect pasame la biblia a docx
```

Para usarlo en una terminal con Claude Code, el plugin tiene que estar instalado en ese entorno. Si no aparece al tipear `/`, no está instalado ahí — y no pasa nada: `CLAUDE.md` por sí solo ya rutea al modelo.

---

## 5. Modo A — Inicio de un proyecto (o formalizar uno existente)

Este es el flujo principal. Cinco pasos.

### Paso 1 — Interrogatorio sin piedad

El skill te grillea. No dispara veinte preguntas sueltas: ataca **una falla estructural por ronda**, empezando por lo más *load-bearing* (el modelo de negocio, el borde del sistema, el modelo de datos). Si está mal contestada, tira todo abajo.

Cómo es un buen grill:

- **Concreto y adversarial, no genérico.** No "¿cómo monetiza?" sino "decís precio único, pero ¿qué pasa cuando saques una skin nueva post-lanzamiento — los que ya compraron la reciben gratis o nunca más vendés una skin?".
- **Persigue las respuestas esquivadas.** Si contestás con un requisito nuevo en vez de responder, vuelve sobre la pregunta original.
- **Confronta con evidencia.** Si hay repos o docs previos, los lee y usa las contradicciones reales como munición.
- **Cierra explícitamente.** Cuando converge, recita el diseño final en una pasada y pide **confirmación literal** antes de generar nada.

Frentes que cubre antes de cerrar: visión y propuesta de valor · modelo de negocio · quién es el usuario · stack y por qué · arquitectura de datos · qué entra y qué NO entra en el alcance · cómo se corta en fases y etapas · qué tareas merecen módulo propio · qué cosas nunca se deben hacer.

### Paso 2 — Parir `biblia.md` y `CLAUDE.md`

Recién con el diseño confirmado, genera los dos archivos respetando el cero-solapamiento.

### Paso 3 — Modelo de fases reanudable

El plan se corta en **fases → etapas → work-orders** (tareas atómicas, una por sesión de terminal). Cada etapa lista sus pasos con casillas `[ ]` / `[x]`, y `CLAUDE.md` mantiene el puntero único de "dónde vamos". Ver §8.

### Paso 4 — Las tres secciones vivas

Quedan escritas en `biblia.md` para registrar el proyecto a lo largo del tiempo. Ver §9.

### Paso 5 — Regla de modularización

Se deja escrita la regla de cuándo sacar un flujo repetible a su propio MD. Ver §10.

---

## 6. Modo B — Auditar un diseño existente

Si ya tenés un proyecto (con o sin docs), el skill lo formaliza y lo audita. La diferencia con el Modo A es que **arranca leyendo lo que ya existe** y usa las contradicciones reales como munición del interrogatorio.

Cómo usarlo para auditar:

```
/anthropic-skills:berserk-arquitect auditá este repo antes de que siga codeando
```

Qué hace:

1. **Lee el código y los docs reales** (no pregunta al aire).
2. **Busca contradicciones** entre lo que el doc dice y lo que el repo hace. Ejemplo real de esta sesión: el `CLAUDE.md` viejo decía que el proyecto estaba en "FASE 0 monolito", pero el repo ya estaba migrado a React — esa contradicción fue la primera munición.
3. **Grillea los bordes abiertos** del negocio y del diseño, uno por ronda.
4. **Cristaliza** el diseño auditado en los dos archivos, marcando lo que ya funciona como `[x]` y lo que falta como `[ ]`.

La auditoría no reescribe lo que ya anda: lo documenta y sigue desde el próximo paso.

---

## 7. Modo C — Mantener sincronizado (uso recurrente)

Cada vez que avanzás —completás un paso, cerrás un bug, registrás una mejora— invocás el skill para que actualice **solo el archivo que corresponde**:

```
/anthropic-skills:berserk-arquitect cerré la etapa 2.1 y encontré un bug, actualizá
```

1. Identifica qué cambió y a qué archivo le toca (estado → `CLAUDE.md`; verdad del proyecto → `biblia.md`).
2. Actualiza **solo ese archivo**.
3. No copia el cambio al otro. La separación garantiza que sigan coherentes.

Así nunca tenés que revisar a mano si los dos están al día.

---

## 8. El modelo de fases (reanudable)

El desarrollo se corta así:

```
FASE        → un hito grande de producto (ej: "v1.0 en stores")
  ETAPA     → un bloque de trabajo dentro de la fase (ej: "Capacitor setup")
    WORK-ORDER → una tarea atómica, tomable de a una por sesión ([ ] / [x])
```

Es **reanudable**: cualquiera puede parar y retomar exactamente en el último paso, sin releer todo. Lo que lo hace posible:

- Las casillas `[ ]`/`[x]` viven en `biblia.md` §8 (el plan con su avance).
- El **puntero único** vive en `CLAUDE.md → ESTADO ACTUAL`: fase activa, etapa activa, último paso completado, próximo paso exacto, último archivo tocado.
- El **protocolo de retoma** (en `CLAUDE.md`) dice: al arrancar una sesión, leé el ESTADO ACTUAL antes que nada, no reescribas lo que ya funciona, seguí desde el próximo paso.

**Regla de ejecución:** una etapa por sesión. No mezclar etapas ni adelantar fases.

---

## 9. Las tres secciones vivas (en `biblia.md`)

Separadas a propósito, para que un bug no se confunda con una etapa planificada ni una mejora ensucie el roadmap:

| Sección | Qué registra | Atributos |
|---------|--------------|-----------|
| **STEPS de etapas** | El avance de fases/etapas con sus casillas. El plan en ejecución. | casillas `[ ]`/`[x]` |
| **BUGS** | Defectos hallados fuera del flujo normal de etapas. | estado · etapa donde se reconoció · fecha · duración de corrección · descripción · fix |
| **MEJORAS TÉCNICAS** | Cambios de calidad técnica que no son ni feature ni bug. | estado · etapa de referencia · fecha · duración · impacto medible · descripción |

---

## 10. Regla de modularización

Las tareas repetibles con workflow propio **no** viven como sección de la biblia: se sacan a su propio MD y se referencian desde ella.

En este proyecto:

- **`SKIN_GENERATOR.md`** — flujo para crear una skin nueva (existe).
- **`GACHA_BANNER_GENERATOR.md`** — flujo para dar de alta un banner/temporada (a crear en Fase 3).

Se disparan explícitamente: "generá un md reutilizable del flujo X". El skill no los crea por su cuenta.

---

## 11. Export docx (snapshot)

Si pedís "pasame la biblia a docx" o "una versión presentable para los devs", el skill genera un **snapshot** de la biblia en docx. **No es un cuarto archivo a sincronizar** — es una foto con fecha. Tratarlo como vivo devolvería el problema de drift.

---

## 12. Ejemplo completo, sacado de este proyecto

Así se vio el flujo real al formalizar **Routine Universe** (codename del repo: `berserk`):

1. **Invocación:** `/anthropic-skills:berserk-arquitect trabajemos sobre este repo`.
2. **Auditoría:** el skill leyó el repo y detectó que el `CLAUDE.md` monolítico decía "FASE 0 monolito" mientras el código ya estaba migrado a React. Contradicción → munición.
3. **Grill, una falla por ronda:**
   - *Ronda 1 — negocio:* "Decís 'skins desbloqueables' y a la vez 'todas incluidas en la compra'. No pueden ser ciertas las dos. ¿El catálogo es fijo o expandible y monetizable?" → se eligió expandible.
   - *Ronda 2 — funnel:* "¿app gratis o paga?" → gratis.
   - *Rondas 3-4 — gacha:* "¿con qué se paga la tirada?" y "¿qué hay en el pozo y qué pasa con los duplicados?" → híbrido con tirada gratis por entrenar + packs pagos; duplicados → fragmentos.
   - *Ronda 5 — alcance:* el usuario sumó 3D, pets, ranking, temporadas de golpe → el skill frenó el scope creep y forzó la pregunta: "¿app de fitness con gancho gacha, o juego gacha con tema fitness?" → fitness con gancho, **dividido por fases**.
   - *Ronda IP:* "No podés vender skins de Berserk/Evangelion — es IP ajena. ¿Confirmás pivote a IP 100% original?" → sí, con excepción de skins privadas whitelisted.
4. **Cierre explícito:** el skill recitó el diseño final y pidió confirmación literal antes de tocar archivos.
5. **Cristalización:** generó `biblia.md` (verdad) + reescribió `CLAUDE.md` (corto), y agregó el gate de copyright a `SKIN_GENERATOR.md`.

El resultado: cada decisión load-bearing quedó cerrada *antes* de codear, y documentada en el archivo correcto.

---

## 13. Handoff a la terminal (ejecución)

El skill **no ejecuta las etapas** — eso lo hace un modelo potente en una terminal. El loop de ejecución:

1. Abrís la terminal en la raíz del repo y lanzás Claude Code (`claude`).
2. Prompt de arranque:
   ```
   Leé CLAUDE.md y después biblia.md completa. Tomá la etapa que indica
   ESTADO ACTUAL, work-order por work-order. Al cerrar cada paso, marcá la
   casilla en biblia.md §8 y actualizá ESTADO ACTUAL en CLAUDE.md. Commit
   chico por work-order. No mezcles etapas ni adelantes fases.
   ```
3. Cada sesión: lee el estado → ejecuta una etapa → marca casillas → mueve el puntero → commitea. Si se corta el contexto, el protocolo de retoma deja todo listo para la próxima.

Podés invocar el skill al inicio de cada sesión de terminal **en modo mantenimiento** (no interrogatorio) para que respete la disciplina de docs mientras codea.

---

## 14. Qué NO hace el skill

- No ejecuta las etapas de desarrollo (eso es el modelo de terminal).
- No genera módulos de tarea por su cuenta (solo deja la regla; los disparás vos).
- No mantiene el docx como archivo vivo (es snapshot bajo demanda).
- **Nunca duplica contenido entre `biblia.md` y `CLAUDE.md`.**

---

## 15. Cheat sheet

```
INICIO       /anthropic-skills:berserk-arquitect armemos la biblia
AUDITORÍA    /anthropic-skills:berserk-arquitect auditá el diseño antes de codear
AVANCE       /anthropic-skills:berserk-arquitect cerré la etapa X, actualizá el estado
MÓDULO       /anthropic-skills:berserk-arquitect generá un md reutilizable del flujo X
SNAPSHOT     /anthropic-skills:berserk-arquitect pasame la biblia a docx

Leé primero:   CLAUDE.md (dónde vamos) → biblia.md (la verdad completa)
Una sola fuente del estado:  CLAUDE.md → ESTADO ACTUAL
Una etapa por sesión.  No mezclar etapas.  No duplicar entre archivos.
```
