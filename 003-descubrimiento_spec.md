# Especificación de Feature: Descubrimiento de herramientas por proximidad

**Rama**: `003-descubrimiento`
**Creada**: 2026-07-15
**Estado**: Borrador
**Input**: Descripción del usuario: "Banco Comunitario de Herramientas — MVP. EPIC 3 (Descubrimiento): que un vecino pueda buscar herramientas disponibles cerca de él, filtrarlas por categoría y por distancia, ver la ficha completa de una herramienta y ver todas las herramientas de un mismo propietario. Solo puede solicitar una herramienta si está dentro del radio máximo que definió su dueño (1–50 km)."

> **Nota de prioridades**: el backlog original usa P0/P1/P2. Aquí se traduce al estándar de la skill: **P0→P1** (imprescindible), **P1→P2** (importante), **P2→P3** (deseable).

## Escenarios de usuario y pruebas *(obligatorio)*

### Historia de Usuario 1 - Buscar herramientas disponibles cerca de mí (Prioridad: P1)

Como vecino que necesita una herramienta puntualmente, quiero buscar qué hay disponible cerca de mi ubicación, para encontrar algo que pueda recoger fácilmente sin tener que comprarlo.

**Por qué esta prioridad**: es la mitad de la demanda del marketplace; sin descubrimiento por proximidad, el prestatario no puede encontrar nada y la propuesta de valor entera se cae.

**Test independiente**: se puede probar buscando desde una ubicación conocida y comprobando que aparecen las herramientas disponibles cuyo alcance cubre esa ubicación, y no las demás.

**Escenarios de aceptación**:
1. **Dado** que soy un usuario autenticado con ubicación aproximada definida, **Cuando** hago una búsqueda, **Entonces** veo las herramientas marcadas como "disponibles" cuyo radio máximo alcanza mi ubicación, ordenadas por relevancia de cercanía.
2. **Dado** que estoy fuera del radio máximo de una herramienta, **Cuando** busco, **Entonces** esa herramienta no aparece entre mis resultados.
3. **Dado** que no hay ninguna herramienta que me alcance, **Cuando** busco, **Entonces** veo un estado vacío que me lo explica y me sugiere ampliar criterios o volver más tarde.
4. **Dado** que no tengo ubicación aproximada definida, **Cuando** intento buscar, **Entonces** el sistema me pide fijarla primero, porque la proximidad es condición del producto.

### Historia de Usuario 2 - Ver la ficha de una herramienta (Prioridad: P1)

Como vecino interesado en una herramienta, quiero ver toda su información y la de su propietario, para decidir si la solicito.

**Por qué esta prioridad**: es el paso obligatorio entre encontrar y solicitar; sin la ficha, el prestatario no tiene datos para decidir ni para confiar.

**Test independiente**: se puede probar abriendo la ficha de una herramienta concreta y comprobando que muestra todos sus datos y los datos públicos de su dueño.

**Escenarios de aceptación**:
1. **Dado** que veo una herramienta en los resultados, **Cuando** abro su ficha, **Entonces** veo nombre, categoría, descripción, fotografías, estado de conservación, disponibilidad, duración máxima recomendada, condiciones de uso y el perfil público de su propietario (incluida su reputación y estado de verificación).
2. **Dado** que abro la ficha de una herramienta que está dentro de mi alcance y disponible, **Cuando** se muestra, **Entonces** dispongo de la opción de solicitar el préstamo.
3. **Dado** que abro la ficha de una herramienta que quedó "no disponible" o fuera de mi alcance mientras navegaba, **Cuando** se muestra, **Entonces** el sistema me indica su estado y no me ofrece solicitarla.

### Historia de Usuario 3 - Filtrar por categoría (Prioridad: P1)

Como vecino que busca algo concreto, quiero filtrar por categoría, para llegar antes al tipo de herramienta que necesito.

**Por qué esta prioridad**: con un inventario mínimamente amplio, sin filtro por categoría la búsqueda se vuelve inmanejable; es parte del descubrimiento básico.

**Test independiente**: se puede probar aplicando un filtro de categoría y comprobando que solo aparecen herramientas de esa categoría dentro de mi alcance.

**Escenarios de aceptación**:
1. **Dado** que estoy en la búsqueda, **Cuando** filtro por una categoría, **Entonces** solo veo herramientas disponibles de esa categoría que alcancen mi ubicación.
2. **Dado** que una categoría no tiene herramientas dentro de mi alcance, **Cuando** la selecciono, **Entonces** veo un estado vacío explicativo.

### Historia de Usuario 4 - Filtrar por distancia (Prioridad: P2)

Como vecino, quiero acotar los resultados a una distancia máxima menor que el alcance de las herramientas, para priorizar lo que tengo más a mano.

**Por qué esta prioridad**: mejora la experiencia cuando hay mucho inventario, pero el filtro de proximidad base (el radio del propietario) ya limita los resultados; por eso es importante, no imprescindible.

**Test independiente**: se puede probar fijando una distancia máxima y comprobando que se ocultan las herramientas más lejanas aunque su radio me alcanzara.

**Escenarios de aceptación**:
1. **Dado** que veo resultados, **Cuando** fijo una distancia máxima, **Entonces** solo permanecen las herramientas cuya distancia estimada a mi ubicación es menor o igual a la fijada.
2. **Dado** que fijo una distancia muy pequeña sin resultados, **Cuando** se aplica, **Entonces** veo un estado vacío y puedo ampliarla fácilmente.

