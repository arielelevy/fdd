# Revisión del cambio documental

Fecha: 2026-10-31.
Alcance: diff de `skills/fdd/SKILL.md` y contenido nuevo de `PENDIENTES.md`, contrastados con la skill completa y el README.

## Hallazgos corregidos

- `skills/fdd/SKILL.md`, §4: permitir omitir la suite completa contradecía §6. El control de deriva ahora se exige al cambiar la versión y se aclara que no sustituye la suite de cierre. Se quitó la duración no medida.
- `skills/fdd/SKILL.md`, §8: «una sola vez al cierre» podía postergar controles de contratos. Se separaron las mediciones finales de los controles por cambio de versión.
- `skills/fdd/SKILL.md`, §7: se incluyeron los efectos compartidos en las condiciones del hito diseñado para mantener coherencia con la nueva plantilla.
- `PENDIENTES.md`: la introducción decía que las propuestas no estaban aplicadas, aunque cada entrada declaraba lo contrario. Se aclaró que el texto está incorporado y las cinco propuestas siguen pendientes de validación de Ariel.

## Validación y límites

Revisión manual de coherencia entre plantilla, hitos y coordinación. Sin hallazgos bloqueantes restantes en el alcance revisado.

Sólo se modifica Markdown. No hay código ejecutable, dependencias ni configuración de pruebas en este repositorio; no corresponde ejecutar el recorrido de pruebas agénticas de código. No se verificaron los hechos históricos de openSeec contra ese proyecto ni se afirman nuevas mediciones.

Las cinco casillas de pendientes se conservan abiertas. El push no marca su validación ni el hito integrado.

## Seguimiento: cierre solicitado por Ariel

Después del primer push, Ariel pidió hacer las cinco tareas pendientes. Se volvió a leer la skill y se comprobó que las cinco modificaciones documentales ya estaban incorporadas en `0ef22fc`:

| Propuesta | Evidencia en `skills/fdd/SKILL.md` |
|---|---|
| Ficha y encargo únicos | §4 reutiliza la ficha que contiene el encargo; §8 exige que el encargo sea la ficha y se actualice por hitos. |
| Efectos compartidos | §4 incluye la sección con responsables y momentos; §7 la exige para diseñado; §8 reparte los efectos antes del trabajo. |
| Proteger evidencia citada | §4 registra las rutas; §8 exige buscar referencias y confirmar antes de borrar evidencia citada. |
| Dominio con backend | §2 incluye explícitamente datos, motores, servicios y datasets además de pantallas. |
| Verificación real | §7 exige comprobar al menos un criterio por el camino real e incluye los ejemplos de carpeta vacía y rutas agregadas por el runner. |

Se marcaron las cinco entradas como cerradas por ese pedido explícito. No se modificó código ni se añadió una afirmación de pruebas funcionales ejecutadas. La anotación anterior conserva el estado del primer push; este seguimiento registra el cierre posterior.
