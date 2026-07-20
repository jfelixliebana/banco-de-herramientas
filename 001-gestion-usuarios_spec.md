# Especificación de Feature: Onboarding y gestión de cuenta de vecino

**Rama**: `001-gestion-usuarios`
**Creada**: 2026-07-15
**Actualizada**: Incorpora las decisiones funcionales aprobadas para el MVP y resuelve las aclaraciones funcionales de esta feature. 
**Estado**: Definitiva
**Input**: Descripción del usuario: "Banco Comunitario de Herramientas — MVP. EPIC 1 (Gestión de usuarios): registro con email y contraseña, verificación de email, inicio de sesión, recuperación de contraseña, perfil público y edición de perfil, como base de confianza de una plataforma de préstamo gratuito de herramientas entre vecinos cercanos."

> **Nota de prioridades**: el backlog original usa P0/P1/P2. Aquí se traduce al estándar de la skill: **P0→P1** (imprescindible), **P1→P2** (importante), **P2→P3** (deseable).

## Fuera de alcance

Esta feature NO incluye:

- Inicio de sesión mediante Google, Apple, Microsoft o cualquier otro proveedor externo de identidad.
- Inicio de sesión mediante número de teléfono.
- Autenticación multifactor (MFA).
- Verificación de identidad mediante DNI, biometría o servicios de terceros.
- Gestión administrativa de usuarios (suspender, bloquear, reactivar o eliminar cuentas), cubierta por la EPIC 7.
- Eliminación definitiva de cuentas de usuario.
- Gestión de roles distintos del usuario estándar.
- Gestión de sesiones simultáneas entre múltiples dispositivos.
- Integraciones con sistemas externos de autenticación o directorios corporativos.

## Escenarios de usuario y pruebas *(obligatorio)*

### Historia de Usuario 1 - Crear una cuenta con email y contraseña (Prioridad: P1)

Como vecino que quiere prestar o pedir prestadas herramientas, quiero darme de alta con mi email y una contraseña, para tener una identidad reconocible dentro de la comunidad y poder operar en la plataforma.

**Por qué esta prioridad**: sin cuenta no existe ninguna otra acción del producto (publicar, buscar, solicitar). Es el punto de entrada del que cuelga todo lo demás.

**Test independiente**: se puede probar dándose de alta con un email nuevo y comprobando que queda una cuenta creada en estado "sin verificar", sin depender de ninguna otra feature.

**Escenarios de aceptación**:
1. **Dado** que soy un visitante sin cuenta, **Cuando** introduzco un email válido no registrado, una contraseña que cumple los requisitos de seguridad y acepto los términos, **Entonces** se crea mi cuenta en estado "sin verificar" y se dispara el envío del email de verificación.
2. **Dado** que introduzco un email ya registrado, **Cuando** intento darme de alta, **Entonces** el sistema me informa de que ese email ya tiene cuenta y me ofrece iniciar sesión o recuperar la contraseña, sin revelar más datos de esa cuenta.
3. **Dado** que introduzco una contraseña de menos de 10 caracteres, **Cuando** intento continuar, **Entonces** el sistema rechaza el alta e indica la longitud mínima requerida.
4. **Dado** que introduzco una contraseña que coincide con una lista conocida de credenciales filtradas, **Cuando** intento continuar, **Entonces** el sistema rechaza el alta y me pide elegir una contraseña distinta.
5. **Dado** que no acepto los términos y la política de datos, **Cuando** intento continuar, **Entonces** el sistema no permite completar el alta.
6. **Dado** que mi cuenta está "sin verificar", **Cuando** intento publicar una herramienta o solicitar un préstamo, **Entonces** el sistema me lo impide y me indica que debo verificar mi email antes de continuar. *(DF-002: la verificación de email es obligatoria y bloqueante para estas dos acciones.)*

### Historia de Usuario 2 - Verificar mi email (Prioridad: P1)

