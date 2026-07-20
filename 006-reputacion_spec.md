# Especificación de Feature: Reputación y confianza entre vecinos

**Rama**: `006-reputacion`
**Creada**: 2026-07-15
**Estado**: Borrador
**Input**: Descripción del usuario: "Banco Comunitario de Herramientas — MVP. EPIC 6 (Reputación): tras completar un préstamo, prestamista y prestatario pueden valorarse mutuamente, y cualquiera puede consultar la reputación de otro usuario. La reputación (Trust Score) es el mecanismo que sostiene la confianza en una plataforma sin pagos ni verificación de identidad."

> **Nota de prioridades**: el backlog original usa P0/P1/P2. Aquí se traduce al estándar de la skill: **P0→P1** (imprescindible), **P1→P2** (importante), **P2→P3** (deseable).

## Escenarios de usuario y pruebas *(obligatorio)*

### Historia de Usuario 1 - Valorar a la otra parte tras el préstamo (Prioridad: P2)

Como participante de un préstamo completado, quiero valorar a la otra persona, para premiar el buen comportamiento y avisar a la comunidad de las malas experiencias.

**Por qué esta prioridad**: es la materia prima de la confianza que el modelo necesita. El backlog la marca como importante (no imprescindible) porque el ciclo de préstamo puede completarse sin valoración, pero sin ella el sistema de confianza no se alimenta.

*(Cubre HU-025 "Valorar prestatario" y HU-026 "Valorar prestamista": mismo journey desde cada rol.)*

**Test independiente**: se puede probar completando un préstamo y comprobando que cada parte puede dejar una valoración que se refleja en la reputación de la otra.

**Escenarios de aceptación**:
1. **Dado** que he participado en un préstamo "completado", **Cuando** valoro a la otra parte (puntuación y comentario opcional), **Entonces** la valoración queda registrada y afecta a su valoración media pública.
2. **Dado** que un préstamo no está "completado", **Cuando** intento valorar, **Entonces** el sistema no me lo permite (solo se valora sobre préstamos cerrados).
3. **Dado** que ya he valorado ese préstamo, **Cuando** intento valorar otra vez, **Entonces** el sistema me lo impide o me permite editar la valoración existente. [NECESITA ACLARACIÓN: ¿la valoración es editable tras enviarla y durante cuánto tiempo?]
4. **Dado** que la otra parte aún no me ha valorado, **Cuando** dejo mi valoración, **Entonces** el sistema decide cuándo se hace visible. [NECESITA ACLARACIÓN: ¿se publica al instante o solo cuando ambos han valorado / al vencer un plazo, para evitar valoraciones de represalia?]

### Historia de Usuario 2 - Consultar la reputación de un usuario (Prioridad: P2)

Como vecino a punto de prestar o pedir prestado, quiero consultar la reputación de la otra persona, para decidir si me fío antes de comprometerme.

**Por qué esta prioridad**: es el consumo de la confianza acumulada; sin poder consultarla, valorar no serviría de nada. Importante en cuanto exista actividad que valorar.

**Test independiente**: se puede probar consultando la reputación de un usuario con valoraciones y comprobando que se muestran los indicadores del Trust Score.

**Escenarios de aceptación**:
1. **Dado** que consulto a un usuario, **Cuando** veo su reputación, **Entonces** aparecen su valoración media, el número de valoraciones, el número de préstamos completados, su índice de respuesta, su fecha de alta y su estado de verificación (el conjunto "Trust Score").
2. **Dado** que consulto a un usuario sin valoraciones aún, **Cuando** veo su reputación, **Entonces** los indicadores sin datos se muestran como "sin valoraciones aún" y no como un cero engañoso.
3. **Dado** que consulto valoraciones con comentario, **Cuando** las abro, **Entonces** veo los comentarios asociados y de qué rol provienen (como prestamista o como prestatario). [NECESITA ACLARACIÓN: ¿se muestran los comentarios individuales públicamente o solo agregados?]

### Casos límite

