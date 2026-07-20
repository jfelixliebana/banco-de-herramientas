# Especificación de Feature: Comunicación entre vecinos (chat y notificaciones)

**Rama**: `005-comunicacion`
**Creada**: 2026-07-15
**Estado**: Borrador
**Input**: Descripción del usuario: "Banco Comunitario de Herramientas — MVP. EPIC 5 (Comunicación): que prestamista y prestatario puedan escribirse dentro de la app para acordar la entrega y la devolución, y que ambos reciban notificaciones de los eventos importantes de sus solicitudes y préstamos."

> **Nota de prioridades**: el backlog original usa P0/P1/P2. Aquí se traduce al estándar de la skill: **P0→P1** (imprescindible), **P1→P2** (importante), **P2→P3** (deseable).

## Escenarios de usuario y pruebas *(obligatorio)*

### Historia de Usuario 1 - Recibir notificaciones de mis solicitudes y préstamos (Prioridad: P2)

Como usuario con solicitudes o préstamos en marcha, quiero recibir avisos de los eventos importantes (nueva solicitud, aceptación, rechazo, entrega, devolución, nuevo mensaje), para reaccionar a tiempo y no dejar a nadie esperando.

**Por qué esta prioridad**: sostiene el índice de respuesta y evita que las solicitudes mueran por desatención. Es la más importante de esta épica porque el ciclo de préstamo depende de que la gente se entere de lo que pasa; aun así, el backlog la sitúa como importante (no imprescindible) porque el flujo puede completarse consultando manualmente.

**Test independiente**: se puede probar disparando un evento (p. ej. una nueva solicitud) y comprobando que el usuario destinatario recibe la notificación correspondiente.

**Escenarios de aceptación**:
1. **Dado** que soy propietario, **Cuando** recibo una nueva solicitud sobre una herramienta mía, **Entonces** recibo una notificación que me lleva a esa solicitud.
2. **Dado** que soy prestatario, **Cuando** el propietario acepta o rechaza mi solicitud, **Entonces** recibo una notificación con el resultado.
3. **Dado** que participo en un préstamo, **Cuando** se confirma la entrega o la devolución, **Entonces** recibo la notificación correspondiente.
4. **Dado** que recibo un mensaje de la otra parte, **Cuando** llega, **Entonces** recibo un aviso que me lleva a la conversación.
5. **Dado** que tengo varias notificaciones, **Cuando** abro el centro de notificaciones, **Entonces** las veo listadas y puedo distinguir las no leídas de las leídas.

### Historia de Usuario 2 - Chatear con la otra parte de un préstamo (Prioridad: P2)

Como parte de una solicitud o préstamo, quiero escribirme con la otra persona dentro de la app, para acordar el punto y la hora de entrega y de devolución sin salir de la plataforma ni intercambiar datos personales antes de tiempo.

**Por qué esta prioridad**: facilita el acuerdo de entrega que la plataforma "solo facilita", y protege la privacidad al no obligar a compartir teléfono o email. Importante para una buena experiencia, pero un primer préstamo podría cerrarse por otros medios; por eso no es imprescindible.

**Test independiente**: se puede probar abriendo la conversación asociada a una solicitud y comprobando que ambos participantes pueden enviar y leer mensajes.

**Escenarios de aceptación**:
1. **Dado** que existe una solicitud entre dos usuarios, **Cuando** abro su conversación y envío un mensaje, **Entonces** la otra parte lo recibe y puede responder.
2. **Dado** que la conversación está asociada a una solicitud o préstamo concreto, **Cuando** la abro, **Entonces** veo el contexto (qué herramienta y en qué estado) junto a los mensajes.
3. **Dado** que intento escribir a un usuario con el que no tengo ninguna solicitud ni préstamo, **Cuando** lo intento, **Entonces** el sistema no lo permite (el chat existe solo en el marco de un préstamo). [NECESITA ACLARACIÓN: ¿el chat se habilita desde que se envía la solicitud o solo tras aceptarla?]
4. **Dado** que la solicitud fue rechazada o cancelada, **Cuando** intento seguir escribiendo, **Entonces** el sistema aplica la regla de cierre de conversación definida. [NECESITA ACLARACIÓN: ¿la conversación se cierra, se archiva en solo lectura o sigue abierta?]

### Casos límite