Como vecino recién registrado, quiero confirmar que el email es mío, para que la comunidad sepa que detrás de la cuenta hay un contacto real, y para desbloquear la posibilidad de publicar y solicitar herramientas.

**Por qué esta prioridad**: la verificación de email es el mínimo mecanismo de confianza del MVP (la verificación de identidad queda fuera de alcance) y, según DF-002, es requisito obligatorio para operar en la plataforma, no solo un distintivo. El estado de verificación aparece en el perfil público y sostiene el Trust Score.

**Test independiente**: se puede probar registrando una cuenta, comprobando que publicar/solicitar está bloqueado, usando el enlace/código de verificación y comprobando que la cuenta pasa a estado "email verificado" y esas acciones quedan desbloqueadas.

**Escenarios de aceptación**:
1. **Dado** que tengo una cuenta "sin verificar", **Cuando** uso el enlace o código de verificación recibido por email antes de que caduque, **Entonces** mi cuenta pasa a "email verificado", mi perfil muestra ese estado, y quedo habilitado para publicar herramientas y solicitar préstamos.
2. **Dado** que el enlace o código ha caducado, **Cuando** intento verificar, **Entonces** el sistema me lo indica y me permite solicitar uno nuevo.
3. **Dado** que no he recibido el email, **Cuando** solicito reenviarlo, **Entonces** el sistema envía un nuevo email de verificación al mismo correo, respetando un límite de reenvíos.
4. **Dado** que mi cuenta ya está en estado "email verificado", **Cuando** utilizo de nuevo un enlace o código de verificación ya válido o previamente utilizado, **Entonces** el sistema me informa de que la cuenta ya se encuentra verificada y no realiza ningún cambio sobre su estado.

### Historia de Usuario 3 - Iniciar sesión (Prioridad: P1)

Como vecino con cuenta, quiero iniciar sesión con mi email y contraseña, para acceder a mis herramientas, solicitudes y mensajes.

**Por qué esta prioridad**: es la puerta de acceso recurrente al producto; sin ella la cuenta no sirve para nada tras el alta.

**Test independiente**: se puede probar con una cuenta existente, comprobando acceso con credenciales correctas y bloqueo con incorrectas.

**Escenarios de aceptación**:
1. **Dado** que tengo una cuenta, **Cuando** introduzco email y contraseña correctos, **Entonces** accedo a la aplicación autenticado, esté o no verificado mi email.
2. **Dado** que introduzco credenciales incorrectas, **Cuando** intento iniciar sesión, **Entonces** el sistema deniega el acceso con un mensaje genérico que no revela si el fallo está en el email o en la contraseña.
3. **Dado** que he fallado 3 intentos de inicio de sesión consecutivos sobre la misma cuenta, **Cuando** vuelvo a intentarlo, **Entonces** el sistema exige superar un CAPTCHA antes de procesar el siguiente intento.
4. **Dado** que he fallado 8 intentos de inicio de sesión consecutivos sobre la misma cuenta, **Cuando** vuelvo a intentarlo, **Entonces** el sistema bloquea temporalmente los intentos sobre esa cuenta durante un periodo determinado, informando de cuándo podré volver a intentarlo.

### Historia de Usuario 4 - Recuperar mi contraseña (Prioridad: P1)

Como vecino que ha olvidado su contraseña, quiero restablecerla desde mi email, para recuperar el acceso a mi cuenta sin perder mi historial ni mi reputación.

**Por qué esta prioridad**: sin recuperación, cualquier olvido convierte la cuenta en irrecuperable y expulsa al usuario del producto.

**Test independiente**: se puede probar solicitando el restablecimiento sobre una cuenta existente y fijando una contraseña nueva con la que luego se inicia sesión.

