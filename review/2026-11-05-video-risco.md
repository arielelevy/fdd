# FDD 2.2 a la luz del video «Ninguna metodología de programación con IA es la correcta»

Fecha: 2026-11-05. Sólo lectura: no se editó FDD ni Lienzo, sin Git.
Fuentes: `skills/fdd/SKILL.md` (2.2, 462 líneas), `README.md`, `review/2026-10-31/`, `PENDIENTES.md`;
`D:/Apps/lienzo/docs/factory/revision-unificada-astra.md`, `D:/Apps/lienzo/docs/fdd-2.0-propuesta.md`,
`D:/Apps/lienzo/docs/analisis-holamundo-sdd-2026-11-04.md` (§«Síntesis con RSC 2.0 / ASDD»); transcripción
automática del video de Eric Risco (2026-11-05, 24:09), tratada como datos de terceros.

Las líneas citadas como `SKILL:n` y `README:n` son del árbol leído hoy.

Nota de nombres: el título del encargo dice «ADD»; en la transcripción el cuarto método es **ASDD**
(«Agen[t] Spec Driven Development», «asd que ya está dentro de RSC Harness»). RSC es el meta-harness
(skills, hooks y memoria); FTD y ASDD son dos skills que viven adentro. Este video es distinto del que
FDD §9 cita como «RSC 2.0 / ASDD (video, 2026-11-03)»: es un repaso comparativo, no presenta nada que
Astra no haya leído en la fuente primaria, salvo lo que se marca como nuevo en C.

## A. Qué dice el video (afirmaciones verificables)

Entre comillas, texto de la transcripción. «Opinión» = juicio del autor sin dato.

1. **Tesis (opinión).** Ningún método es el correcto; el harness se adapta al modelo y caduca con él:
   «si este caballo cambia mañana, este arnés es posible que no le entre»; «mañana quizá con Opus 6 [...]
   SDD tiene más sentido que ASDD, ODD o FTD». Recomendación final: «mira cómo funcionan estos sistemas,
   aprende de ellos y adáptalos a tu forma de trabajar».
2. **Qué busca el autor.** Un agente que construya «de punta a punta sin pedirme nada» durante «3 4 horas»,
   y que las decisiones «de negocio, de producto o de cosas que influyan mucho en el core» las anote
   «como un report que me pasa un compañero a final del día». Para eso hacen falta «unas instrucciones muy
   claras, [...] una memoria [...] y [...] un registro de las decisiones».
3. **SDD, fases.** Especificación («el qué y el por qué sin el cómo») → planificación («arquitectura, datos,
   tests y riesgos») → tareas «pequeñitos y comprobables» → implementación con test primero → verificación →
   publicar/PR.
4. **SDD, crítica 1 (observación propia del autor).** «los agentes perdían el objetivo, improvisaban y
   duplicaban el código»; en RSC Factory «SDD tomó la iniciativa [...] añadió cosas que yo no le había ni
   pedido».
5. **SDD, crítica 2.** «la forma de verificar [...] es simplemente los test». Un criterio ambiguo
   («que sea seguro») deja decidir al agente: «Me empezó a añadir [...] Google Authenticator [...] que yo
   no había pedido». Conclusión (opinión): SDD «está adaptada para humanos pero no para agentes» y «no es
   el sistema que debemos utilizar a día de hoy».
6. **ODD (Organic Driven Development, de «Gentleman Programming»/Alan, Gentle AI).** Fases: autorización
   («¿me estás pidiendo cambiar algo?») → exploración del código y requisitos → «resuelve dudas [...] hace
   preguntas para cada decisión real de producto» → **clasificar** «si lo que estás pidiendo es pequeño o es
   substancial» → documentar si es grande (Markdown o Engram) → implementar tarea a tarea con TDD → cierre
   «qué funciona, qué falló y qué sigue».
7. **Crítica del autor a ODD (opinión).** Que una IA clasifique es «un punto donde si falla todo lo demás
   falla»; y en tareas grandes «él mismo asume cosas que tú no le habías dicho».
