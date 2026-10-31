# Pendientes de FDD

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