**Escenarios de aceptación**:
1. **Dado** que tengo una cuenta y he olvidado la contraseña, **Cuando** solicito el restablecimiento con mi email, **Entonces** recibo un enlace o código temporal para fijar una contraseña nueva.
2. **Dado** que introduzco un email no registrado, **Cuando** solicito el restablecimiento, **Entonces** el sistema muestra un mensaje neutro (sin confirmar ni negar la existencia de la cuenta) para no filtrar qué correos están dados de alta.
3. **Dado** que uso un enlace de restablecimiento caducado o ya utilizado, **Cuando** intento fijar la contraseña, **Entonces** el sistema lo rechaza y me permite pedir uno nuevo.
4. **Dado** que solicito repetidamente el restablecimiento de contraseña en un corto intervalo de tiempo, **Cuando** supero el límite permitido de solicitudes, **Entonces** el sistema limita temporalmente nuevas solicitudes e informa de que debo esperar antes de volver a intentarlo, sin revelar información adicional sobre la cuenta.
5. **Dado** que fijo una contraseña nueva al restablecer, **Cuando** guardo, **Entonces** esa contraseña debe cumplir la misma política mínima de seguridad que rige en el alta (FR-003).

### Historia de Usuario 5 - Consultar el perfil público de un usuario (Prioridad: P1)

Como vecino que va a prestar o a pedir una herramienta, quiero ver el perfil público de la otra persona, para decidir si me fío antes de acordar un préstamo.

**Por qué esta prioridad**: la confianza es el eje del modelo. Poder mirar el perfil (reputación, verificación, actividad) es lo que hace que un desconocido acepte prestar o entregar una herramienta.

**Test independiente**: se puede probar abriendo el perfil público de otra cuenta y comprobando que se muestran los datos públicos y ninguno privado.

**Escenarios de aceptación**:
1. **Dado** que estoy autenticado, **Cuando** abro el perfil público de otro usuario, **Entonces** veo su nombre, fotografía, barrio o zona aproximada, fecha de alta, número de préstamos realizados, número de herramientas publicadas, índice de respuesta, valoración media y estado de verificación. *(Lista base según DF-012; el índice de respuesta se añade desde el backlog original — ver Suposiciones.)*
2. **Dado** que consulto un perfil público, **Cuando** se muestra, **Entonces** en ningún caso aparecen datos privados: email, contraseña, dirección o coordenadas exactas, historial de acceso, ni teléfono salvo que exista un préstamo aceptado con esa persona. *(DF-012.)*
3. **Dado** que consulto el perfil de un usuario recién registrado sin actividad, **Cuando** se muestra, **Entonces** los indicadores sin datos (p. ej. valoración media) aparecen como "sin valoraciones aún" en lugar de un valor engañoso.

### Historia de Usuario 6 - Editar mi perfil (Prioridad: P2)

Como vecino con cuenta, quiero editar mi nombre, mi foto y mi ubicación aproximada, para mantener mi perfil actualizado y creíble ante el resto de la comunidad.

**Por qué esta prioridad**: mejora la calidad de la confianza y de la proximidad, pero el producto puede operar en su primera versión con los datos capturados en el alta; por eso es importante, no imprescindible.

**Test independiente**: se puede probar editando los campos del perfil propio y comprobando que los cambios se reflejan en el perfil público, y que las herramientas ya publicadas heredan la nueva ubicación.

**Escenarios de aceptación**:
1. **Dado** que estoy autenticado, **Cuando** cambio mi nombre o mi foto y guardo, **Entonces** los cambios quedan reflejados en mi perfil público.
2. **Dado** que introduzco un dato inválido (p. ej. una imagen en formato no admitido), **Cuando** intento guardar, **Entonces** el sistema rechaza el cambio e indica el motivo, conservando los valores anteriores.
3. **Dado** que edito mi ubicación aproximada y guardo, **Cuando** el cambio se confirma, **Entonces** todas las herramientas que tengo publicadas adoptan automáticamente la nueva ubicación aproximada como referencia de proximidad, y el sistema me muestra un aviso informándome de que mis publicaciones han cambiado de zona de búsqueda.
4. **Dado** que estoy editando mi ubicación aproximada, **Cuando** el sistema tiene acceso a la ubicación puntual de mi dispositivo, **Entonces** me sugiere automáticamente un barrio o zona aproximada, y puedo confirmarla o corregirla manualmente antes de guardar.
5. **Dado** que el sistema no tiene acceso a la ubicación del dispositivo o la rechazo, **Cuando** edito mi ubicación, **Entonces** puedo introducirla manualmente sobre un mapa o buscador de zonas. 
6. **Dado** que solicito cambiar mi dirección de correo electrónico, **Cuando** confirmo el nuevo correo mediante el enlace de verificación, **Entonces** el sistema sustituye el email asociado a la cuenta, mantiene el estado "email verificado", conserva todo el historial de la cuenta y envía una notificación informativa al correo anterior.