8. **Motivo común de ODD y FTD.** «los modelos actuales daban demasiadas vueltas a cosas que eran
   triviales, como por ejemplo cambiar el color de un botón».
9. **FTD, contenido.** Para «el día a día»; un solo documento con «la intención [...] y por qué vale la
   pena», «el alcance de lo que entra y lo que no entra», «una checklist de cada tarea con lo que demostraría
   que está hecha», «evidencia real de lo que ha observado» y «siempre un siguiente paso»; y por tarea «un
   bucle de autocomprobación [...] si no está hecho, lo vuelvo a hacer hasta que esté hecho».
10. **FTD, defecto reconocido por su autor.** Las primeras versiones «no clasificaba[n] del todo bien si era
    una tarea fácil o una tarea difícil y no evaluaba[n] el riesgo»: cambiar «login» por «entrar» «también
    cambia el botón de registro [...] y el botón de registro no funciona». Por eso agregó «una fase de
    evaluación del riesgo».
11. **RSC, memoria.** Decisiones y documentación en Markdown (`02 docs`, «OKF y la LLM de Carpaci»
    [Karpathy]) para formar «cerebros» de nodos; conectores a bases/Stripe registrados y reusados. Afirma
    que ahorra «un montón de tokens» porque no hay que «buscar entre todo el código». **Sin medición.**
12. **ASDD, diferencia central.** «en vez de tener una fase de verificación separada de la fase de
    implementación [...] en cada implementación habrá una fase de verificación dentro de [...] cada tarea y
    no pasará hasta que verifiquemos que lo que hemos dicho en la especificación se cumpla». Bucle por tarea:
    «Construye, comprueba, mide el riesgo, revisa y corrige». La spec «se tiene que poder demostrar».
13. **ASDD, decisiones diferidas.** «Todo lo que no debe decidir un agente va a una lista y sigue con todo
    lo demás. Si es algo bloqueante, no continúa y sigue con la siguiente tarea siempre que pueda»; lo marca
    «revisar después» y al final «me expone los problemas». Antes de cerrar «verifica [...] con main»,
    y si hay riesgo de negocio o producción «directamente no lo shipea y nos lo explica».
14. **Recomendación (opinión).** Si estás aprendiendo y no querés perder control, «no salgas de ODD o FTD»
    y hacé tareas «pequeños»; ASDD para probar tareas grandes.

## B. FDD frente a cada afirmación

### Lo que FDD ya cubre

