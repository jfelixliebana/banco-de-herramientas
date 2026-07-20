# Especificación de Feature: Ciclo de solicitud y préstamo de herramientas

**Rama**: `004-solicitudes-prestamo`
**Creada**: 2026-07-15
**Estado**: Borrador
**Input**: Descripción del usuario: "Banco Comunitario de Herramientas — MVP. EPIC 4 (Solicitudes de préstamo): el corazón del producto. Un prestatario solicita una herramienta; el propietario acepta o rechaza manualmente; se acuerda la entrega; se confirma la entrega y, al final, la devolución; ambas partes pueden cancelar y consultar su historial. No hay pagos: la plataforma solo facilita el contacto."

> **Nota de prioridades**: el backlog original usa P0/P1/P2. Aquí se traduce al estándar de la skill: **P0→P1** (imprescindible), **P1→P2** (importante), **P2→P3** (deseable).

## Escenarios de usuario y pruebas *(obligatorio)*

### Historia de Usuario 1 - Solicitar un préstamo (Prioridad: P1)

Como vecino que ha encontrado la herramienta que necesita, quiero solicitar el préstamo a su propietario, para iniciar el acuerdo de recogida.

**Por qué esta prioridad**: es el disparador del ciclo de valor completo. Sin solicitud no hay préstamo, y validar que la gente solicita es el objetivo central del MVP.

**Test independiente**: se puede probar solicitando una herramienta disponible dentro de alcance y comprobando que se crea una solicitud "pendiente" notificada al propietario.

**Escenarios de aceptación**:
1. **Dado** que soy un usuario autenticado y veo una herramienta disponible dentro de mi alcance, **Cuando** solicito el préstamo indicando (opcionalmente) fechas o un mensaje, **Entonces** se crea una solicitud en estado "pendiente" asociada a esa herramienta y al propietario, que recibe un aviso.
2. **Dado** que la herramienta pasó a "no disponible" o quedó fuera de mi alcance justo antes de enviar, **Cuando** intento solicitarla, **Entonces** el sistema me lo impide y me informa del motivo.
3. **Dado** que ya tengo una solicitud "pendiente" o "aceptada" sobre esa misma herramienta, **Cuando** intento solicitarla de nuevo, **Entonces** el sistema evita la solicitud duplicada. [NECESITA ACLARACIÓN: ¿se permite a un mismo prestatario tener varias solicitudes sobre la misma herramienta en distintas fechas?]
4. **Dado** que intento solicitar mi propia herramienta, **Cuando** lo hago, **Entonces** el sistema lo impide.

### Historia de Usuario 2 - Aceptar una solicitud (Prioridad: P1)

Como propietario, quiero aceptar manualmente las solicitudes que me convencen, para mantener el control sobre a quién y cuándo presto.

**Por qué esta prioridad**: la aceptación manual es una decisión funcional explícita del MVP y el mecanismo por el que el propietario ejerce la confianza. Es imprescindible.

**Test independiente**: se puede probar aceptando una solicitud pendiente y comprobando que pasa a "aceptada" y que el prestatario es avisado.

**Escenarios de aceptación**:
1. **Dado** que tengo una solicitud "pendiente", **Cuando** la acepto, **Entonces** pasa a "aceptada", el prestatario recibe un aviso y ambos disponen de un canal para acordar la entrega.
2. **Dado** que tengo varias solicitudes pendientes sobre la misma herramienta, **Cuando** acepto una, **Entonces** el sistema me indica cómo quedan las demás. [NECESITA ACLARACIÓN: ¿aceptar una rechaza automáticamente las otras, o pueden convivir?]
3. **Dado** que una solicitud fue cancelada por el prestatario, **Cuando** intento aceptarla, **Entonces** el sistema me informa de que ya no es válida.

### Historia de Usuario 3 - Rechazar una solicitud (Prioridad: P1)

Como propietario, quiero rechazar una solicitud que no quiero atender, para cerrarla con claridad y no dejar al prestatario esperando.

**Por qué esta prioridad**: es la contraparte necesaria de la aceptación; sin rechazo explícito, las solicitudes quedan en un limbo que daña la confianza y el índice de respuesta.

**Test independiente**: se puede probar rechazando una solicitud pendiente y comprobando que pasa a "rechazada" y que el prestatario es avisado.

**Escenarios de aceptación**:
1. **Dado** que tengo una solicitud "pendiente", **Cuando** la rechazo (con motivo opcional), **Entonces** pasa a "rechazada" y el prestatario recibe un aviso.
2. **Dado** que rechazo una solicitud, **Cuando** se cierra, **Entonces** la herramienta sigue disponible para otras solicitudes.

### Historia de Usuario 4 - Confirmar la entrega (Prioridad: P2)