### Casos límite

- ¿Qué pasa si alguien intenta registrarse con un email con formato válido pero inexistente? (El alta se crea pero nunca llega el email de verificación; la cuenta queda "sin verificar" indefinidamente y, por tanto, sin poder publicar ni solicitar — DF-002.)
- ¿Qué ocurre si un usuario "sin verificar" intenta publicar o solicitar una herramienta? **Resuelto por DF-002**: el sistema lo bloquea explícitamente hasta que verifique su email.
- ¿Qué ocurre si el usuario intenta cambiar su email por otro que ya está asociado a una cuenta existente?
- ¿Qué se muestra en el perfil público de una cuenta que ha sido suspendida o dada de baja por administración? (Ver relación con EPIC 7.)
- ¿Qué pasa si el usuario rechaza el permiso de ubicación del dispositivo tanto en el alta como al editar el perfil? (Cubierto por FR-014b: siempre queda la vía manual como alternativa.) 

## Requisitos *(obligatorio)*

### Requisitos funcionales

- **FR-001**: El sistema DEBE permitir crear una cuenta a partir de un email único y una contraseña.
- **FR-002**: El sistema DEBE rechazar el alta con un email ya registrado sin revelar datos de la cuenta existente.
- **FR-003**: El sistema DEBE exigir una contraseña de al menos 10 caracteres, sin imponer una combinación obligatoria de mayúsculas, minúsculas, números y símbolos, y DEBE rechazar contraseñas presentes en listas conocidas de credenciales filtradas.
- **FR-004**: El sistema DEBE almacenar la contraseña de forma que nunca sea recuperable ni visible en claro.
- **FR-005**: El sistema DEBE requerir la aceptación de los términos y de la política de tratamiento de datos antes de completar el alta.
- **FR-006**: El sistema DEBE enviar un mecanismo de verificación de email tras el registro y permitir reenviarlo con un límite de frecuencia.
- **FR-007**: El sistema DEBE registrar el estado de verificación de cada cuenta ("sin verificar", "email verificado", "identidad verificada"—este último reservado para el futuro).
- **FR-008**: El sistema DEBE permitir iniciar sesión con email y contraseña y denegar el acceso con credenciales incorrectas mediante un mensaje que no distinga cuál de los dos campos ha fallado.
- **FR-009**: El sistema DEBE aplicar mecanismos progresivos de protección frente a intentos automatizados de autenticación, incluyendo desafíos adicionales y bloqueo temporal cuando se superen los umbrales definidos por la política de seguridad.
- **FR-010**: El sistema DEBE permitir restablecer la contraseña mediante un mecanismo temporal enviado al email, de un solo uso y con caducidad.
- **FR-010b**: El sistema DEBE limitar la frecuencia de solicitudes de recuperación de contraseña para evitar abusos y usos automatizados.
- **FR-011**: El sistema DEBE mostrar en el perfil público los indicadores disponibles (número de préstamos realizados, número de herramientas publicadas, índice de respuesta y valoración media), calculados por las features responsables de dichos indicadores.
- **FR-012**: El sistema DEBE mantener fuera de cualquier vista pública la siguiente información privada: email, contraseña, dirección exacta, coordenadas exactas e historial de acceso. *(DF-012.)*
- **FR-012b**: El sistema DEBE permitir registrar un número de teléfono opcional en el perfil, tratado como información privada por defecto; su exposición condicionada a la existencia de un préstamo aceptado se especifica en las features de Solicitudes (EPIC 4) y Comunicación (EPIC 5). *(DF-012 — este requisito solo cubre el almacenamiento y el carácter privado por defecto; la lógica de cuándo compartirlo no es de esta spec.)*
- **FR-013**: El sistema DEBE permitir a cada usuario editar su nombre, su fotografía y su ubicación aproximada.
- **FR-014**: El sistema DEBE representar la ubicación del usuario de forma "aproximada" de cara a terceros, sin exponer su posición exacta, sin geolocalización en tiempo real y sin que la dirección exacta sea nunca pública. *(DF-001 — restricciones ya fijadas en origen; el mecanismo de captura se especifica en FR-014b.)*
- **FR-014b**: El sistema DEBE, tanto en el alta como en la edición de perfil, sugerir un barrio o zona aproximada a partir de una captura puntual de la ubicación del dispositivo (si el usuario la concede), permitiendo siempre confirmarla, corregirla o introducirla manualmente sobre un mapa o buscador de zonas.
- **FR-014c**: El sistema DEBE aplicar de forma retroactiva un cambio de ubicación aproximada del usuario a todas sus herramientas ya publicadas, notificando al usuario del cambio de zona de búsqueda resultante.
- **FR-015**: El sistema DEBE bloquear la publicación de herramientas y la solicitud de préstamos a cualquier usuario cuya cuenta esté en estado "sin verificar". *(Resuelto por DF-002 — la verificación de email es obligatoria y bloqueante, no solo un distintivo de confianza.)*
- **FR-016**: El sistema DEBE permitir cambiar el email asociado a una cuenta. El cambio solo se aplicará una vez confirmado el nuevo correo electrónico. Durante el proceso la cuenta permanecerá plenamente operativa y conservará su estado de verificación. Tras completar el cambio el sistema enviará una notificación informativa al correo electrónico anterior. Si el nuevo correo ya pertenece a otra cuenta, el sistema rechazará el cambio sin revelar información adicional.

