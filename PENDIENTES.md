# Pendientes de FDD

- [x] **2026-11-01 · Maqueta inicial y criterio de la coordinadora IA en olas posteriores.** *Aplicado en `skills/fdd/SKILL.md` §5 «Quién aprueba» (y hito *diseñado* en §7), incorporado en `0942a68`; la adaptación de Lienzo (`skills/lienzo/references/fdd.md`, «Aprobación») ya dice lo mismo. Queda que Ariel confirme el cierre, como en las entradas de `0ef22fc`.* Decisión explícita de Ariel desde OpenSeec ola20: «maqueta es al principio y en las olas a criterio de la coordinadora». Mantener la maqueta inicial antes de construir la app. En las olas posteriores, la coordinadora IA decide si hace falta una maqueta nueva, la revisa y la aprueba dentro del alcance y diseño autorizados; no pedir aprobación humana por cada ajuste de UI. Esto no amplía permisos para cambiar alcance, arquitectura, contratos o decisiones cerradas. Actualizar `skills/fdd/SKILL.md:170` (§5, aprobación atribuida siempre a la persona) y coordinar su adaptación `C:/Users/ArielLevy/OneDrive - smartBI/claude-skills/lienzo/references/fdd.md:58` («aprobado por Ariel antes de construir»). Reproducción: una ola agrega un formulario a una app existente; la regla actual frena la implementación esperando aprobación humana de otra maqueta. Criterio esperado: la coordinadora registra su decisión y sigue sin esa espera. Origen: OpenSeec UX-08, maqueta `maqueta/ola20-instrucciones.html`, sesión `01a19844-e68c-7d51-9ce9-35fb5ea66458`. Entrada pendiente para incorporar en FDD; no se marca completada. Texto literal de Ariel en el transcript (2026-11-01T21:3xZ): «si es cmabio e la app de ola la mauqets es ac irterio del coordinador ia» y «mandaelo a fdd maquete es al princio y eb las oas a criterio d ela coordinadorw». Propuesta de aplicación (2026-11-04): `D:/Apps/lienzo/.temp/coordinacion/2026-11-04/fdd-global-propuesta.patch`, §5 «Quién aprueba»; sin integrar ni publicar.

Las cinco propuestas se incorporaron en `0ef22fc`. Cierre solicitado por Ariel el 2026-10-31 y comprobado contra la skill. Cada entrada conserva su origen y evidencia.

- [x] **2026-10-30 · Ficha y encargo en un solo documento.** *Aplicado en `skills/fdd/SKILL.md` §8 «El encargo de un frente es su ficha», incorporado en `0ef22fc`; cerrado por pedido de Ariel (2026-10-31).* En openSeec 0.19.0 (cinco frentes Claude coordinados por
  Lienzo) la ficha FDD y el encargo del frente decían casi lo mismo: dos archivos por feature, o una ficha que sólo
  apunta al encargo. Propuesta: que el encargo de cada frente *sea* la ficha (con `Hito:`, «Archivos y dueños» y
  «Dominio»), para no duplicar.
- [x] **2026-10-30 · Sección «efectos compartidos» obligatoria en la ficha.** *Aplicado en `skills/fdd/SKILL.md` §4 «Efectos compartidos» y §8 reparto antes, incorporado en `0ef22fc`; cerrado por pedido de Ariel (2026-10-31).* En openSeec 0.18.0 la suite global encontró
  10 fallas al cierre; casi todas eran pines, sellos y versiones que no siguieron el cambio de otro dueño
  (`EXPORT_VERSION`, el hash sellado de un contrato, opciones nuevas del servidor), más una prueba de localidad que hubo
  que re-medir. Propuesta: la ficha lista qué versiones, contratos, sellos y pruebas medidas toca, para planificar
  re-sellos y re-mediciones antes de repartir en vez de descubrirlos en la suite global.
- [x] **2026-10-30 · Evidencia citada protegida.** *Aplicado en `skills/fdd/SKILL.md` §4 «Evidencia» y §8 limpieza de temporales, incorporado en `0ef22fc`; cerrado por pedido de Ariel (2026-10-31).* La coordinadora borró por error, al limpiar temporales, la carpeta con
  el arnés y la evidencia de una prueba de localidad que un contrato citaba por ruta; hubo que reconstruir el arnés y
  re-medir. Propuesta: la ficha nombra la evidencia que respalda cada hito, y antes de limpiar se buscan las rutas
  citadas por fichas y contratos.
- [x] **2026-10-30 · El mapa del dominio también cubre el backend.** *Aplicado en `skills/fdd/SKILL.md` §2, incorporado en `0ef22fc`; cerrado por pedido de Ariel (2026-10-31).* En openSeec `docs/dominio.md` sólo indexaba la UI
  (corrida, motor, señal, informe, configuración); las features de dependencias PHP/NuGet tocaban entidades que no
  figuraban. Propuesta: la guía del mapa pide entidades de datos y de motores, no sólo pantallas.
- [x] **2026-10-30 · Lo que FDD no reemplaza.** *Aplicado en `skills/fdd/SKILL.md` §7 «Verificado es por el camino real», incorporado en `0ef22fc`; cerrado por pedido de Ariel (2026-10-31).* Los defectos más graves de esa ronda (tests que pasaban sin verificar nada
  porque recorrían una carpeta vacía; diferencia entre el `sys.path` de los tests y el de producción) se encontraron con
  verificación real (arrancar como producción, re-medir), no con documentos. Propuesta: decirlo en la guía, junto a
  «evidencia observada, no afirmada», con un ejemplo de verificación por el camino real.