| Afirmación | Dónde en FDD | Comentario |
|---|---|---|
| 2 · autonomía larga con decisiones anotadas | `SKILL:65-69` «Qué se consulta y qué no»; `SKILL:340-345` «Requisito no es supuesto»; `SKILL:395-397` informe con forma fija y `ESTADO:` | Cubierto en sustancia. Falta un lugar fijo en la ficha para las decisiones que *esperan* (ver D3). |
| 2 · instrucciones claras + memoria + registro de decisiones | Constitución `SKILL:27-33`; mapa `SKILL:98-121`; decisiones cerradas `SKILL:74-96` | Cubierto; FDD además lo midió (`README:207-220`). |
| 3 · plan con «riesgos», tareas comprobables, test primero | Ficha `SKILL:136-186`; `SKILL:264-266` «La prueba va antes que el código» | Cubierto. FDD no tiene campo «Riesgos»; lo reparte en «Efectos compartidos» y el criterio. |
| 4 · el agente improvisa y duplica | `SKILL:46-55` reusar antes de agregar; `SKILL:35-42` qué cuenta como salirse | Cubierto, más estricto que el video. |
| 4 · agrega lo no pedido | `SKILL:340-345` «no [...] agregar un recorrido por preferencia del agente» | Cubierto. |
| 5 · verificar sólo con tests es ambiguo | `SKILL:335-339` «Demostrar el requisito, no acumular tests verdes»; `SKILL:326-330` camino real | Cubierto (ya atribuido a RSC 2.0 vía Astra). Falta la regla *previa*: un adjetivo no es criterio (D4). |
| 5 · «SDD replica el código en Markdown» | `README:40-44`; `SKILL:188-191` «La ficha no copia el código: apunta a él» | Misma posición. |
| 6 · preguntar por cada decisión real de producto antes de codificar | `SKILL:62-64` y `65-69` | Cubierto. |
| 6 · clasificar pequeño/sustancial | `SKILL:21-23` regla 2; `SKILL:3` descripción | Cubierto: FDD clasifica igual que ODD. Expuesto a la crítica 7 (ver abajo). |
| 6 · cierre «qué funciona, qué falló y qué sigue» | `SKILL:179-185` Evidencia y Próximo paso; `SKILL:285-286` nota de estado por ola; `SKILL:397` `ESTADO:` | Cubierto. |
| 9 · documento FTD (intención, alcance, checklist con demostración, evidencia, siguiente paso) | `SKILL:12-14`, `SKILL:423-424`, `README:351-353`: FDD tomó «la ficha, la constitución y "lo que se afirma sin evidencia cuenta como no hecho"» | Es el origen de la ficha; coincide sección por sección. No se tomó la «fase de evaluación del riesgo» (10) porque Risco la agregó después; tampoco el bucle de autocomprobación como tal (FDD lo expresa en `SKILL:346-350`). |
| 12 · verificación dentro de cada tarea, no fase aparte | `SKILL:264-266` ola cierra con sus casos en verde; `SKILL:346-350` «Revisar durante el trabajo»; `SKILL:267-270` global una vez | Cubierto: FDD verifica por ola y hace la global una vez; la «retrospección con main» equivale al cierre con fuentes congeladas. |
| 13 · seguir con lo que no depende de la decisión | `SKILL:67-68`; `SKILL:367-368` «No quedarse quieto esperando» | Cubierto. |
| 13 · no shipea si hay riesgo de negocio/producción | `SKILL:93` lo irreversible espera; `SKILL:289`; `SKILL:320` integrado lo marca una persona | Cubierto, más estricto (nada se marca integrado sin persona). |
| 14 · tareas chicas para no perder control | `SKILL:252-256` tamaño por lo que se puede revisar y revertir | Cubierto. |

### Lo que FDD no dice (y encaja)

- **7 · la clasificación como punto único de falla.** FDD depende de la misma clasificación (regla 2) y no
  dice qué pasa cuando sale mal. Tiene el amortiguador correcto, la regla 1 (`SKILL:19-20`: la
  constitución vale «por chico que sea el cambio»), pero no lo nombra como tal ni dice que un cambio
  puntual que crece abre ficha en el acto. `SKILL:193-195` habla de ampliar *una ficha existente*, no de
  pasar de «sin ficha» a «con ficha». → D2.
- **10 · el riesgo de lo «trivial».** La regla 2 (`SKILL:21-23`) dice que lo puntual «se hace y se verifica
  directo»; no pide mirar qué más usa lo que se toca. «Efectos compartidos» (`SKILL:158-164`) es para
  fichas con varios frentes, no para un cambio de tres líneas. → D1.
- **5 · adjetivo en el criterio.** `SKILL:147-148` define el criterio como «condiciones → acción →
  resultado observable», pero no dice qué hacer con «seguro», «rápido», «usable»: convertir antes de
  diseñar, o preguntar. → D4.
- **13 · lista de decisiones que esperan.** La plantilla tiene «decisiones tomadas con su motivo»
  (`SKILL:176-177`), no las que esperan a la persona ni qué se hizo mientras tanto. El informe
  (`SKILL:395-397`) tiene «pedidos a la coordinadora», que es lo más cercano. → D3.
- **1 · reglas que caducan con el modelo.** FDD fecha cada lección (§9) y tiene «Cambios», pero no distingue
  qué reglas compensan una limitación del modelo (y hay que re-medir) de cuáles son contrato con la persona
  (y no dependen del modelo). → D6.