### Historia de Usuario 5 - Ver las herramientas de un propietario (Prioridad: P3)

Como vecino que ha tenido una buena experiencia con alguien, quiero ver todo lo que ese propietario ofrece, para volver a pedirle prestado con confianza.

**Por qué esta prioridad**: refuerza la relación de confianza y la recurrencia, pero no es necesaria para el ciclo básico de descubrir y solicitar; por eso es deseable.

**Test independiente**: se puede probar abriendo el listado público de herramientas de un propietario y comprobando que aparecen las suyas disponibles que me alcanzan.

**Escenarios de aceptación**:
1. **Dado** que estoy en el perfil público de un propietario, **Cuando** consulto sus herramientas, **Entonces** veo las que tiene disponibles y cuyo alcance cubre mi ubicación.
2. **Dado** que el propietario no tiene ninguna herramienta que me alcance, **Cuando** consulto, **Entonces** veo un estado vacío explicativo.

### Casos límite

- ¿Qué pasa si el usuario cambia su ubicación aproximada entre una búsqueda y la siguiente? (Los resultados deben recalcularse.)
- ¿Cómo se calcula la "distancia" entre dos ubicaciones aproximadas sin exponer posiciones exactas?
- ¿Qué se muestra si una herramienta pasa a "no disponible" justo después de aparecer en resultados pero antes de abrir su ficha?
- ¿Un usuario "sin verificar" puede buscar y ver fichas, o solo los verificados? (Ver EPIC 1.)
- ¿Cómo se ordenan resultados a igual cercanía? [NECESITA ACLARACIÓN: ¿reputación, novedad, aleatorio?]
- ¿Se permite buscar por texto libre además de por categoría? [NECESITA ACLARACIÓN: el backlog no incluye búsqueda por palabra clave en el MVP]

## Requisitos *(obligatorio)*

### Requisitos funcionales

- **FR-001**: El sistema DEBE permitir a un usuario autenticado con ubicación aproximada definida buscar herramientas disponibles.
- **FR-002**: El sistema DEBE mostrar únicamente herramientas "disponibles" cuyo radio máximo cubra la ubicación aproximada del usuario que busca.
- **FR-003**: El sistema DEBE impedir que aparezcan (y que se puedan solicitar) herramientas cuyo alcance no cubra al usuario.
- **FR-004**: El sistema DEBE requerir que el usuario tenga una ubicación aproximada antes de poder buscar.
- **FR-005**: El sistema DEBE permitir filtrar los resultados por categoría.
- **FR-006**: El sistema DEBE permitir filtrar los resultados por una distancia máxima definida por el usuario.
- **FR-007**: El sistema DEBE mostrar una ficha de herramienta con todos sus datos y el perfil público de su propietario.
- **FR-008**: El sistema DEBE ofrecer la opción de solicitar el préstamo únicamente cuando la herramienta está disponible y dentro del alcance del usuario.
- **FR-009**: El sistema DEBE permitir consultar las herramientas (disponibles y dentro de alcance) de un propietario concreto.
- **FR-010**: El sistema DEBE estimar la distancia entre usuario y herramienta sin exponer ubicaciones exactas de ninguna de las partes.
- **FR-011**: El sistema DEBE definir cómo se captura y representa la ubicación aproximada. [NECESITA ACLARACIÓN: ¿el usuario introduce una dirección, elige un punto/zona en un mapa, o se captura su posición de forma puntual? Recordatorio: la "geolocalización en tiempo real" queda fuera de alcance.]
- **FR-012**: El sistema DEBE definir un orden por defecto de los resultados. [NECESITA ACLARACIÓN: criterio de ordenación a igualdad de cercanía]

### Entidades clave

- **Ubicación aproximada**: representación no exacta de dónde está un usuario (o su herramienta), usada para calcular proximidad sin revelar la posición real. [NECESITA ACLARACIÓN: forma de captura y granularidad — barrio, código postal, punto difuminado…]
- **Resultado de búsqueda**: conjunto de herramientas disponibles que alcanzan al usuario, con su distancia estimada, filtrable por categoría y distancia.
- *(Reutiliza las entidades **Herramienta** y **Categoría** definidas en EPIC 2 y **Usuario** de EPIC 1.)*

## Criterios de éxito *(obligatorio)*

### Resultados medibles

- **SC-001**: Al menos el 70% de las búsquedas que un usuario realiza en una zona con inventario devuelven al menos una herramienta relevante.
- **SC-002**: Al menos el 40% de los usuarios que abren una ficha de herramienta acaban iniciando una solicitud de préstamo.
- **SC-003**: El tiempo medio desde que un prestatario empieza a buscar hasta que abre una ficha que le encaja es inferior a 2 minutos.

## Suposiciones

- Las cifras objetivo de los criterios de éxito son propuestas iniciales y deben validarse con negocio.
- El **criterio de proximidad del MVP es el radio por herramienta** (1–50 km) que fija cada propietario, tal como establece el backlog. La hipótesis de la inception sobre "radio fijo / comunidades / distancia configurable" se considera resuelta a favor del radio configurable por herramienta, pendiente de validación con usuarios.
- La ubicación de ambas partes es siempre **aproximada**; nunca se muestran posiciones exactas.
- Queda **fuera de alcance**: geolocalización en tiempo real, recomendaciones mediante IA y (a validar) la búsqueda por texto libre.
- La acción de solicitar el préstamo desde la ficha pertenece a la feature de Solicitudes (EPIC 4); aquí solo se habilita el punto de entrada.