- ¿Qué pasa con la reputación cuando un usuario elimina su cuenta? ¿Se conservan las valoraciones que emitió sobre otros?
- ¿Cómo se evitan valoraciones de represalia (te pongo mala nota porque tú me la pusiste)?
- ¿Se puede reportar una valoración injusta o abusiva? (Enlaza con moderación — EPIC 7.)
- ¿Cómo afecta a la valoración media un usuario con muy pocas valoraciones frente a otro con muchas? [NECESITA ACLARACIÓN: ¿se muestra la media tal cual o con algún mínimo de valoraciones?]
- ¿El índice de respuesta cuenta también las solicitudes que el propietario deja caducar sin responder?
- ¿Qué ocurre si un préstamo se completa pero una parte nunca valora? (La media se calcula solo con lo recibido.)

## Requisitos *(obligatorio)*

### Requisitos funcionales

- **FR-001**: El sistema DEBE permitir a cada participante de un préstamo "completado" valorar a la otra parte con una puntuación y un comentario opcional.
- **FR-002**: El sistema DEBE permitir la valoración únicamente sobre préstamos en estado "completado".
- **FR-003**: El sistema DEBE limitar a una valoración por participante y por préstamo. [NECESITA ACLARACIÓN: ¿editable tras el envío?]
- **FR-004**: El sistema DEBE recoger la valoración en ambos sentidos (prestamista→prestatario y prestatario→prestamista).
- **FR-005**: El sistema DEBE calcular y exponer la valoración media pública de cada usuario a partir de las valoraciones recibidas.
- **FR-006**: El sistema DEBE mostrar el conjunto de indicadores de confianza ("Trust Score"): valoración media, número de valoraciones, número de préstamos completados, índice de respuesta, fecha de alta y estado de verificación.
- **FR-007**: El sistema DEBE calcular el "índice de respuesta" del usuario. [NECESITA ACLARACIÓN: definición exacta — p. ej. % de solicitudes respondidas (aceptadas o rechazadas) sobre el total, y en qué ventana de tiempo]
- **FR-008**: El sistema DEBE representar la ausencia de datos ("sin valoraciones aún") sin mostrar valores engañosos.
- **FR-009**: El sistema DEBE definir la política de visibilidad de las valoraciones para mitigar represalias. [NECESITA ACLARACIÓN: publicación inmediata vs. doble ciego con plazo]
- **FR-010**: El sistema DEBE conservar la coherencia de la reputación cuando una de las partes deja de existir. [NECESITA ACLARACIÓN: qué ocurre con valoraciones de/para cuentas eliminadas]

### Entidades clave

- **Valoración**: opinión de un participante sobre el otro tras un préstamo completado. Atributos: autor, destinatario, préstamo asociado, puntuación, comentario opcional, rol del autor (prestamista/prestatario), visibilidad.
- **Reputación / Trust Score**: agregado público de un usuario. Atributos derivados: valoración media, número de valoraciones, préstamos completados, índice de respuesta, antigüedad (fecha de alta), estado de verificación.
- *(Reutiliza **Usuario** de EPIC 1 y **Préstamo** de EPIC 4.)*

## Criterios de éxito *(obligatorio)*

### Resultados medibles

- **SC-001**: Al menos el 60% de los préstamos completados reciben valoración de al menos una de las dos partes.
- **SC-002**: Al menos el 90% de los usuarios con 3 o más préstamos completados muestran una valoración media pública.
- **SC-003**: Los usuarios consultan la reputación de la otra parte antes de aceptar o solicitar en al menos el 50% de los casos (indicador de que la confianza mostrada se usa para decidir).

## Suposiciones

- Las cifras objetivo de los criterios de éxito son propuestas iniciales y deben validarse con negocio.
- La reputación es el **principal mecanismo de confianza** del MVP, dado que no hay pagos ni verificación de identidad.
- La valoración se dispara desde el cierre del préstamo definido en EPIC 4.
- El índice de respuesta se alimenta del comportamiento del usuario ante las solicitudes (EPIC 4); su fórmula concreta está pendiente de aclaración.
- La gestión de valoraciones reportadas como abusivas se trata en Administración (EPIC 7).
- Queda **fuera de alcance**: reputación importada de otras plataformas, insignias más allá del estado de verificación, y verificación de identidad (futuro).
