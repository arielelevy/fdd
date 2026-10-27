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

### Decisiones de diseño cerradas

Las decisiones de diseño tomadas se escriben en una tabla en el `AGENTS.md` (o el índice de docs) y
**no se reabren**: ni el agente ni un frente las rediscute por su cuenta.

```markdown
| Tema | Decisión | Por qué |
|---|---|---|
| Base | PostgreSQL en contenedor, local y producción | mismo motor en los dos ambientes |
| Autenticación | JWT + refresh, roles | lo que ya usa el panel |
```

- Los **invariantes con una sola puerta** se nombran: «`pagos.aprobar` es la única puerta a un pago
  aprobado». Nada llega a ese estado por otro camino, y una excepción se agrega sólo con decisión
  escrita.
- **El doc manda antes que el código.** Si el código va a contradecir una decisión o un doc, primero se
  corrige el doc (y se avisa); nunca queda el código diciendo una cosa y el doc otra.
- **Verificar en vivo antes de afirmar** que algo existe (un recurso en la nube, una cuenta, un
  endpoint). Con la credencial o la cuenta equivocada, muchas APIs contestan 200 y vacío.
- **Lo irreversible se propone y espera**: dinero real, credenciales productivas, DNS, borrar datos.

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

## Maqueta
Si la feature es visual: enlace a la maqueta aprobada (ver sección 5).

## Olas
| Ola | Termina con (criterio observable) | Pruebas que la cierran | Depende de | Estado |
|---|---|---|---|---|

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

## 5. Maqueta primero (apps visuales)

Si la aplicación tiene pantallas, **antes del código va una maqueta**, independiente del código de la
app: HTML estático con su CSS en `maqueta/`, una imagen o un diseño. Los agentes trabajan mejor cuando
dibujan primero: la maqueta fija qué se ve, en qué orden y con qué textos, y la persona responsable la
aprueba sin leer código.

- Es una **referencia de diseño, no el frontend**: no se importa ni se reusa como código.
- Usa la identidad visual que ya existe (colores, tipografías, componentes del sitio o del sistema de
  diseño), no una inventada.
- Se prueba como se va a usar: en el ancho de un celular si la usan desde el celular.
- La ficha la enlaza, y los criterios de aceptación visuales se comprueban contra ella.

## 6. El plan en olas

Una feature grande se construye en **olas** (fases cortas), en orden, y cada una termina con algo que
se puede probar. Así se construyó de punta a punta un sistema de cobros en producción: cada ola cerrada y probada antes
de abrir la siguiente.

- **Cada ola tiene un criterio de aceptación observable**, del lado del usuario: «desde el celular,
  buscar un DNI de prueba, elegir cuotas y ver los datos para transferir», no «terminar el servicio de
  cuotas».
- **La prueba va antes que el código.** Los casos críticos de la ola se numeran y se escriben primero;
  la ola no se cierra sin ellos en verde, con la suite completa, no sólo los suyos.
- **Las dependencias entre olas van en una tabla** (ola, esfuerzo, depende de). Lo que bloquea
  producción pero no el desarrollo (credenciales, definiciones del cliente) es una ola 0 que corre en
  paralelo; mientras tanto se trabaja con valores de ejemplo.
- **Cada ola cierra con revisión de código y una nota de estado** (qué quedó, qué falta, qué se
  decidió). Nada se marca hecho hasta que la persona responsable lo indique.
- Dentro de una ola, los frentes que no comparten archivos corren **en paralelo** (sección 8).
- Lo que bloquea una ola de producción (dinero real, DNS, credenciales) va al final y espera confirmación.

### Estado para retomar

En un proyecto con varias olas o features, `docs/estado.md` es **lo primero que se lee al retomar**:
una tabla con cada ola y su estado (hecho en local, desplegado, falta…), lo que se agregó fuera del
plan, la suite al cierre y las decisiones de cada ronda con su fecha. Lo mantiene sólo la coordinadora.

## 7. Hitos

| Hito | Se marca cuando |
|---|---|
| diseñado | objetivo, alcance, criterios, dominio, constitución, arquitectura, dueños, olas y (si es visual) maqueta aprobada |
| implementado | el cambio está hecho en el árbol o en una rama |
| verificado | cada criterio tiene evidencia observada, no afirmada |
| integrado | **lo marca una persona**, nunca el agente |

- Un turno terminado, un test aislado o un merge no mueven un hito solos.
- Lo que se afirma sin evidencia cuenta como no hecho.

## 8. Varios frentes

- Cada frente tiene archivos disjuntos, escritos en la ficha antes de empezar. Si dos fichas abiertas
  nombran el mismo archivo, se resuelve antes de repartir.
- **Los archivos compartidos son de la coordinadora** (configuración, modelos y migraciones,
  dependencias, fixtures comunes, `AGENTS.md`, `estado.md`, CI). Un frente que necesita un cambio ahí lo
  pide en su informe con el diff exacto, y sigue con lo que no depende de eso.
- **Contratos entre frentes**: la firma de lo que uno expone y el otro llama se fija antes de repartir,
  y se respeta aunque el otro todavía no exista. Nadie reimplementa lo que es de otro frente.
- **No quedarse quieto esperando**: si algo depende de otro frente o de un servicio externo, se finge
  con una implementación en memoria de la misma interfaz y se sigue.
- **Recursos aislados**: cada frente con su base de pruebas, su puerto y sus temporales, nombrados con
  su letra. Lo que comparte el equipo (contenedores, el server de la persona) no se frena ni se reinicia.
- Lo que un frente encuentra fuera de sus archivos lo anota en una nota propia, no lo edita.
- Un solo integrador por árbol: los frentes no hacen `git add`, `commit`, `checkout` ni `pull`.
- **Informe con forma fija**: qué quedó, archivo por archivo, con la prueba que lo cubre; qué se midió;
  qué quedó afuera y por qué; pedidos a la coordinadora; lo visto fuera de su carpeta. La última línea,
  siempre: `ESTADO: terminado | trabado: motivo | sigo: qué falta`.
- **Revisión cruzada de lo crítico**: los caminos del dinero, la seguridad y los datos personales los
  revisa otro frente, no el que los escribió. Todo hallazgo alto se corrige con su prueba.

## Qué no hace

- No exige modelar el dominio completo antes de empezar.
- No impone roles fijos ni un tope de tiempo por feature.
- No pide especificar el código: las decisiones, los contratos y los criterios sí; las firmas,
  clases y pseudocódigo, no.
- No cierra tareas automáticamente.
