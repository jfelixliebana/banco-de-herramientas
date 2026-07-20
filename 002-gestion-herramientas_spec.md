# Especificación de Feature: Publicación y gestión de herramientas

**Rama**: `002-gestion-herramientas`
**Creada**: 2026-07-15
**Estado**: Borrador
**Input**: Descripción del usuario: "Banco Comunitario de Herramientas — MVP. EPIC 2 (Gestión de herramientas): que un vecino pueda publicar una herramienta que está dispuesto a prestar, editarla, eliminarla, cambiar su disponibilidad y ver el listado de las suyas. Cada herramienta define su propio radio máximo de préstamo (1–50 km)."

> **Nota de prioridades**: el backlog original usa P0/P1/P2. Aquí se traduce al estándar de la skill: **P0→P1** (imprescindible), **P1→P2** (importante), **P2→P3** (deseable).

## Escenarios de usuario y pruebas *(obligatorio)*

### Historia de Usuario 1 - Publicar una herramienta para prestar (Prioridad: P1)

Como vecino que tiene herramientas paradas en casa, quiero publicarlas indicando qué son y en qué condiciones las presto, para que otros vecinos cercanos puedan encontrarlas y pedírmelas.

**Por qué esta prioridad**: sin oferta publicada no hay nada que buscar ni que solicitar. Es la mitad del marketplace (el lado de la oferta) sin la cual el producto no arranca.

**Test independiente**: se puede probar publicando una herramienta con sus datos y comprobando que queda registrada, visible como propia y marcada como disponible.

**Escenarios de aceptación**:
1. **Dado** que soy un usuario autenticado, **Cuando** publico una herramienta con nombre, categoría, descripción, al menos una fotografía, estado de conservación y radio máximo dentro de 1–50 km, **Entonces** la herramienta queda creada, asociada a mí como propietario y marcada como "disponible".
2. **Dado** que dejo en blanco un dato obligatorio (p. ej. la categoría o la fotografía), **Cuando** intento publicar, **Entonces** el sistema rechaza la publicación e indica qué falta.
3. **Dado** que introduzco un radio máximo fuera del rango 1–50 km, **Cuando** intento publicar, **Entonces** el sistema lo rechaza e indica el rango permitido.
4. **Dado** que aún no tengo una ubicación aproximada definida en mi perfil, **Cuando** intento publicar, **Entonces** el sistema me pide fijarla primero, porque sin ella no puede aplicarse el criterio de proximidad. [NECESITA ACLARACIÓN: ¿la ubicación de referencia de una herramienta es siempre la del propietario, o se puede fijar por herramienta?]

### Historia de Usuario 2 - Cambiar la disponibilidad de una herramienta (Prioridad: P1)

Como propietario de una herramienta publicada, quiero marcarla como no disponible cuando la estoy usando o no la quiero prestar ahora, para no recibir solicitudes que no puedo atender.

**Por qué esta prioridad**: mantener el inventario realmente disponible es una de las preocupaciones centrales del producto ("¿qué nos quita el sueño?"). Sin este control, la confianza se erosiona con solicitudes imposibles.

**Test independiente**: se puede probar cambiando el estado de una herramienta propia entre "disponible" y "no disponible" y comprobando que se refleja de inmediato.

**Escenarios de aceptación**:
1. **Dado** que tengo una herramienta "disponible", **Cuando** la marco como "no disponible", **Entonces** deja de aparecer como prestable en las búsquedas de otros vecinos.
2. **Dado** que tengo una herramienta "no disponible", **Cuando** la marco como "disponible", **Entonces** vuelve a poder aparecer en las búsquedas.
3. **Dado** que una herramienta tiene una solicitud de préstamo en curso, **Cuando** intento cambiar su disponibilidad, **Entonces** el sistema me advierte del impacto sobre esa solicitud antes de aplicar el cambio. [NECESITA ACLARACIÓN: ¿marcar "no disponible" cancela solicitudes pendientes o solo evita nuevas?]

