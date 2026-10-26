# fdd

Feature-Driven Development para agentes de IA (Claude Code, Codex, Kiro y cualquiera que lea
skills en Markdown).

> Borrador. Se publica después de probarlo en una feature real.

Un agente que retoma una feature de otra sesión reconstruye el contexto de cero. FDD
(De Luca y Coad, 1997) lo resolvía para equipos humanos con un modelo del dominio y seguimiento por
feature. Esta skill toma esas piezas y les suma la ficha y la exigencia de evidencia de FTD:

- **Mapa del dominio vivo**: un índice de entidades, flujos y dónde vive cada uno.
- **Features con nombre de resultado**: «acción + resultado + objeto».
- **Una ficha por feature**, con criterios de aceptación, archivos con dueño y evidencia.
- **Avance por hitos**: diseñado, implementado, verificado, integrado. El último lo marca una persona.

La skill está en [`skills/fdd/SKILL.md`](skills/fdd/SKILL.md).

## Instalación

Claude Code lee `~/.claude/skills/<nombre>`; Codex, `~/.agents/skills/<nombre>`. Un enlace a
`skills/fdd` en cada uno alcanza.

## Fuentes

- Feature-Driven Development: Jeff De Luca y Peter Coad, 1997; Palmer y Felsing,
  *A Practical Guide to Feature-Driven Development* (2002).
- FTD (Fast-Track Development) de [RSC](https://github.com/ericrisco/rsc-harness), licencia MIT.

## Licencia

MIT.