### Requisitos no funcionales

- **NFR-001**: El sistema DEBE cumplir la normativa vigente en materia de protección de datos personales aplicable al ámbito de operación del producto (incluyendo RGPD cuando corresponda).
- **NFR-002**: El sistema DEBE garantizar que el proceso de restablecimiento de contraseña no provoque la pérdida del historial de actividad, la reputación, las herramientas publicadas ni los préstamos asociados a la cuenta.
- **NFR-003**: El sistema DEBE impedir que el estado de verificación de una cuenta retroceda de "email verificado" a "sin verificar" de forma automática; solo una acción administrativa explícita (fuera de alcance de esta feature — ver EPIC 7) podrá revertirlo.

### Entidades clave

- **Usuario**: persona dada de alta en la plataforma. Contiene información privada (email principal, email pendiente de confirmación —si existe un cambio de email en curso—, contraseña protegida, dirección o coordenadas exactas, historial de acceso y teléfono opcional) e información pública (nombre, fotografía, barrio o zona aproximada, fecha de alta, estado de verificación), además de indicadores derivados de su actividad (número de préstamos realizados, número de herramientas publicadas, índice de respuesta y valoración media). La **ubicación aproximada** es un atributo del **Usuario**, no de cada **Herramienta**. Por tanto, cualquier cambio de ubicación se refleja automáticamente en todas las herramientas publicadas por ese usuario. El **email principal** identifica de forma única la cuenta. Si el usuario solicita un cambio de email, el nuevo correo permanecerá como **email pendiente de confirmación** hasta completar el proceso de verificación. Solo entonces sustituirá al email principal, manteniendo el historial, el estado de verificación y el resto de atributos de la cuenta.
- **Estado de verificación**: nivel de confianza asociado a un usuario — "sin verificar" (bloquea publicar/solicitar, DF-002), "email verificado" (habilita publicar/solicitar), "identidad verificada" (futuro).
- **Solicitud de verificación de email**: relación temporal entre un usuario y un mecanismo de confirmación con caducidad y límite de reenvíos.
- **Solicitud de restablecimiento de contraseña**: mecanismo temporal, de un solo uso y con caducidad, asociado a un usuario.