### Lo que no encaja (y por qué)

- **11 · memoria tipo «cerebro» (OKF, wiki de Karpathy), Engram.** FDD ya tiene su memoria mínima (mapa,
  decisiones cerradas, ficha) y la midió. Astra lo dejó como «experimento de interoperabilidad cuando exista
  consumidor concreto» (`revision-unificada-astra.md:69`). El video no trae medición. No adoptar.
- **11 · conectores y scripts que el harness registra solo; instalador del meta-harness.** Es tooling, no
  método. FDD exige auditar herramientas de terceros antes (`SKILL:56-58`). Fuera de alcance.
- **2 · «3 o 4 horas sin preguntar» como objetivo.** FDD mide autonomía por alcance autorizado, no por
  tiempo; lo irreversible y lo que cambia una decisión espera (`SKILL:63-64`, `93`). Contradice en parte
  el espíritu de «no lo hace y avisa después». No adoptar como meta.
- **3 y 12 · cadena de fases (spec → plan → tareas) con «preguntas muy claras» como etapa.** FDD es una
  ficha, no fases (`README:296-297`); ya descartado por Astra (`SKILL:437-440`). ASDD mejora SDD; FDD partió
  de otro lado.
- **6 · fase de «autorización» (¿me pedís un cambio?) como paso formal.** FDD lo resuelve en la regla 2
  sin ceremonia. No agregar.

## C. Contraste con Astra y las lecturas previas

**Refuerza lo ya tomado.** Las tres ideas que Astra extrajo de ASDD (demostrar el requisito, revisar
durante el trabajo, decidir sin frenar lo independiente; `analisis-holamundo-sdd:200-209`,
`revision-unificada-astra.md:69`) aparecen en el video dichas por su autor con los mismos términos. También
refuerza la crítica a SDD del README (`README:40-44`): Risco llega a lo mismo por su cuenta («adaptada para
humanos pero no para agentes»). Y confirma de primera mano que FTD es la fuente correcta de la ficha.

**Nuevo respecto de Astra.** (a) La lección de FTD sobre el riesgo de lo trivial (login → entrar rompe
registro) con su corrección: una evaluación de riesgo antes de tocar. (b) La crítica a la clasificación
pequeño/grande como punto único de falla, que alcanza a la regla 2 de FDD. (c) La tesis de que el método
caduca con el modelo. (d) El mecanismo explícito «lista de decisiones que esperan + informe al final»; Astra
lo tenía como «decisiones recuperables» y «continuar lo independiente», sin la lista como artefacto.
(e) Un dato de proceso: el propio autor de FTD reconoce que la clasificación rápida falló y la corrigió; es
evidencia sobre la fuente de la que FDD tomó la ficha.

**Ya descartado, y el video no lo revive.** Cadena completa de documentos de RSC, clasificación de riesgo
como autoridad, ratificaciones provisionales y auto-merge (`SKILL:437-440`, Astra:69): el video ni los
menciona. OKF como memoria: lo menciona sin medición; sigue como experimento (Astra:69). Topes universales y
pausa por test de HolaMundo (`SKILL:441-443`): el video recomienda «tareas pequeñas» sin topes, coincide con
FDD. Cifras de Ponytail: no aplican.

**Decisión de Ariel que se respeta.** RSC 2.0/ASDD y HolaMundo no se adoptan como tales; las ideas entran
«vía la revisión de Astra». Este video no cambia eso: lo nuevo (a)–(d) son lecciones puntuales que se
atribuyen al video y se escriben en el estilo de la skill.

## D. Propuesta de cambios (priorizada)

Formato: texto propuesto · sección · fuente · riesgo.

1. **Puntual no es sin riesgo.**
   «Antes de un cambio puntual se mira qué más usa lo que se toca (quién lo llama, qué comparte el
   componente, qué prueba lo cubre) y se verifica eso también, no sólo lo pedido. Cambiar el texto de un
   botón puede cambiar el de al lado.» · *Dos reglas de fondo*, regla 2 (`SKILL:21-23`) y eco en
   `README:178-179` · Video, afirmación 10 (lección de FTD) · Riesgo bajo; se hace, no se documenta, para no
   inflar la fricción que la regla 2 quiere baja.

