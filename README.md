# berserk — marketplace de skills

Marketplace de Claude Code / Cowork con herramientas para el flujo de trabajo de desarrollo.

Plugin disponible:

- **berserk-arquitect** — fase de arquitectura para proyectos nuevos. Interroga la lógica de negocio y de software sin piedad y genera dos archivos núcleo sin solapamiento (`biblia.md` + `CLAUDE.md`), con fases reanudables y secciones vivas de bugs, steps y mejoras técnicas.

## Instalación

En Claude Code o Cowork, agregá este marketplace y después instalá el plugin:

```
/plugin marketplace add <TU-USUARIO-GITHUB>/berserk-arquitect-marketplace
/plugin install berserk-arquitect@berserk
```

> Reemplazá `<TU-USUARIO-GITHUB>` por tu usuario real de GitHub una vez que subas el repo.

Para actualizar a una versión nueva:

```
/plugin marketplace update berserk
```

## Cómo se usa el skill

Una vez instalado, el skill se dispara solo cuando arrancás un proyecto y decís cosas como "armemos la biblia", "fase de arquitectura", "generá el CLAUDE.md" o "auditá el diseño antes de codear". También sirve para formalizar un proyecto que ya existe.

Ver el detalle completo en [`plugins/berserk-arquitect/README.md`](plugins/berserk-arquitect/README.md).

## Contribuir

Issues y PRs bienvenidos. Si probás el skill y encontrás que falta cubrir un caso, abrí un issue contando qué proyecto estabas armando y dónde se quedó corto.

## Licencia

MIT — ver [LICENSE](LICENSE).