Como parte de un préstamo aceptado, quiero confirmar que la herramienta ha cambiado de manos, para dejar constancia del inicio del préstamo.

**Por qué esta prioridad**: da trazabilidad al préstamo y habilita el cierre y la reputación, pero el contacto para acordar la entrega ya ocurre tras aceptar; por eso es importante, no imprescindible en la primera iteración.

**Test independiente**: se puede probar confirmando la entrega sobre una solicitud aceptada y comprobando que el préstamo pasa a "en curso".

**Escenarios de aceptación**:
1. **Dado** que existe una solicitud "aceptada", **Cuando** se confirma la entrega, **Entonces** el préstamo pasa a "en curso" y la herramienta refleja que está prestada.
2. **Dado** que solo una de las dos partes confirma la entrega, **Cuando** transcurre el acuerdo, **Entonces** el sistema refleja el estado según la regla de confirmación definida. [NECESITA ACLARACIÓN: ¿la entrega la confirma solo el propietario, solo el prestatario, o requiere ambas partes?]

### Historia de Usuario 5 - Confirmar la devolución (Prioridad: P2)

Como parte de un préstamo en curso, quiero confirmar que la herramienta ha vuelto a su dueño, para cerrar el préstamo y poder valorarnos.

**Por qué esta prioridad**: cierra el ciclo y es la puerta a la reputación (EPIC 6); importante para el aprendizaje del modelo, pero el préstamo puede ocurrir físicamente sin este registro en una primera versión.

**Test independiente**: se puede probar confirmando la devolución de un préstamo en curso y comprobando que pasa a "completado".

**Escenarios de aceptación**:
1. **Dado** que existe un préstamo "en curso", **Cuando** se confirma la devolución, **Entonces** el préstamo pasa a "completado", la herramienta vuelve a estar disponible y se habilita la valoración mutua.
2. **Dado** que se confirma la devolución, **Cuando** el préstamo se cierra, **Entonces** se incrementan los contadores de préstamos realizados de ambas partes.
3. **Dado** que la devolución no llega a confirmarse, **Cuando** pasa el tiempo, **Entonces** el préstamo permanece "en curso". [NECESITA ACLARACIÓN: ¿existe un cierre por inactividad o un aviso? Recordar que la plataforma no interviene en incidencias en el MVP.]

### Historia de Usuario 6 - Cancelar una solicitud (Prioridad: P2)

Como prestatario (o como propietario antes de la entrega), quiero cancelar una solicitud que ya no procede, para no bloquear la herramienta ni al otro.

**Por qué esta prioridad**: aporta higiene al flujo y evita solicitudes zombis, pero el ciclo básico funciona con aceptar/rechazar; por eso es importante, no imprescindible.

**Test independiente**: se puede probar cancelando una solicitud propia "pendiente" o "aceptada" (antes de la entrega) y comprobando que pasa a "cancelada".

**Escenarios de aceptación**:
1. **Dado** que soy el prestatario y tengo una solicitud "pendiente" o "aceptada" sin entrega confirmada, **Cuando** la cancelo, **Entonces** pasa a "cancelada" y el propietario es avisado.
2. **Dado** que la entrega ya está confirmada (préstamo "en curso"), **Cuando** intento cancelar, **Entonces** el sistema no lo permite y me indica que el flujo aplicable es la devolución.

### Historia de Usuario 7 - Consultar mi historial de préstamos (Prioridad: P2)

Como usuario, quiero ver mis préstamos pasados y en curso, como prestamista y como prestatario, para hacer seguimiento y recordar con quién he tratado.

**Por qué esta prioridad**: da continuidad y contexto de confianza, y alimenta la percepción de reputación, pero no es imprescindible para completar un préstamo suelto.

**Test independiente**: se puede probar consultando el historial de un usuario con préstamos en varios estados y comprobando que aparecen correctamente clasificados.

**Escenarios de aceptación**:
1. **Dado** que he participado en préstamos, **Cuando** abro mi historial, **Entonces** veo mis solicitudes y préstamos con su estado (pendiente, aceptada, rechazada, cancelada, en curso, completado) y mi rol en cada uno.
2. **Dado** que no tengo actividad, **Cuando** abro mi historial, **Entonces** veo un estado vacío explicativo.

### Casos límite

- ¿Qué pasa si el propietario no responde nunca a una solicitud? (Impacto en el índice de respuesta; ¿caducan las solicitudes? [NECESITA ACLARACIÓN])
- ¿Qué ocurre si el propietario marca la herramienta "no disponible" con solicitudes pendientes? (Ver EPIC 2.)
- ¿Qué pasa si la herramienta se daña o no se devuelve? (La plataforma **no interviene en incidencias ni daños** en el MVP; solo se contempla registro de incidencias en Administración — EPIC 7.)
- ¿Puede el prestatario tener varios préstamos en curso a la vez? ¿Y el mismo objeto prestado a dos personas? (No: una herramienta en curso no puede prestarse a otra persona simultáneamente.)
- ¿Qué pasa si una de las partes elimina su cuenta a mitad de un préstamo?
- ¿Se pueden pactar y registrar fechas de recogida/devolución, o solo la "duración máxima recomendada" es informativa? [NECESITA ACLARACIÓN]

