---
name: fdd
description: Feature-Driven Development para agentes de IA. Una ficha por feature con nombre de resultado, dentro de la constitución y la arquitectura del proyecto (que ningún cambio rompe, ni el más chico), con mapa del dominio vivo, avance por hitos y archivos con dueño. Usar para construir o repartir una feature entre sesiones, retomar una feature de otra sesión o planear trabajo de varios frentes. Un cambio puntual y claro no necesita ficha, pero tampoco se sale de la constitución ni de la arquitectura.
---

# FDD · Feature-Driven Development para agentes

Feature-Driven Development (De Luca y Coad, 1997) organizaba equipos grandes alrededor de features
chicas con valor para el usuario. Esta skill toma lo que le sirve a un agente que trabaja en
sesiones cortas y que otra sesión tiene que poder retomar sin el chat. La ficha y la evidencia
vienen de FTD (Fast-Track Development) de [RSC](https://github.com/ericrisco/rsc-harness).

Dos reglas de fondo:

1. **Nada se sale de la constitución ni de la arquitectura**, por chico que sea el cambio. Un cambio
   puntual se salta la ficha, nunca estas reglas.
2. **La fricción es proporcional al riesgo.** Una pregunta o un cambio puntual claro se hacen y se
   verifican directo. La ficha es para lo que tiene varias decisiones, se reparte entre sesiones o hay
   que poder retomar.

## 1. Constitución y arquitectura

**La constitución** son las reglas duraderas del proyecto: las instrucciones del usuario, el
`AGENTS.md` (o `CLAUDE.md`, `.cursorrules`) del repo, las decisiones de producto ya tomadas y los
contratos técnicos vigentes (APIs, formatos, invariantes).

**La arquitectura** es cómo está hecho: las capas y módulos, la base de datos, la autenticación, el
manejo de errores, el logging, la configuración, las librerías elegidas y los componentes que ya
existen para cada cosa.

Valen para **todo** cambio, con ficha o sin ella. Un cambio de tres líneas también se sale si:

- mete una **base de datos** o un almacenamiento que no es el de la app;
- autentica o autoriza con **otro método** que el de la app;
- trae una **librería nueva** para algo que una de las que ya están resuelve;
- **no reusa** lo que la app ya tiene (un cliente HTTP, un helper, un componente, un patrón de
  acceso a datos) y escribe otro paralelo;
- saltea una capa (la vista que consulta la base directo, el handler que arma SQL).

Cómo se aplica:

- **Reusar antes de agregar.** Antes de escribir algo, buscar en el repo si ya existe; si existe, se usa.
- FDD trabaja **dentro** de ellas. No exige crear otro archivo: si esas reglas ya están escritas, son
  ellas. Si no hay nada escrito, la primera ficha anota en `docs/dominio.md`, sección «Reglas que no
  se rompen», las que encontró en el código (qué base, qué autenticación, qué capas).
- Antes de diseñar, la ficha dice qué reglas y qué partes de la arquitectura toca.
- **Salirse es una decisión de la persona responsable, antes de implementar.** El agente propone,
  explica por qué lo existente no alcanza y espera; no lo hace y avisa después.
- Una decisión nueva que tiene que durar (un contrato, una regla de calidad, un cambio de
  arquitectura aprobado) se incorpora a la constitución o a la arquitectura al integrar, no queda sólo
  en la ficha.

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

## Dominio, constitución y arquitectura
Entidades y flujos de `docs/dominio.md` que toca, reglas de la constitución que la afectan y partes de
la arquitectura que reusa (base, autenticación, componentes).
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

- **La ficha no copia el código: apunta a él.** Nada de firmas, clases, pseudocódigo ni diseño
  detallado que el agente puede leer en el repo; eso se desactualiza apenas cambia una línea. La ficha
  guarda lo que el código no dice: el objetivo, lo que queda afuera, las decisiones con su motivo y la
  evidencia. Con eso, el agente decide el cómo con autonomía, dentro de la constitución y la arquitectura.
- La ficha se actualiza en hitos útiles, no después de cada herramienta.
- Se amplía según la complejidad (arquitectura, contratos, riesgos) en la misma ficha. No se abre
  otra metodología.
- No se crea otro backlog: la ficha enlaza el pendiente de la lista que el proyecto ya tenga.

## 5. Hitos

| Hito | Se marca cuando |
|---|---|
| diseñado | objetivo, alcance, criterios, dominio, constitución, arquitectura y dueños escritos |
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