2. **Clasificar mal no exime; lo puntual que crece abre ficha.**
   «Si un cambio puntual, al hacerlo, toca otro archivo, un contrato o una decisión, deja de ser puntual:
   se abre la ficha ahí mismo con lo ya hecho como evidencia. Clasificar mal no exime de nada: la
   constitución, la arquitectura y la verificación valen igual.» · *Dos reglas de fondo* (después de la
   regla 2) · Video, afirmación 7 (crítica a ODD) · Riesgo bajo; hace explícito el amortiguador que FDD ya
   tiene en la regla 1.

3. **Decisiones que esperan a la persona, en la ficha y en el informe.**
   Plantilla §4, sección «Trabajo y decisiones»: «Tareas, dependencias y decisiones tomadas con su motivo.
   **Decisiones que esperan a la persona**: qué se preguntó, las opciones, qué tarea quedó frenada y qué se
   siguió mientras tanto.» Informe §8 (`SKILL:395-397`): sumar «decisiones que esperan» entre «pedidos a la
   coordinadora» y «lo visto fuera de su carpeta». · `SKILL:176-177` y `SKILL:395-397` · Video, afirmación
   13 (ASDD) · Riesgo bajo; `SKILL:343-344` ya impide que una decisión anotada valga como autoridad.

4. **Un adjetivo no es un criterio.**
   «"Seguro", "rápido", "usable" no son criterios: se convierten en condición → acción → resultado observable
   antes de diseñar; si no se puede, es una pregunta a la persona, no una libertad del agente.» · §4, al pie
   de «Criterios y demostración» (`SKILL:147-148`), o §3 · Video, afirmación 5 (ejemplo «seguro» → 2FA) ·
   Riesgo bajo; complementa «Demostrar el requisito» (`SKILL:335-339`), que actúa después.

5. **§9 y README: registrar este video y precisar FTD.**
   En «Lecturas consideradas» (`SKILL:434-450` y `README:318-332`): «**Video "Ninguna metodología..." (Eric
   Risco, 2026-11-05)**: el riesgo de lo trivial y el puntual que crece (reglas de fondo), decisiones que
   esperan (§4, §8), adjetivo no es criterio (§4). No se toman la memoria tipo cerebro (OKF) ni el tiempo sin
   preguntar como meta.» Y en la entrada FTD (`SKILL:423-424`): «FTD y ASDD son dos skills del mismo
   harness; de FTD se toman la ficha y la evidencia, no su clasificación rápida, que su autor corrigió.» ·
   §9, README «Fuentes» y «Cambios» (2.3) · Riesgo nulo.

6. **Reglas que dependen del modelo, fechadas y re-medibles.**
   «Hay reglas que compensan una limitación del modelo (el mapa para ahorrar tokens, el detalle sólo de la
   próxima ola) y se re-miden cuando el modelo cambia; y reglas que son contrato con la persona
   (constitución, aceptación, evidencia observada) y no dependen del modelo. Las primeras llevan fecha y
   medición; las segundas no se relajan por un modelo mejor.» · §9, antes de «Fuentes adoptadas»
   (`SKILL:415-418`) · Video, afirmación 1 · Riesgo medio: puede leerse como licencia para aflojar; por eso
   separa las dos clases. Prioridad baja; conviene si Ariel va a seguir revisando FDD con cada video.

7. **Cerrar la ola sobre el árbol integrado actual.** (prioridad baja)
   «La revisión de cierre de la ola se hace sobre el árbol integrado al momento del cierre, no sobre la
   copia con la que se empezó.» · §6, viñeta «Cada ola cierra con revisión de código» (`SKILL:285-286`) ·
   Video, afirmación 13 («verifica con main») · Riesgo bajo; `SKILL:381-384` ya pide compilar desde copia
   limpia, esto sólo fija el momento.