### Historia de Usuario 3 - Consultar mis herramientas publicadas (Prioridad: P1)

Como propietario, quiero ver de un vistazo todas las herramientas que he publicado y su estado, para gestionarlas con facilidad.

**Por qué esta prioridad**: es el panel desde el que el prestamista opera; sin él no puede administrar su oferta ni encontrar qué editar o retirar.

**Test independiente**: se puede probar listando las herramientas propias y comprobando que aparecen todas con su estado actual.

**Escenarios de aceptación**:
1. **Dado** que he publicado varias herramientas, **Cuando** abro mi listado, **Entonces** veo todas las mías con su nombre, fotografía, categoría y estado (disponible / no disponible).
2. **Dado** que no he publicado ninguna herramienta, **Cuando** abro mi listado, **Entonces** veo un estado vacío que me invita a publicar la primera.

### Historia de Usuario 4 - Editar una herramienta publicada (Prioridad: P1)

Como propietario, quiero corregir o mejorar los datos de una herramienta ya publicada (descripción, fotos, condiciones, radio), para mantener la información precisa y evitar malentendidos con quien la pida.

**Por qué esta prioridad**: la calidad del inventario depende de que la información sea fiel; sin edición, cualquier error obliga a borrar y volver a crear, perdiendo el historial de la herramienta.

**Test independiente**: se puede probar editando los campos de una herramienta propia y comprobando que los cambios se reflejan en su ficha.

**Escenarios de aceptación**:
1. **Dado** que soy el propietario de una herramienta, **Cuando** edito su descripción, fotos, condiciones de uso, duración máxima recomendada o radio y guardo, **Entonces** los cambios quedan reflejados en su ficha pública.
2. **Dado** que intento fijar un radio fuera de 1–50 km, **Cuando** guardo, **Entonces** el sistema rechaza el cambio e indica el rango permitido.
3. **Dado** que no soy el propietario de una herramienta, **Cuando** intento editarla, **Entonces** el sistema me lo impide.

### Historia de Usuario 5 - Eliminar una herramienta (Prioridad: P2)

Como propietario, quiero retirar una herramienta que ya no quiero prestar, para que deje de aparecer y no reciba solicitudes sobre ella.

**Por qué esta prioridad**: útil para mantener limpio el inventario, pero el producto funciona en su primera versión con marcar "no disponible"; por eso es importante, no imprescindible.

**Test independiente**: se puede probar eliminando una herramienta propia sin solicitudes activas y comprobando que desaparece de las búsquedas y de mi listado.

**Escenarios de aceptación**:
1. **Dado** que tengo una herramienta sin solicitudes en curso, **Cuando** la elimino y confirmo, **Entonces** deja de aparecer en las búsquedas y en mi listado.
2. **Dado** que la herramienta tiene una solicitud aceptada o un préstamo en curso, **Cuando** intento eliminarla, **Entonces** el sistema me advierte y me impide (o pospone) el borrado hasta cerrar ese préstamo. [NECESITA ACLARACIÓN: ¿el borrado es definitivo o se archiva para conservar el historial de préstamos asociados?]

### Casos límite

- ¿Qué pasa si el propietario cambia su ubicación aproximada? ¿Se recalcula el alcance de todas sus herramientas?
- ¿Qué ocurre si se elimina una herramienta que aparece en el historial de préstamos de otros usuarios?
- ¿Cómo se maneja una herramienta publicada por un usuario que luego es suspendido por administración? (Ver EPIC 7.)
- ¿Qué límite hay al número de fotografías o al tamaño por herramienta? [NECESITA ACLARACIÓN]
- ¿Se permite publicar la misma herramienta duplicada varias veces?
- ¿Qué categorías existen y quién las define? [NECESITA ACLARACIÓN: ¿catálogo cerrado de categorías o libre?]

## Requisitos *(obligatorio)*

