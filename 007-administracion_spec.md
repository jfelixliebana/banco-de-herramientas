# Especificación de Feature: Administración y moderación de la plataforma

**Rama**: `007-administracion`
**Creada**: 2026-07-15
**Estado**: Borrador
**Input**: Descripción del usuario: "Banco Comunitario de Herramientas — MVP. EPIC 7 (Administración): que un rol administrador de la plataforma pueda gestionar usuarios (suspender, dar de baja), registrar y gestionar incidencias reportadas por los vecinos, y moderar contenido inapropiado (herramientas, mensajes, valoraciones). Es el mínimo control operativo que necesita una plataforma basada en confianza que no interviene en los préstamos."

> **Nota de prioridades**: el backlog original usa P0/P1/P2. Aquí se traduce al estándar de la skill: **P0→P1** (imprescindible), **P1→P2** (importante), **P2→P3** (deseable). Las tres historias de esta épica son P2 en el backlog → **P3 (deseable)** aquí.

> **Aviso de alcance**: en el MVP la plataforma **no interviene en incidencias ni daños durante el préstamo** ni garantiza el estado de las herramientas. Esta feature cubre el control operativo mínimo (moderar contenido, atender reportes, actuar sobre cuentas problemáticas), **no** la resolución de disputas entre vecinos. Conviene confirmar el alcance real que se quiere en el MVP frente a lo que se pospone.

## Escenarios de usuario y pruebas *(obligatorio)*

### Historia de Usuario 1 - Moderar contenido inapropiado (Prioridad: P3)

Como administrador de la plataforma, quiero revisar y retirar contenido inapropiado (herramientas, mensajes o valoraciones), para mantener un entorno seguro y de confianza en la comunidad.

**Por qué esta prioridad**: protege la confianza del conjunto, pero el MVP puede lanzarse en comunidades pequeñas y controladas donde la moderación sea puntual; por eso es deseable y no imprescindible desde el día uno.

**Test independiente**: se puede probar marcando un contenido como inapropiado y comprobando que el administrador puede retirarlo y que deja de ser visible.

**Escenarios de aceptación**:
1. **Dado** que soy administrador, **Cuando** reviso un contenido reportado o detectado como inapropiado, **Entonces** puedo retirarlo y deja de mostrarse al resto de usuarios.
2. **Dado** que retiro un contenido, **Cuando** se aplica, **Entonces** queda registrada la acción y su motivo, y se informa al autor según la política definida. [NECESITA ACLARACIÓN: ¿se notifica al autor y con qué detalle?]
3. **Dado** que un contenido reportado no incumple las normas, **Cuando** lo reviso, **Entonces** puedo descartarlo y dejarlo publicado, registrando la decisión.

### Historia de Usuario 2 - Registrar y gestionar incidencias (Prioridad: P3)

Como administrador, quiero registrar y hacer seguimiento de las incidencias que reportan los vecinos, para tener trazabilidad de los problemas aunque la plataforma no medie en los préstamos.

**Por qué esta prioridad**: da visibilidad operativa y aprendizaje sobre qué falla, pero no es imprescindible para que ocurra un préstamo; por eso es deseable.

**Test independiente**: se puede probar creando una incidencia a partir de un reporte y comprobando que puede seguirse hasta su cierre.

**Escenarios de aceptación**:
1. **Dado** que un usuario reporta un problema (con otro usuario, una herramienta o un préstamo), **Cuando** el administrador lo recibe, **Entonces** se crea una incidencia con su contexto y estado "abierta".
2. **Dado** que trabajo una incidencia, **Cuando** la actualizo, **Entonces** puedo cambiar su estado (p. ej. en revisión, resuelta, descartada) y dejar constancia de las acciones tomadas.
3. **Dado** que la incidencia implica daño o pérdida de una herramienta durante el préstamo, **Cuando** la gestiono, **Entonces** el sistema refleja que la plataforma **no resuelve la disputa** en el MVP y se limita a registrar y, si procede, actuar sobre las cuentas. [NECESITA ACLARACIÓN: ¿qué acciones se consideran dentro de alcance ante una incidencia de daño?]

### Historia de Usuario 3 - Gestionar usuarios (Prioridad: P3)

Como administrador, quiero poder suspender o dar de baja a usuarios que incumplen las normas, para proteger a la comunidad de comportamientos abusivos o fraudulentos.

**Por qué esta prioridad**: es la palanca última de seguridad, pero en una comunidad inicial pequeña puede gestionarse de forma manual; por eso es deseable y no bloqueante para el lanzamiento.

**Test independiente**: se puede probar suspendiendo una cuenta y comprobando que deja de poder operar mientras dura la suspensión.

