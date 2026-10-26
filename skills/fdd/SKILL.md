---
name: fdd
description: Feature-Driven Development para agentes de IA. Una ficha por feature con nombre de resultado, dentro de la constitución del proyecto, con mapa del dominio vivo, avance por hitos y archivos con dueño. Usar para construir o repartir una feature entre sesiones, retomar una feature de otra sesión o planear trabajo de varios frentes. Un cambio puntual y claro no la necesita.
---

# FDD · Feature-Driven Development para agentes

Feature-Driven Development (De Luca y Coad, 1997) organizaba equipos grandes alrededor de features
chicas con valor para el usuario. Esta skill toma lo que le sirve a un agente que trabaja en
sesiones cortas y que otra sesión tiene que poder retomar sin el chat. La ficha y la evidencia
vienen de FTD (Fast-Track Development) de [RSC](https://github.com/ericrisco/rsc-harness).

La regla de fondo: **la fricción es proporcional al riesgo.** Una pregunta o un cambio puntual
claro se hacen y se verifican directo. La ficha es para lo que tiene varias decisiones, se reparte
entre sesiones o hay que poder retomar.

## 1. La constitución

Las reglas duraderas del proyecto, que ninguna feature rompe: las instrucciones del usuario, el
`AGENTS.md` (o `CLAUDE.md`, `.cursorrules`) del repo, las decisiones de producto ya tomadas y los
contratos técnicos vigentes (APIs, formatos, invariantes).

- FDD trabaja **dentro** de la constitución. No exige crear otro archivo: si esas reglas ya existen,
  la constitución son ellas. Si no hay ninguna escrita, la primera ficha anota en `docs/dominio.md`,
  sección «Reglas que no se rompen», las que encontró en el código.
- Antes de diseñar, la ficha dice qué reglas de la constitución toca. Si la feature choca con una, se
  resuelve con la persona responsable **antes** de implementar, no después.
- Una decisión nueva que tiene que durar (un contrato, una regla de calidad) se incorpora a la
  constitución al integrar la feature, no queda sólo en la ficha.
## 2. Mapa del dominio

Un archivo por proyecto, `docs/dominio.md`, que se lee antes de abrir una ficha:

```markdown
# Dominio de <proyecto>
Actualizado: <fecha> (<commit>)

## Entidades
- **<Entidad>**: qué es, en una línea. Vive en `<ruta>`. Se relaciona con <otra> por <qué>.

## Flujos principales
- <flujo>: <entidad> → <entidad> → <resultado>. Entra por `<ruta>`.

## Reglas que no se rompen
- <invariante>, y dónde se verifica.
```

- Es un índice, no documentación: una línea por entidad y la ruta donde vive.
- Se corrige cuando una feature cambia una entidad o un flujo, en el mismo cambio.
- Si no existe, la primera ficha del proyecto lo crea con lo que ya se leyó para la feature, no con
  un relevamiento aparte.

## 3. La feature

Nombre de resultado: **acción + resultado + objeto**, del lado del usuario.

- Bien: «avisar cuando una regla de informe queda apagada».
- Mal: «arreglar rules.py», «refactor de reglas».

Si no entra en una frase así, son varias features: una ficha cada una.

## 4. La ficha

Una por feature. Si ya hay una que contiene el encargo, se usa esa.

```markdown
# <acción + resultado + objeto>
Hito: diseñado | implementado | verificado | integrado
Pendiente: <referencia en la lista de trabajo del proyecto>

## Objetivo
Qué cambia para el usuario y por qué.

## Alcance
Qué incluye y qué queda afuera.

## Criterios de aceptación
Comportamientos observables y cómo comprobarlos.

## Dominio y constitución
Entidades y flujos de `docs/dominio.md` que toca, y reglas de la constitución que la afectan.
Si cambia alguno, qué cambia.

## Archivos y dueños
- `<ruta>`: <sesión o frente dueño>. Nadie más lo edita mientras la ficha esté abierta.

## Trabajo y decisiones
Tareas, dependencias y decisiones tomadas con su motivo.

## Evidencia
Lo observado: pruebas que pasaron, que fallaron y que no se corrieron, con la condición en
que se midió, y los commits.

## Próximo paso
Una acción concreta para quien retome.
```

- La ficha se actualiza en hitos útiles, no después de cada herramienta.
- Se amplía según la complejidad (arquitectura, contratos, riesgos) en la misma ficha. No se abre
  otra metodología.
- No se crea otro backlog: la ficha enlaza el pendiente de la lista que el proyecto ya tenga.

## 5. Hitos

| Hito | Se marca cuando |
|---|---|
| diseñado | objetivo, alcance, criterios, dominio, constitución y dueños escritos |
| implementado | el cambio está hecho en el árbol o en una rama |
| verificado | cada criterio tiene evidencia observada, no afirmada |
| integrado | **lo marca una persona**, nunca el agente |

- Un turno terminado, un test aislado o un merge no mueven un hito solos.
- Lo que se afirma sin evidencia cuenta como no hecho.

## 6. Varios frentes

- Cada frente tiene archivos disjuntos, escritos en la ficha antes de empezar. Si dos fichas abiertas
  nombran el mismo archivo, se resuelve antes de repartir.
- Lo que un frente encuentra fuera de sus archivos lo anota en la ficha, no lo edita.
- Un solo integrador por árbol: los frentes no hacen commit.

## Qué no hace

- No exige modelar el dominio completo antes de empezar.
- No impone roles fijos ni un tope de tiempo por feature.
- No cierra tareas automáticamente.
