---
name: fdd
description: Feature-Driven Development para agentes de IA. Una ficha por feature con nombre de resultado, dentro de la constitución y la arquitectura del proyecto (que ningún cambio rompe, ni el más chico), con mapa del dominio vivo, avance por hitos y archivos con dueño. Usar para construir o repartir una feature entre sesiones, retomar una feature de otra sesión o planear trabajo de varios frentes. Un cambio puntual y claro no necesita ficha, pero tampoco se sale de la constitución ni de la arquitectura.
metadata:
  version: "2.2"
---

# FDD · Feature-Driven Development para agentes

Feature-Driven Development (De Luca y Coad, 1997) organizaba equipos grandes alrededor de features
chicas con valor para el usuario. Esta skill toma lo que le sirve a un agente que trabaja en
sesiones cortas y que otra sesión tiene que poder retomar sin el chat. La ficha y la evidencia
vienen de FTD (Fast-Track Development) de [rsc-harness](https://github.com/ericrisco/rsc-harness)
(2026-10), distinta de RSC 2.0/ASDD. Las
lecciones de método y de dónde viene cada una están en la sección 9.

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

- **Reusar antes de agregar: lo mínimo necesario.** Antes de escribir código, bajar una escalera:
  ¿hace falta de verdad?, ¿ya está en el repo? (si existe, se usa), ¿lo resuelve la biblioteca estándar
  o algo nativo de la plataforma?, ¿una dependencia ya instalada?, ¿se resuelve en una línea?; recién
  entonces, lo mínimo que cumple el criterio. Un atajo tomado a conciencia se anota como deuda en la
  lista de trabajo, con su motivo; no se esconde. La lógica riesgosa (dinero, seguridad, parsers,
  datos) lleva igual su prueba, por mínima que sea. La simplicidad se mide por el problema resuelto, el
  uso y el mantenimiento, no por tener menos líneas. (Criterio inspirado en
  [Ponytail](https://github.com/DietrichGebert/ponytail); no depende de esa herramienta ni de sus cifras.
  Los dos límites los sugiere su prueba aislada, 2026-11-05: menos código vino con menos pruebas y menos
  defensas.)
- **Una herramienta de terceros se audita antes de adoptarla**: qué ejecuta, qué lee, qué escribe y si
  usa red; y se prueba aislada, sin tocar la configuración ni los datos de trabajo (prueba de Ponytail,
  2026-11-05).
- FDD trabaja **dentro** de ellas. No exige crear otro archivo: si esas reglas ya están escritas, son
  ellas. Si no hay nada escrito, la primera ficha anota en `docs/dominio.md`, sección «Reglas que no
  se rompen», las que encontró en el código (qué base, qué autenticación, qué capas).
- Antes de diseñar, la ficha dice qué reglas y qué partes de la arquitectura toca.
- **Salirse es una decisión de la persona responsable, antes de implementar.** El agente propone,
  explica por qué lo existente no alcanza y espera; no lo hace y avisa después.
- **Qué se consulta y qué no.** Dentro del alcance autorizado, lo rutinario se resuelve sin pedir
  permiso por paso. Se consulta lo que cambia el resultado, el alcance, los permisos o una decisión
  previa, con motivo, opciones y efecto de cada una; mientras tanto sigue el trabajo que no depende de
  esa respuesta. Una duda resuelta no se vuelve a preguntar sin evidencia nueva. (Idea tomada de RSC 2.0
  vía la revisión de Astra.)
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
- **Si no se sabe si un efecto externo ocurrió** (un pago, un envío, un alta), se consulta al sistema
  dueño de ese efecto antes de repetirlo; reintentar a ciegas puede duplicarlo (propuesta de Factory de
  Astra).

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
- Cubre el sistema entero, no sólo lo que se ve: entidades de datos, motores, servicios y datasets, además
  de las pantallas. Un mapa que sólo indexa la interfaz deja afuera justo lo que las features de backend tocan.
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

## Criterios y demostración
Condiciones → acción → resultado observable; cómo se comprueba y qué evidencia queda.

## Dominio, constitución y arquitectura
Entidades y flujos de `docs/dominio.md` que toca, reglas de la constitución que la afectan y partes de
la arquitectura que reusa (base, autenticación, componentes).
Si cambia alguno, qué cambia.

## Archivos y dueños
- `<ruta>`: <sesión o frente dueño>. Nadie más lo edita mientras la ficha esté abierta.

## Efectos compartidos
Lo que el cambio mueve fuera de sus archivos: versiones de contratos o formatos, hashes sellados, pruebas
medidas que se invalidan (rendimiento, equivalencia, localidad), pines en pruebas de otros, datos o
esquemas compartidos. Cada uno con quién lo actualiza y cuándo, para planificar los re-sellos y las
re-mediciones antes de repartir y no descubrirlos en la suite completa del cierre. Quien sube una
versión con contrato documentado corre su prueba de deriva de contratos en el mismo paso, sin esperar
al cierre. Ese control no reemplaza la verificación global que el proyecto exija al cierre.

## Maqueta
Si la feature es visual: enlace a la maqueta editable y quién la aprobó (ver sección 5).

## Olas
El plan de olas vive en el objetivo (lo propone la herramienta o la coordinadora, ver sección 6). La
ficha enlaza la ola o las olas que implementan esta feature; no copia la tabla. Si no hay herramienta
ni objetivo que registre las olas, la tabla:
| Ola | Termina con (criterio observable) | Pruebas que la cierran | Depende de | Estado |
|---|---|---|---|---|

## Trabajo y decisiones
Tareas, dependencias y decisiones tomadas con su motivo.

## Evidencia
Lo observado: pruebas que pasaron, que fallaron y que no se corrieron, con la condición en
que se midió, y los commits. La ruta de cada evidencia que respalda un hito (logs, mediciones,
arneses) queda escrita acá: lo citado no se borra al limpiar temporales.

## Próximo paso
Una acción concreta para quien retome.
```

- **La ficha no copia el código: apunta a él.** Nada de firmas, clases, pseudocódigo ni diseño
  detallado que el agente puede leer en el repo; eso se desactualiza apenas cambia una línea. La ficha
  guarda lo que el código no dice: el objetivo, lo que queda afuera, las decisiones con su motivo y la
  evidencia. Con eso, el agente decide el cómo con autonomía, dentro de la constitución y la arquitectura.
- La ficha se actualiza en hitos útiles, no después de cada herramienta.
- Se amplía según la complejidad (arquitectura, contratos, riesgos) en la misma ficha. No se abre
  otra metodología. «Archivos y dueños», «Efectos compartidos», «Maqueta» y «Olas» se agregan sólo
  cuando el trabajo los necesita.
- No se crea otro backlog: la ficha enlaza el pendiente de la lista que el proyecto ya tenga.

## 5. Maqueta primero (apps visuales)

Antes del código de una app visual, una **maqueta editable con el recorrido conectado y una base de
diseño central**: colores, tipografía, tarjetas, componentes y estados. Se reusa la maqueta y la base
que la app ya tenga; la identidad existente se conserva. En una app nueva se define una base mínima que
crece con las olas. La herramienta la elige el proyecto; la elección actual de quien mantiene FDD es
**Penpot** (2026-11-04).

- Conectar el recorrido entre pantallas para probar el objetivo de uso, no sólo imágenes aisladas.
- La maqueta cubre los estados del recorrido (vacío, carga, error o conflicto, regreso), el teclado y
  el foco. Que la maqueta coincida con la app en tokens no prueba que sea usable (revisión de Astra).
- Reutilizar tokens y componentes compartidos; el diseño evoluciona con incrementos usables.
  Si los tokens se copian a mano desde el código, registrar su origen y comprobar diferencias
  antes de usar la maqueta.
- Verificar en vivo el archivo y el recorrido del visor antes de afirmar que están guardados.
- La ficha enlaza proyecto, archivo y flujo de la maqueta (en Penpot, por ejemplo) con sus criterios de
  diseño y de uso.
- **Quién aprueba** (decisión del 2026-11-01): la maqueta inicial de la app la aprueba la
  persona responsable antes de construir. En las olas posteriores, la coordinadora decide si hace
  falta una maqueta, la revisa y la aprueba dentro del alcance y el diseño ya autorizados, y
  registra la decisión en la ficha. Eso no autoriza cambiar alcance, identidad visual,
  arquitectura, contratos ni decisiones cerradas; ante un recorrido principal nuevo, consultar.
- Comprobar el ancho de celular cuando corresponda al uso previsto.
- HTML y capturas pueden acompañar la evidencia; no reemplazan la referencia editable
  salvo pedido explícito. Si falta la conexión, informar el impedimento y
  preparar su acceso, sin sustituir la herramienta elegida por cuenta propia.
- El prototipo no acepta entregas ni sustituye la verificación de la app real.

## 6. El plan en olas

Organización (decisión del 2026-11-05): **Proyecto → Objetivo → Feature → Ola → Tarea**. Es cómo se
ordena el trabajo, no una relación uno a uno: una ola puede implementar una o más features, y una
feature puede llevar varias olas. Un
**objetivo** es grande: se descompone en **features** (resultados de valor) que se implementan en
**olas**, en orden, y cada ola termina con algo que se puede probar. Las features viven en el
Markdown del objetivo; no son un nivel aparte en la herramienta. Las olas no van en ese Markdown: las
propone la herramienta o la coordinadora, que las replanifica; el plan de olas es del objetivo y cada
ola referencia las features que implementa. Una
ola es un sprint: uno o varios agentes avanzan en paralelo una o más features que no comparten
archivos. Así se construyó de punta a punta un sistema de cobros en producción: cada ola cerrada y probada antes
de abrir la siguiente.

- **Cada ola tiene un criterio de aceptación observable**, del lado del usuario: «desde el celular,
  buscar un DNI de prueba, elegir cuotas y ver los datos para transferir», no «terminar el servicio de
  cuotas».
- **Cada ola entrega algo usable y vertical**: atraviesa sólo las capas necesarias para su resultado.
  Base, arquitectura y diseño crecen con las olas. Sólo la próxima ola se detalla del todo; las
  siguientes conservan resultado y dependencias. Crecer por olas no autoriza improvisar autenticación,
  durabilidad ni permisos: eso sigue la arquitectura desde la primera ola (propuesta de FDD 2.0 y lectura
  de HolaMundo).
- **La primera ola llega al resultado, no a media capa** (Astra P0.2). Una ola que termina en «datos
  listos» o «encargo recibido» no es todavía una entrega usable: la primera porción debe llegar hasta el
  resultado que una persona puede ver y probar de punta a punta. Una tarea de infraestructura puede ser
  necesaria, pero por sí sola no es una ola de producto.
- **El tamaño de la ola se mide por lo que se puede revisar y revertir.** Cuanto más grande la entrega
  de una vez, más tarde aparecen los desacuerdos y más caro es corregir. Sin topes universales de
  criterios, archivos o líneas: un cambio de permisos o de recuperación necesita más casos que uno
  visual. («Revisar y revertir» es idea de HolaMundo, vía la revisión de Astra; de esa lectura la
  revisión de Astra conserva «historia vertical pequeña, sin máximos universales».)
- **Una sola fuente editable; la revisión que se ejecuta es derivada e inmutable** (Astra P0.1). Cada
  cosa del plan se edita en un solo lugar (las features, en el Markdown del objetivo; las olas, donde se
  replanifican). Cada intento usa una revisión derivada de esa fuente, congelada para ese intento y con
  la huella de lo que se aprobó. El registro de hechos (intentos, recibos, evidencia) no vuelve a ser
  otra fuente editable. Editar una ola futura después de un intento no borra ni renumera la historia:
  sólo invalida la autorización afectada. Dos representaciones no son un problema si una deriva de la
  otra; dos autoridades editables sí.
- **La prueba va antes que el código.** Los casos críticos de la ola se numeran con su demostración y
  se escriben primero. La ola cierra con ellos y las pruebas afectadas por su delta en verde, y al
  menos un criterio comprobado por el camino real.
- **La verificación global, una vez.** La suite completa, y la mutación amplia o el clon limpio si el
  proyecto los exige, se corren al cierre del release o de la integración, con las fuentes
  congeladas. Se repiten sólo ante un fallo, un cambio que invalide la evidencia o una exigencia
  explícita. La mutación del delta, cuando el riesgo de la lógica la justifica; no por rutina.
- **Las dependencias entre olas van en una tabla** (ola, esfuerzo, depende de). Lo que bloquea
  producción pero no el desarrollo (credenciales, definiciones del cliente) es una ola 0 que corre en
  paralelo; mientras tanto se trabaja con valores de ejemplo.
- **Una lista ordenada no es el grafo.** Las dependencias se declaran y se revisan antes de ejecutar, y
  no obligan a correr todo en serie: lo que no depende de nada pendiente puede avanzar (Astra P1).
- **Antes de lanzar un objetivo, se reconcilia su plan con lo ya hecho.** Un plan escrito días antes
  repite trabajo: cada entrada se verifica en el código (no sólo en el texto) y lo resuelto sale con su
  commit. Al reconciliar un plan de 61 entradas, 31 ya estaban resueltas (práctica del 2026-11-05).
- **El objetivo de la coordinadora sigue la ola en curso, no el objetivo entero.** Si la herramienta le
  fija una condición de cierre a la sesión, es la ola en curso con sus features y su criterio, con el
  objetivo como contexto; al aceptarse la ola, pasa a la siguiente. Corta y en una línea: un comando
  largo que la herramienta convierte en adjunto no se ejecuta (decisión del 2026-11-05).
- **Por proyecto, un objetivo en ejecución a la vez.** Los demás esperan en una cola visible, salvo
  decisión explícita de la persona responsable (decisión del 2026-11-05).
- **Cada ola cierra con revisión de código y una nota de estado** (qué quedó, qué falta, qué se
  decidió). Nada se marca hecho hasta que la persona responsable lo indique.
- Dentro de una ola, los frentes que no comparten archivos pueden correr **en paralelo** (sección 8)
  cuando compensa; un frente por funcionalidad de punta a punta.
- Lo que bloquea una ola de producción (dinero real, DNS, credenciales) va al final y espera confirmación.

### Estado para retomar

En un proyecto con varias olas o features, el estado es **lo primero que se lee al retomar**: cada ola y
su estado (hecho en local, desplegado, falta…), lo que se agregó fuera del plan, la verificación al
cierre y las decisiones de cada ronda con su fecha. Una sola fuente, que mantiene la coordinadora:

- Si el proyecto usa una herramienta que registra objetivos, olas y su avance, ese registro es el
  estado; la ficha enlaza el objetivo y el plan fuente.
- Si no, `docs/estado.md`, o un enlace explícito en la ficha al documento que cumple ese papel.
- No se mantienen dos estados del mismo plan.
- **Una ola lanzada a mano no mueve el registro.** Si la herramienta sólo sigue lo que despacha ella,
  la coordinadora registra cada paso por separado, con su motivo: al asignar (en curso), al recibir la
  entrega (entregada), al integrarla (integrada) y, cuando la persona la acepta, aceptada. Integrar no es
  aceptar. Lo preferible es lanzar la ola por la herramienta para que el seguimiento sea de punta a punta.
- **Aceptar una ola habilita la siguiente que depende de ella**, no la cadena. Una ola sin
  dependencias pendientes puede asignarse a un frente libre; verificar no es aceptar. Cambiar esa regla
  —encadenar olas por verificación en vez de por aceptación— no es un ajuste interno de la herramienta:
  es un cambio del contrato entre olas, con el tipo de dependencia y quién la habilita escritos y
  versionados, decidido por la persona responsable (Astra P0.3).
- **La metodología queda visible para quien usa la app**: una página o ayuda resume el flujo y enlaza
  esta skill y el plan fuente, sin agregar reglas propias.

## 7. Hitos

| Hito | Se marca cuando |
|---|---|
| diseñado | objetivo, alcance, criterios con su demostración, dominio, constitución y arquitectura; dueños, efectos compartidos y olas cuando correspondan; si es visual, maqueta aprobada según la sección 5 |
| implementado | el cambio está hecho en el árbol o en una rama |
| verificado | cada criterio tiene evidencia observada, no afirmada |
| integrado | **lo marca una persona**, nunca el agente |

- Un turno terminado, un test aislado o un merge no mueven un hito solos.
- Lo que se afirma sin evidencia cuenta como no hecho.
- **Aprobar un plan no acepta sus entregas**; entregado no es correcto y verde no es aceptado. Aceptar y
  cerrar son decisiones de la persona (Astra P0.3).
- **Verificado es por el camino real.** La ficha ordena el trabajo, pero no encuentra defectos: los más
  caros aparecen corriendo el cambio como corre de verdad (el proceso arrancado como en producción, el
  binario real, la medición repetida), no con dobles ni con el arnés de pruebas solo. Un test que recorre
  una carpeta vacía pasa sin verificar nada; un runner que agrega rutas que producción no tiene oculta
  justo esos errores. Al menos un criterio de cada feature se comprueba por el camino real.
- **En documentos e investigaciones, el criterio es de contenido y fuentes**: lo inferido va separado de
  lo observado, y lo incierto se declara incierto, no aprobado (propuesta de Factory de Astra, F14).
- **Una mejora del método se afirma comparando** contra una base de complejidad similar, no con una
  corrida suelta (propuesta de FDD 2.0; prueba de Ponytail).
- **Demostrar el requisito, no acumular tests verdes** (idea tomada de RSC 2.0 vía la revisión de
  Astra). Un agente puede superar sus
  pruebas y construir igual un producto distinto del pedido. Por eso cada criterio dice qué podrá hacer la
  persona, bajo qué condiciones y permisos, qué resultado observable obtiene y cómo se comprueba; una
  captura o una respuesta de API apoyan un criterio, no demuestran por sí solas toda la experiencia.
- **Requisito no es supuesto.** Ante un conflicto real con lo pedido, explicar la evidencia y las
  alternativas y resolverlo con la persona; no omitir en silencio una función ni agregar un recorrido por
  preferencia del agente (por ejemplo, cambiar el método de acceso pedido por otro). Anotar un supuesto
  no da autoridad para cambiar alcance, permisos ni una decisión cerrada. Quien implementa tampoco
  cambia en silencio la prueba o el criterio que lo acepta: si hace falta, lo propone, y esa corrida no
  se valida a sí misma (Astra, propuesta de Factory).
- **Revisar durante el trabajo, no sólo al final** (idea tomada de RSC 2.0 vía la revisión de Astra).
  Construir una porción acotada,
  comprobarla y contrastar los hallazgos con el código: confirmar, corregir o descartar con su motivo. Un
  informe de otro agente no convierte una hipótesis en defecto; un impedimento del entorno no prueba un
  defecto del producto ni que funcione; una explicación no reemplaza una comprobación ejecutada.

## 8. Varios frentes

- **El encargo de un frente es su ficha.** No se escribe un encargo y además una ficha que dice lo mismo:
  el encargo lleva el `Hito:`, los archivos con dueño, el dominio y los efectos compartidos, y el frente lo
  actualiza al pasar de hito.
- Cada frente tiene archivos disjuntos, escritos en la ficha antes de empezar. Si dos fichas abiertas
  nombran el mismo archivo, se resuelve antes de repartir.
- **Los efectos compartidos se reparten antes**: quién sube cada versión, quién re-sella cada hash y
  cuándo se re-mide cada prueba medida. Las mediciones finales se coordinan al cierre con las fuentes
  congeladas; los controles de contratos corren con cada cambio de versión.
- **Los archivos compartidos son de la coordinadora** (configuración, modelos y migraciones,
  dependencias, fixtures comunes, `AGENTS.md`, el estado del plan, CI). Un frente que necesita un cambio ahí lo
  pide en su informe con el diff exacto, y sigue con lo que no depende de eso.
- **Contratos entre frentes**: la firma de lo que uno expone y el otro llama se fija antes de repartir,
  y se respeta aunque el otro todavía no exista. Nadie reimplementa lo que es de otro frente.
- **No quedarse quieto esperando**: si algo depende de otro frente o de un servicio externo, se finge
  con una implementación en memoria de la misma interfaz y se sigue.
- **Recursos aislados**: cada frente con su base de pruebas, su puerto y sus temporales, nombrados con
  su letra. Lo que comparte el equipo (contenedores, el server de la persona) no se frena ni se reinicia.
- **Las pruebas nunca tocan el estado real de la máquina.** Corren con carpetas temporales, y ningún hilo
  o proceso que lance una prueba sigue vivo cuando termina. Un hilo de prueba que sobrevivió reescribió dos
  veces la identidad de la máquina y cortó el enlace con la otra PC (incidente del 2026-11-05).
- **Una falla no se atribuye sin línea de base.** Antes de culpar al delta de un frente, la prueba se corre
  en una copia limpia del commit anterior. Una falla que ya estaba se anota aparte con su dueño y no
  frena la entrega.
- Lo que un frente encuentra fuera de sus archivos lo anota en una nota propia, no lo edita.
- **Antes de limpiar temporales de coordinación**, buscar qué rutas citan las fichas, los contratos y los
  informes, y no borrar esas sin confirmación: la evidencia citada respalda hitos y pruebas medidas.
- Un solo integrador por árbol: los frentes no hacen `git add`, `commit`, `checkout` ni `pull`.
- **Integrar con otro frente a medias en el mismo árbol**: commitear sólo los archivos del frente
  entregado y compilar desde una copia limpia de ese commit (worktree); no reiniciar el servicio si el
  backend de otro frente está a medio cambiar. Una regresión causada por el delta ajeno se atribuye a su
  dueño y se le avisa; no bloquea la entrega que no la causa.
- **Pedidos nuevos de la persona durante una ola**: si tocan archivos de un frente en curso, se suman a
  su ficha como ampliación fechada con la cita textual; si son independientes, van a un frente libre con
  ficha propia. Si la persona insiste, se prioriza y se entrega aparte en cuanto esté.
- **Una sola coordinadora por proyecto**, la que la herramienta marca como tal. Otra sesión que empieza
  a repartir trabajo o lanzar frentes se detiene y le traspasa sus frentes, fichas y cambios sin commitear.
- **Las preguntas a la persona pasan por un solo lugar** (el human-in-the-loop formal): agrupadas por
  proyecto y objetivo, con contexto suficiente para responder sin abrir otra pantalla, y cada respuesta
  vuelve a quien preguntó. La coordinadora no hace de cartero ni responde por la persona.
- **«Achicar» no es rediseñar**: un pedido de ajuste visual conserva dibujo y comportamiento; se parte de
  la versión anterior del componente (el commit previo) y sólo se cambian tamaños.
- **Informe con forma fija**: qué quedó, archivo por archivo, con la prueba que lo cubre; qué se midió;
  qué quedó afuera y por qué; pedidos a la coordinadora; lo visto fuera de su carpeta. La última línea,
  siempre: `ESTADO: terminado | trabado: motivo | sigo: qué falta`.
- **Revisión cruzada de lo crítico**: los caminos del dinero, la seguridad y los datos personales los
  revisa otro frente, no el que los escribió. Todo hallazgo alto se corrige con su prueba. «Lo revisó
  otra IA» no reemplaza la evidencia del criterio. Cada revisión adicional cubre un riesgo distinto;
  repetir la misma revisión no suma evidencia. Se revisa más donde el riesgo es mayor, y el resultado se
  mide también en incidentes y retrabajo, no sólo en hallazgos (artículo de review de Anthropic, vía la
  revisión de Astra).
- **Ninguna sesión del proyecto queda sin vínculo**: informa a la coordinadora o se declara personal. La
  coordinadora asigna enseguida trabajo sin dependencias pendientes a un frente libre, o lo cierra si
  está integrado y no tiene tarea siguiente, sin consultar (práctica de coordinación, 2026-11-05).
- El nombre del encargo dice su ola y se actualiza al reasignarlo.
- **Presupuesto o cupo de los agentes: asignar, atribuir y frenar son tres cosas** (Astra P0.4). Sin
  consumo atribuible se dice «sin dato», que no cuenta como margen, y no se infiere restando lecturas
  globales. Pausar nuevas admisiones no corta lo que ya corre ni garantiza un techo duro: se declara qué
  límite sí se impone. El límite frena al lanzar trabajo, no al planificarlo.

## 9. De dónde viene: fuentes y lecturas

La base es FDD clásico y FTD. Las lecciones de método de la versión 2.0 —no reglas de una app ni de
una herramienta— salen sobre todo de la revisión de Astra, que analizó también otras lecturas. Cada
lección dice su fuente donde se aplica.

Fuentes adoptadas:

- **FDD 1997** (De Luca y Coad): feature con valor para el usuario, modelo del dominio compartido,
  propiedad de clases/archivos y avance contado por hitos. Resumen en `docs/fdd-1997.md`.
- **FTD** ([rsc-harness](https://github.com/ericrisco/rsc-harness), 2026-10, MIT; distinta de RSC 2.0/ASDD):
  la ficha, la constitución y «lo que se afirma sin evidencia cuenta como no hecho».
- **Revisión de Astra** (revisión unificada de Factory y FDD 2.0, 2026-11-04, y su propuesta de
  Factory; repositorio privado de Lienzo): P0.1 una sola fuente editable y una revisión ejecutable
  derivada (§6); P0.2 la primera ola llega al resultado (§6); P0.3 la aceptación entre olas no se
  reemplaza por verificación, y aprobar, entregar o dar verde no es aceptar (§6, §7); P1 las
  dependencias son un grafo declarado, no una lista (§6); P0.4 asignar, atribuir y frenar el
  presupuesto son tres cosas (§8); el oráculo no lo cambia quien implementa (§7); ante un efecto
  externo incierto se consulta al sistema dueño (§1); F14 el criterio de documentos e investigaciones
  (§7); la maqueta cubre estados, teclado y foco (§5).

Lecturas consideradas, no adoptadas como metodología (las analizó la revisión de Astra; de ellas se
tomaron ideas, adaptadas a FDD y atribuidas «vía la revisión de Astra»):

- **RSC 2.0 / ASDD** (video de Eric Risco, 2026-11-03): demostrar el requisito, requisito no es
  supuesto y revisar durante el trabajo (§7); qué se consulta y qué no (§1). No se toman su cadena de
  documentos, la clasificación de riesgo como autoridad, las ratificaciones provisionales ni el
  auto-merge.
- **HolaMundo / Hola AI** (video, 2026-10-23): el tamaño de la ola se mide por lo que se puede revisar y
  revertir (§6), que refuerza el detalle sólo de la próxima ola. No se toman topes universales de
  historias o criterios ni la pausa después de cada test, que contradice la autonomía autorizada.
- **Ponytail** ([DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail), MIT): inspira la
  escalera de «lo mínimo necesario» (§1). No se instaló; una prueba aislada de una corrida por brazo
  (2026-11-05) no mostró ahorro (−33 % código, +6 % costo). De esa prueba salen la auditoría previa de
  herramientas, que la lógica riesgosa lleve su prueba (§1) y comparar contra una base antes de afirmar
  una mejora (§7). Sus cifras propias no están verificadas.
- **Artículo de review de Anthropic**: cada revisión cubre un riesgo distinto, revisar por riesgo y medir
  también incidentes y retrabajo; otra IA no reemplaza la evidencia (§8).

Ninguna herramienta de terceros se instaló para esto (Ponytail sólo se probó aislado, como se dice
arriba), y ninguna cifra ajena se da por medida en otro proyecto.

## Qué no hace

- No exige modelar el dominio completo antes de empezar.
- No impone roles fijos ni un tope de tiempo por feature.
- No pide especificar el código: las decisiones, los contratos y los criterios sí; las firmas,
  clases y pseudocódigo, no.
- No cierra tareas automáticamente.