**Escenarios de aceptación**:
1. **Dado** que un usuario incumple las normas, **Cuando** el administrador lo suspende, **Entonces** ese usuario no puede publicar, solicitar ni valorar mientras dure la suspensión, y se registra el motivo.
2. **Dado** que una cuenta se da de baja, **Cuando** se aplica, **Entonces** sus herramientas dejan de estar disponibles y su perfil deja de ser operativo, preservando la integridad de los historiales de préstamo de terceros. [NECESITA ACLARACIÓN: ¿qué se conserva y qué se anonimiza de una cuenta dada de baja, conforme a protección de datos?]
3. **Dado** que un usuario suspendido tiene préstamos en curso, **Cuando** se le suspende, **Entonces** el sistema aplica la regla definida para esos préstamos abiertos. [NECESITA ACLARACIÓN: ¿se congelan, se permite solo cerrarlos, o se cancelan?]

### Casos límite

- ¿Quién puede ser administrador y cómo se le asigna ese rol? [NECESITA ACLARACIÓN: gestión de roles administrativos no descrita en el backlog]
- ¿Cómo reporta un usuario normal un contenido, una incidencia o a otro usuario? (¿Existe un flujo de "reportar" en el MVP, o los reportes llegan por fuera de la app?) [NECESITA ACLARACIÓN]
- ¿Qué pasa con los datos personales de una cuenta eliminada? (Debe cumplir la normativa de protección de datos.)
- ¿Puede un usuario suspendido volver a registrarse con otro email?
- ¿Se avisa a la contraparte de un préstamo cuando el otro usuario es suspendido o dado de baja?
- ¿Hay un registro de auditoría de las acciones de administración?

## Requisitos *(obligatorio)*

### Requisitos funcionales

- **FR-001**: El sistema DEBE ofrecer un rol de administrador con permisos diferenciados del de un usuario normal.
- **FR-002**: El sistema DEBE permitir al administrador retirar contenido inapropiado (herramientas, mensajes, valoraciones) de modo que deje de ser visible.
- **FR-003**: El sistema DEBE registrar cada acción de moderación con su motivo y su autor.
- **FR-004**: El sistema DEBE permitir crear, actualizar el estado y cerrar incidencias asociadas a usuarios, herramientas o préstamos.
- **FR-005**: El sistema DEBE permitir al administrador suspender temporalmente a un usuario, impidiéndole operar mientras dure la suspensión.
- **FR-006**: El sistema DEBE permitir dar de baja una cuenta preservando la integridad de los historiales de préstamo de terceros.
- **FR-007**: El sistema DEBE tratar los datos de las cuentas dadas de baja conforme a la normativa de protección de datos aplicable. [NECESITA ACLARACIÓN: política de retención/anonimización]
- **FR-008**: El sistema DEBE definir cómo los usuarios reportan contenido, incidencias u otros usuarios. [NECESITA ACLARACIÓN: ¿existe flujo de reporte in-app en el MVP?]
- **FR-009**: El sistema DEBE definir la regla aplicable a los préstamos en curso cuando una de las partes es suspendida o dada de baja. [NECESITA ACLARACIÓN]
- **FR-010**: El sistema DEBE mantener un registro de auditoría de las acciones administrativas. [NECESITA ACLARACIÓN: ¿requerido en el MVP?]
- **FR-011**: El sistema DEBE dejar explícito que **no resuelve disputas ni daños** entre vecinos en el MVP; su actuación se limita a moderación y gestión de cuentas.

### Entidades clave

- **Administrador**: usuario con permisos elevados para moderar contenido y gestionar cuentas e incidencias.
- **Incidencia**: registro de un problema reportado o detectado, asociado a un usuario, herramienta o préstamo. Atributos: origen, contexto, estado (abierta, en revisión, resuelta, descartada), acciones tomadas.
- **Acción de moderación**: registro de una intervención sobre contenido o cuentas. Atributos: administrador, objeto afectado, tipo de acción, motivo, momento.
- **Estado de la cuenta**: situación operativa de un usuario — activa, suspendida, dada de baja.
- *(Reutiliza **Usuario** de EPIC 1, **Herramienta** de EPIC 2, **Préstamo** de EPIC 4 y **Valoración**/**Mensaje** de EPIC 5–6.)*

## Criterios de éxito *(obligatorio)*

### Resultados medibles

- **SC-001**: El 100% del contenido reportado como inapropiado se revisa en un plazo definido (p. ej. 72 horas). [NECESITA ACLARACIÓN: plazo objetivo]
- **SC-002**: Menos del 2% de los usuarios activos son objeto de una acción de suspensión o baja en un periodo de 90 días (indicador de salud de la comunidad).
- **SC-003**: El 100% de las bajas de cuenta se completan cumpliendo la política de protección de datos, sin incidencias de cumplimiento.

## Suposiciones

- Las cifras objetivo de los criterios de éxito son propuestas iniciales y deben validarse con negocio.
- El MVP **no incluye resolución de disputas ni cobertura de daños**; esta feature aporta solo control operativo mínimo.
- El alcance real de esta épica en el MVP está **por confirmar**: dado que sus tres historias son las de menor prioridad (P3), es candidata a posponerse o reducirse a lo imprescindible para lanzar en comunidades pequeñas.
- La gestión de roles administrativos, el flujo de reporte in-app y las políticas de retención de datos no están detallados en el backlog y quedan marcados como pendientes de aclaración.
- El cumplimiento se rige por la normativa aplicable de protección de datos, consumo y responsabilidad civil citada en la inception.