**No conviene adoptar** (con motivo en B): memoria tipo «cerebro»/OKF/Engram (sin medición; Astra lo dejó
como experimento); conectores autogestionados e instalador de meta-harness (tooling, no método); «horas sin
preguntar» como objetivo (FDD mide por alcance autorizado); cadena de fases spec → plan → tareas (FDD es una
ficha); fase formal de «autorización»; la tabla de «riesgos» como sección nueva de la ficha (lo cubren D1 y
«Efectos compartidos»; Astra pidió no importar la clasificación de riesgo como autoridad).

## E. Contradicciones internas y texto viejo detectado

1. **«Dos reglas» vs «Tres reglas».** `SKILL:17` «Dos reglas de fondo»; `README:174-181` «Tres reglas
   atraviesan todo» (la tercera es «lo que se afirma sin evidencia cuenta como no hecho», que en la skill
   está en §7 `SKILL:323`). Elegir una cuenta o decir en la skill que la tercera vive en §7.
2. **Dos formatos para la tabla de olas.** `SKILL:171-174` (Ola · Termina con · Pruebas · Depende de ·
   Estado) y `SKILL:271` «tabla (ola, esfuerzo, depende de)». Unificar en una.
3. **«Integrado» significa dos cosas.** Como hito de feature lo marca una persona (`SKILL:320`); como
   estado de ola lo hace la coordinadora y «integrar no es aceptar» (`SKILL:301-304`). Los dos vocabularios
   (diseñado/implementado/verificado/integrado vs en curso/entregada/integrada/aceptada) conviven sin que se
   diga cuál aplica a qué. Aclarar: la ola se integra y se acepta; la feature se marca integrada cuando la
   persona aceptó las olas que la cubren.
4. **Afirmación sin fuente en la skill.** `SKILL:236-238` «Así se construyó de punta a punta un sistema de
   cobros en producción»: sin proyecto ni fecha, a diferencia del resto de §6, que fecha cada lección.
5. **Dos lugares para reglas durables.** `SKILL:59-61` manda las «Reglas que no se rompen» a
   `docs/dominio.md`; `SKILL:75-76` manda las decisiones cerradas al `AGENTS.md` (o índice de docs). Puede ser
   intencional (invariantes vs decisiones), pero no se dice.
6. **Nombre «RSC 2.0».** `SKILL:14`, `423`, `437` y `README:321`, `353` usan «RSC 2.0/ASDD». En este video
   Risco llama RSC al meta-harness y ASDD al método; «2.0» es etiqueta de Lienzo
   (`analisis-holamundo-sdd:196-197`). No es error, pero conviene decir «RSC Harness (ASDD)» para que un
   lector externo lo encuentre.
7. **Fecha del video anterior.** §9 dice «video de Eric Risco, 2026-11-03»; el análisis de Lienzo fecha
   el reemplazo el 2026-11-04 sin dar fecha de publicación. No se pudo verificar; marcar como «visto el».
8. **Ya anotado en PENDIENTES.** Figuras SVG, Mermaid del README y «Lo medido» (−43 %) reflejan el modelo
   anterior (`PENDIENTES.md`, entrada 2026-11-05). Sigue abierto; no se repite acá.
9. **Tabla comparativa del README.** `README:300-307` dice SDD «Para: agentes, trabajo grande» mientras
   `README:40-44` y el video dicen que SDD «nació para modelos que necesitaban que se les explicara todo» /
   «adaptada para humanos». No es contradicción estricta, pero la fila no refleja la postura del texto de
   arriba.

## Límites

Transcripción automática: hay errores de reconocimiento («RSDC», «Carpaci», «asd»); se citó lo que se
entiende sin ambigüedad. No se vio el video ni se verificó contra las skills de rsc-harness ni de Gentle AI
en GitHub; las descripciones de ODD y ASDD son las del autor del video. No se midió nada; las propuestas son
de texto y su efecto queda por observar en uso.