### Requisitos funcionales

- **FR-001**: El sistema DEBE permitir a un usuario autenticado publicar una herramienta con nombre, categoría, descripción, una o más fotografías, estado de conservación, radio máximo (1–50 km), duración máxima recomendada y condiciones de uso.
- **FR-002**: El sistema DEBE asociar cada herramienta a un único propietario.
- **FR-003**: El sistema DEBE rechazar la publicación si falta algún dato obligatorio. [NECESITA ACLARACIÓN: lista exacta de campos obligatorios frente a opcionales]
- **FR-004**: El sistema DEBE aceptar únicamente radios entre 1 y 50 km, ambos inclusive.
- **FR-005**: El sistema DEBE marcar toda herramienta recién publicada como "disponible".
- **FR-006**: El sistema DEBE permitir al propietario alternar el estado de una herramienta entre "disponible" y "no disponible".
- **FR-007**: El sistema DEBE excluir de las búsquedas las herramientas marcadas como "no disponible".
- **FR-008**: El sistema DEBE permitir al propietario listar todas sus herramientas con su estado actual.
- **FR-009**: El sistema DEBE permitir al propietario editar los datos de una herramienta de la que es dueño.
- **FR-010**: El sistema DEBE impedir que un usuario edite o elimine herramientas que no le pertenecen.
- **FR-011**: El sistema DEBE permitir al propietario eliminar una herramienta que no tenga préstamos o solicitudes activas.
- **FR-012**: El sistema DEBE preservar la integridad del historial de préstamos cuando una herramienta se elimina. [NECESITA ACLARACIÓN: ¿borrado real o archivado?]
- **FR-013**: El sistema DEBE requerir que el propietario tenga una ubicación aproximada de referencia para que su herramienta pueda aplicar el criterio de proximidad.

### Entidades clave

- **Herramienta**: objeto que un propietario ofrece en préstamo. Atributos: nombre, categoría, descripción, fotografías, estado de conservación, disponibilidad (disponible / no disponible), radio máximo (1–50 km), duración máxima recomendada, condiciones de uso. Relación: pertenece a un Usuario (propietario) y se referencia desde las Solicitudes de préstamo.
- **Categoría**: clasificación de la herramienta usada para filtrar en el descubrimiento. [NECESITA ACLARACIÓN: catálogo cerrado o libre]
- **Estado de conservación**: valoración del propietario sobre en qué estado está la herramienta. [NECESITA ACLARACIÓN: ¿escala fija (nuevo/bueno/usado/…) o texto libre?]

## Criterios de éxito *(obligatorio)*

### Resultados medibles

- **SC-001**: Al menos el 50% de los usuarios que se registran como potenciales prestamistas publican su primera herramienta en su primera semana.
- **SC-002**: Al menos el 80% de las herramientas publicadas incluyen fotografía y descripción suficientes para que otro vecino decida solicitarlas sin pedir aclaraciones previas.
- **SC-003**: El propietario medio mantiene actualizado el estado de disponibilidad, de modo que menos del 10% de las solicitudes se rechazan por "la herramienta no estaba realmente disponible".

## Suposiciones

- Las cifras objetivo de los criterios de éxito son propuestas iniciales y deben validarse con negocio.
- El foco del MVP son **exclusivamente herramientas**; no se publican otros objetos.
- Los préstamos son **gratuitos**: la herramienta no lleva precio ni tarifa.
- La ubicación de referencia para la proximidad se toma del perfil del propietario, salvo que se decida lo contrario (ver aclaración en HU-1).
- El detalle del criterio de proximidad (cómo se mide la distancia y cómo se captura la ubicación) se especifica en la feature de Descubrimiento (EPIC 3); aquí solo se fija el radio por herramienta.
- Queda **fuera de alcance**: verificación del estado real de la herramienta por parte de la plataforma, y cualquier garantía sobre su estado (el producto no garantiza el inventario).