## Requisitos *(obligatorio)*

### Requisitos funcionales

- **FR-001**: El sistema DEBE permitir a un usuario autenticado solicitar el préstamo de una herramienta disponible que esté dentro de su alcance, y no de su propiedad.
- **FR-002**: El sistema DEBE crear cada solicitud en estado "pendiente" y avisar al propietario.
- **FR-003**: El sistema DEBE evitar solicitudes duplicadas activas de un mismo prestatario sobre una misma herramienta. [NECESITA ACLARACIÓN: definición exacta de "duplicada"]
- **FR-004**: El sistema DEBE requerir la aceptación **manual** del propietario para que una solicitud avance; nunca se acepta de forma automática.
- **FR-005**: El sistema DEBE permitir al propietario aceptar o rechazar una solicitud pendiente, avisando al prestatario del resultado.
- **FR-006**: El sistema DEBE permitir cancelar una solicitud antes de que la entrega esté confirmada, avisando a la otra parte.
- **FR-007**: El sistema DEBE registrar la confirmación de entrega y pasar el préstamo a "en curso", reflejando que la herramienta está prestada. [NECESITA ACLARACIÓN: quién confirma la entrega]
- **FR-008**: El sistema DEBE registrar la confirmación de devolución, pasar el préstamo a "completado" y devolver la herramienta a "disponible".
- **FR-009**: El sistema DEBE impedir que una herramienta con un préstamo "en curso" sea prestada simultáneamente a otra persona.
- **FR-010**: El sistema DEBE incrementar los contadores de préstamos realizados de ambas partes al completarse un préstamo.
- **FR-011**: El sistema DEBE habilitar la valoración mutua solo cuando el préstamo está "completado" (ver EPIC 6).
- **FR-012**: El sistema DEBE permitir a cada usuario consultar su historial de solicitudes y préstamos con su estado y su rol.
- **FR-013**: El sistema DEBE alimentar el "índice de respuesta" del propietario en función de si atiende (acepta/rechaza) sus solicitudes. [NECESITA ACLARACIÓN: fórmula y ventana temporal; ver EPIC 6]
- **FR-014**: El sistema DEBE limitarse a facilitar el contacto entre las partes, sin gestionar pagos ni intervenir en incidencias o daños durante el préstamo.

### Entidades clave

- **Solicitud de préstamo**: petición de un prestatario sobre una herramienta. Atributos: prestatario, herramienta (y su propietario), estado (pendiente, aceptada, rechazada, cancelada), momento de creación, mensaje o fechas opcionales.
- **Préstamo**: relación activa que nace de una solicitud aceptada y entregada. Atributos: solicitud de origen, estado (en curso, completado), momentos de entrega y devolución. Relación: conecta a prestamista y prestatario en torno a una herramienta y habilita las valoraciones.
- **Estado de la solicitud/préstamo**: ciclo de vida — pendiente → aceptada/rechazada/cancelada → en curso → completado.
- *(Reutiliza **Usuario** de EPIC 1 y **Herramienta** de EPIC 2.)*

## Criterios de éxito *(obligatorio)*

### Resultados medibles

- **SC-001**: Al menos el 60% de las solicitudes enviadas reciben respuesta (aceptación o rechazo) del propietario en menos de 24 horas.
- **SC-002**: Al menos el 50% de las solicitudes aceptadas terminan en un préstamo completado.
- **SC-003**: Al menos el 30% de los prestatarios que completan un préstamo vuelven a solicitar otro en los 60 días siguientes (validación de que el modelo genera actividad recurrente).
- **SC-004**: Menos del 10% de los préstamos "en curso" quedan sin cerrar (sin devolución confirmada) pasado el doble de su duración máxima recomendada.

## Suposiciones

- Las cifras objetivo de los criterios de éxito son propuestas iniciales y deben validarse con negocio.
- **No hay pagos** en el MVP: la solicitud y el préstamo son gratuitos.
- La plataforma **solo facilita el contacto**; el acuerdo concreto de dónde y cuándo se entrega ocurre entre las partes (apoyado por la feature de Comunicación, EPIC 5).
- La plataforma **no interviene en incidencias ni daños** durante el préstamo ni garantiza el estado de la herramienta.
- La valoración mutua y el cálculo de reputación viven en EPIC 6; aquí solo se habilita el punto de disparo al completar el préstamo.
- Los avisos al propietario y al prestatario se apoyan en la feature de Notificaciones (EPIC 5).