## Matriz de trazabilidad

La siguiente matriz muestra qué requisitos funcionales quedan cubiertos por cada historia de usuario de esta feature. Los NFR no se incluyen porque son transversales a toda la feature, no atribuibles a una única historia.

| Historia de Usuario | Requisitos funcionales relacionados |
|----------------------|-------------------------------------|
| **HU-001 – Crear una cuenta** | FR-001, FR-002, FR-003, FR-004, FR-005, FR-006, FR-015 |
| **HU-002 – Verificar email** | FR-006, FR-007, FR-015 |
| **HU-003 – Iniciar sesión** | FR-008, FR-009 |
| **HU-004 – Recuperar contraseña** | FR-010, FR-010b, FR-003 |
| **HU-005 – Consultar perfil público** | FR-011, FR-012, FR-012b |
| **HU-006 – Editar perfil** | FR-013, FR-014, FR-014b, FR-014c, FR-016 |

### Cobertura

Todos los requisitos funcionales definidos en esta feature deberán quedar implementados por, al menos, una historia de usuario.

Ninguna historia de usuario deberá introducir comportamiento funcional que no esté respaldado por uno o más requisitos funcionales.

## Criterios de éxito *(obligatorio)*

### Resultados medibles

- **SC-001**: Al menos el 80% de las personas que inician el registro completan el alta.
- **SC-002**: Al menos el 70% de las cuentas creadas verifican su email en las primeras 48 horas. 
- **SC-003**: Menos del 5% de los usuarios activos contacta con soporte por no poder acceder a su cuenta en un periodo de 30 días. 
- **SC-004**: Al menos el 60% de los usuarios registrados completan su perfil con foto y ubicación aproximada en su primera semana.

## Suposiciones

- Las cifras objetivo de los criterios de éxito son propuestas iniciales y deben validarse con negocio.
- La confianza en el MVP se apoya en la verificación de email; la verificación de identidad queda **fuera de alcance** (futuro).
- No hay pagos, ni login social, ni autenticación de terceros en el MVP; el registro es exclusivamente por email y contraseña.
- El índice de respuesta y la valoración media que se muestran en el perfil se **calculan** en las features de Solicitudes (EPIC 4) y Reputación (EPIC 6); esta feature solo los expone. El índice de respuesta no aparece de forma explícita en la lista pública de DF-012, así que se mantiene aquí como suposición heredada del backlog original, no como algo ya cerrado por el documento de aclaraciones.
- El teléfono es un dato de perfil opcional, privado por defecto; la regla de cuándo se comparte (préstamo aceptado) pertenece a EPIC 4/5, no a esta spec, que solo asume su existencia y su carácter privado por defecto (DF-012).
- Queda **fuera de alcance** de esta feature: gestión administrativa de cuentas (suspender, banear), cubierta en Administración (EPIC 7).
- El mecanismo de ubicación (FR-014b) asume que la app puede solicitar permiso de geolocalización puntual al dispositivo; si el proyecto decide no depender de ese permiso, esta suposición y FR-014b deberían revisarse.
- Los umbrales de intentos (3 y 8) y la duración del bloqueo en FR-009 son valores de partida sugeridos, no cifras validadas con negocio o seguridad.
- La política de contraseña de FR-003 asume disponibilidad de un servicio o lista de credenciales filtradas contra la que comprobar; si no se dispone de uno, se propone retirar esa cláusula y dejar solo la longitud mínima.
- El cambio retroactivo de ubicación (FR-014c) asume que "ubicación" es un atributo único por Usuario y no por Herramienta; si en el futuro se necesitara que un mismo usuario publicara herramientas "ubicadas" en más de una zona (p. ej. una segunda residencia), este modelo tendría que revisarse.
