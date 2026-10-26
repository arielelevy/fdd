# FDD · Feature-Driven Development para agentes de IA

> **EN** · Feature-Driven Development, rebuilt for AI coding agents (Claude Code, Codex, Kiro and any
> agent that reads Markdown skills). One feature card per feature, a living domain map, milestone-based
> progress and file ownership, so another session can pick the work up without the chat.

**Estado: borrador en piloto.** Se está probando en una feature real de
[Lienzo](https://github.com/arielelevy/lienzo) antes de la versión 1.

---

## El problema

Un agente trabaja en sesiones cortas. Cuando una feature pasa de una sesión a otra (porque se
cortó el contexto, porque se repartió entre varios agentes o porque se retoma al día siguiente), la
sesión nueva reconstruye todo de cero: qué se pidió, qué se decidió, qué archivos toca, qué está hecho
y qué sólo fue afirmado.

Las metodologías para agentes que existen hoy tiran a dos extremos:

- **Spec-driven development (SDD):** especificación, diseño, plan y tareas antes de tocar código. Sirve
  para lo grande; para el trabajo de todos los días es ceremonia.
- **Sin proceso:** rápido hasta que hay que retomar, repartir o verificar.

## La idea

FDD (Jeff De Luca y Peter Coad, 1997) resolvía un problema parecido para equipos humanos grandes:
un **modelo del dominio** compartido y el trabajo partido en **features chicas con valor para el
usuario**, cada una con dueño y avance visible. Esta skill toma esas piezas, deja afuera lo que no le
sirve a un agente y le suma la ficha y la exigencia de evidencia de FTD.

| Pieza | Qué resuelve |
|---|---|
| **Mapa del dominio vivo** (`docs/dominio.md`) | La sesión nueva sabe qué entidades hay y dónde vive cada una sin explorar el repo |
| **Feature con nombre de resultado** | «avisar cuando una regla queda apagada», no «arreglar rules.py»: se entiende sola |
| **Una ficha por feature** | Objetivo, alcance, criterios, dominio, archivos, decisiones, evidencia y próximo paso, en un lugar |
| **Hitos**: diseñado → implementado → verificado → integrado | Se distingue lo propuesto de lo hecho y de lo integrado |
| **Archivos con dueño** | Dos agentes no editan lo mismo; los choques se ven antes de repartir |

Dos reglas atraviesan todo:

1. **La fricción es proporcional al riesgo.** Una pregunta o un cambio puntual claro se hacen y se
   verifican directo, sin ficha.
2. **Lo que se afirma sin evidencia cuenta como no hecho.** Y el último hito, *integrado*, lo marca una
   persona, nunca el agente.

## Cómo se ve una ficha

```markdown
# Avisar a la coordinadora cuando una regla de informe queda apagada
Hito: verificado
Pendiente: PENDIENTES.md → «Un borrado rechazado apaga la regla en silencio»

## Objetivo
Que ninguna coordinadora deje de recibir informes sin saberlo.

## Alcance
Incluye: decir en la respuesta que la regla quedó apagada; listar las apagadas.
Queda afuera: dejar de retener informes sin integrar (es a propósito).

## Criterios de aceptación
1. DELETE /rules/<id> con informes pendientes responde desactivada y la lista. Prueba HTTP.
2. ...

## Dominio
Regla, Informe, flujo «Borrar una regla».

## Archivos y dueños
- lienzo/rules.py, lienzo/server.py: frente server.
- skills/lienzo/coordinar.py: frente coordinación.

## Trabajo y decisiones
- Decidido: el arreglo es de visibilidad, no de retención.

## Evidencia
- 33 pruebas focales y 232 de reglas pasan (2026-10-26).

## Próximo paso
Revisión de la persona responsable para marcar integrado.
```

La plantilla completa y las reglas están en [`skills/fdd/SKILL.md`](skills/fdd/SKILL.md).

## Instalación

La skill es una carpeta con un `SKILL.md`. Se instala enlazándola donde tu agente busca skills.

**Claude Code** (lee `~/.claude/skills/<nombre>`):

```bash
git clone https://github.com/arielelevy/fdd
ln -s "$PWD/fdd/skills/fdd" ~/.claude/skills/fdd
```

En Windows, con PowerShell:

```powershell
New-Item -ItemType Junction -Path "$HOME\.claude\skills\fdd" -Target "$PWD\fdd\skills\fdd"
```

**Codex** (lee `~/.agents/skills/<nombre>`): el mismo enlace, en `~/.agents/skills/fdd`.

**Otros agentes:** cualquiera que acepte instrucciones en Markdown puede leer `skills/fdd/SKILL.md`
desde su archivo de reglas (`AGENTS.md`, `.cursorrules`, etc.).

## Uso

- Pedile al agente una feature que tenga varias decisiones o que se vaya a repartir: la skill se carga
  sola por su descripción, o se la nombrás («usá fdd»).
- La primera ficha del proyecto crea `docs/dominio.md` con lo que el agente ya leyó para esa feature;
  no hace falta un relevamiento previo.
- Para retomar, la sesión nueva lee el mapa y la ficha, y sigue desde **Próximo paso**.

## Qué no es

- No es FDD clásico: no exige modelar todo el dominio antes de empezar, ni roles fijos, ni un tope de
  dos semanas por feature.
- No es SDD: no hay fases ni aprobaciones entre documentos. Si una feature crece, se amplía la misma ficha.
- No cierra tareas solo: ni un test que pasa ni un merge marcan *integrado*.

## Comparación

| | FDD clásico (1997) | SDD | **fdd** |
|---|---|---|---|
| Para | equipos humanos grandes | agentes, trabajo grande | agentes, trabajo de todos los días |
| Antes de codificar | modelo de dominio completo | spec, diseño, plan, tareas | ficha breve; mapa incremental |
| Unidad | feature de ≤ 2 semanas | spec | ficha por feature |
| Avance | 6 hitos por feature | fases | 4 hitos, el último humano |
| Cierre | jefe de programadores | aprobación de fases | sólo con evidencia y una persona |

## Créditos

- **Feature-Driven Development:** Jeff De Luca y Peter Coad (1997). Stephen Palmer y John Felsing,
  *A Practical Guide to Feature-Driven Development* (2002).
- **FTD (Fast-Track Development)**, de donde vienen la ficha y la exigencia de evidencia:
  [RSC harness](https://github.com/ericrisco/rsc-harness), de `ericrisco`, licencia MIT.
- Probado en [Lienzo](https://github.com/arielelevy/lienzo), un tablero para coordinar varios agentes
  en paralelo.

## Licencia

[MIT](LICENSE) © 2026 Ariel Levy.