- ¿Qué pasa con las conversaciones cuando el préstamo se completa? ¿Siguen accesibles en solo lectura?
- ¿Qué ocurre si un usuario elimina su cuenta? ¿Qué se ve en las conversaciones que mantenía?
- ¿Cómo se maneja un usuario que envía mensajes abusivos o spam? (Enlaza con moderación — EPIC 7.)
- ¿Se permite adjuntar imágenes en el chat o solo texto? [NECESITA ACLARACIÓN]
- ¿Qué pasa si el destinatario tiene las notificaciones desactivadas? [NECESITA ACLARACIÓN: ¿el usuario puede configurar qué notificaciones recibe y por qué canal?]
- ¿Hay notificaciones fuera de la app? [NECESITA ACLARACIÓN: el canal de entrega (dentro de la app, email, push) no está definido en el backlog; describir el "qué" sin fijar el "cómo".]

## Requisitos *(obligatorio)*

### Requisitos funcionales

- **FR-001**: El sistema DEBE notificar al propietario cuando recibe una nueva solicitud sobre una de sus herramientas.
- **FR-002**: El sistema DEBE notificar al prestatario cuando su solicitud es aceptada, rechazada o cancelada.
- **FR-003**: El sistema DEBE notificar a ambas partes cuando se confirma la entrega o la devolución de un préstamo.
- **FR-004**: El sistema DEBE notificar a un usuario cuando recibe un nuevo mensaje en una conversación suya.
- **FR-005**: El sistema DEBE ofrecer un centro de notificaciones donde el usuario vea sus avisos y distinga los no leídos.
- **FR-006**: El sistema DEBE permitir que los dos participantes de una solicitud o préstamo intercambien mensajes dentro de la plataforma.
- **FR-007**: El sistema DEBE asociar cada conversación a la solicitud o préstamo que la origina y mostrar ese contexto.
- **FR-008**: El sistema DEBE impedir iniciar una conversación entre usuarios que no comparten ninguna solicitud o préstamo.
- **FR-009**: El sistema DEBE definir el momento en que se habilita la conversación. [NECESITA ACLARACIÓN: ¿al enviar la solicitud o al aceptarla?]
- **FR-010**: El sistema DEBE definir el ciclo de vida de una conversación cuando su solicitud/préstamo se cierra. [NECESITA ACLARACIÓN: cierre, archivo en solo lectura o continuidad]
- **FR-011**: El sistema DEBE definir los canales de entrega de las notificaciones y si el usuario puede configurarlos. [NECESITA ACLARACIÓN: dentro de la app / email / push]
- **FR-012**: El sistema DEBE permitir que el intercambio de mensajes ocurra sin obligar a compartir datos de contacto personales (email, teléfono).

### Entidades clave

- **Conversación**: hilo de mensajes entre los dos participantes de una solicitud o préstamo, ligado a ese contexto. Atributos: participantes, solicitud/préstamo asociado, estado (activa / cerrada / archivada).
- **Mensaje**: comunicación individual dentro de una conversación. Atributos: autor, contenido, momento, estado de lectura.
- **Notificación**: aviso dirigido a un usuario sobre un evento relevante (solicitud, cambio de estado, mensaje). Atributos: destinatario, tipo de evento, referencia al elemento relacionado, estado (leída / no leída).

## Criterios de éxito *(obligatorio)*

### Resultados medibles

- **SC-001**: Al menos el 80% de las solicitudes aceptadas generan al menos un intercambio de mensajes para acordar la entrega.
- **SC-002**: Al menos el 70% de las notificaciones de "nueva solicitud" derivan en que el propietario abra la solicitud en menos de 24 horas.
- **SC-003**: Menos del 5% de los préstamos completados reportan haber tenido que intercambiar datos de contacto fuera de la app por no poder comunicarse dentro.

## Suposiciones

- Las cifras objetivo de los criterios de éxito son propuestas iniciales y deben validarse con negocio.
- El chat existe **exclusivamente en el marco de una solicitud o préstamo**; el producto **no es una red social de vecinos** ni una mensajería general.
- La comunicación busca reforzar la privacidad: las partes no necesitan compartir teléfono ni email para coordinarse.
- El "qué" de las notificaciones se define aquí; el "cómo" (canal técnico) queda como decisión posterior fuera de la spec.
- La moderación de mensajes abusivos y el bloqueo de usuarios se tratan en Administración (EPIC 7).
