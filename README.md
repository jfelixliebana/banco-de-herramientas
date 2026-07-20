# Specs SDD — MVP Banco Comunitario de Herramientas

Conjunto de especificaciones `spec.md` generadas con la skill **`crear-spec-sdd`** (estándar GitHub Spec Kit), una por épica del backlog. Cada spec captura el **QUÉ** y el **PORQUÉ**, nunca el **CÓMO** técnico, y todas están en estado **Borrador** hasta cerrar las aclaraciones pendientes.

## Mapa épica → feature

| # | Rama | Épica del backlog | Historias |
|---|------|-------------------|-----------|
| 001 | `001-gestion-usuarios` | EPIC 1 — Gestión de usuarios | HU-001, 002, 003, 004, 005, 031 |
| 002 | `002-gestion-herramientas` | EPIC 2 — Gestión de herramientas | HU-006, 007, 008, 009, 010 |
| 003 | `003-descubrimiento` | EPIC 3 — Descubrimiento | HU-011, 012, 013, 014, 015 |
| 004 | `004-solicitudes-prestamo` | EPIC 4 — Solicitudes de préstamo | HU-016–022 |
| 005 | `005-comunicacion` | EPIC 5 — Comunicación | HU-023, 024 |
| 006 | `006-reputacion` | EPIC 6 — Reputación | HU-025, 026, 027 |
| 007 | `007-administracion` | EPIC 7 — Administración | HU-028, 029, 030 |

## Traducción de prioridades

El backlog usa **P0/P1/P2**; la skill usa **P1/P2/P3**. Mapeo aplicado en todas las specs:

- **P0 → P1** (imprescindible, el MVP no existe sin ella)
- **P1 → P2** (importante)
- **P2 → P3** (deseable)

## Orden de construcción sugerido

El valor de extremo a extremo aparece al combinar 001 → 002 → 003 → 004. Las features 005 (comunicación) y 006 (reputación) envuelven ese núcleo, y 007 (administración) es candidata a posponerse (sus historias son las de menor prioridad). Esto es coherente con el roadmap por sprints del backlog.

## Aclaraciones transversales pendientes (bloquean el paso de Borrador a definitivo)

1. **Ubicación aproximada** (001, 002, 003): cómo la introduce el usuario (dirección manual, punto/zona en mapa, captura puntual) y con qué granularidad, sabiendo que la geolocalización en tiempo real queda fuera de alcance.
2. **Verificación de email** (001): ¿es bloqueante para publicar/solicitar, o solo un distintivo de confianza?
3. **Índice de respuesta** (004, 006): fórmula exacta y ventana temporal; qué cuenta como "responder".
4. **Confirmación de entrega/devolución** (004): ¿la confirma una parte o requiere ambas? ¿Hay cierre por inactividad?
5. **Chat: momento de apertura y cierre** (005): ¿se habilita al enviar la solicitud o al aceptarla? ¿Qué pasa con la conversación al cerrar el préstamo?
6. **Canales de notificación** (005): dentro de la app / email / push, y si el usuario los configura.
7. **Visibilidad de valoraciones** (006): publicación inmediata vs. doble ciego con plazo, para evitar represalias.
8. **Categorías y estado de conservación** (002): ¿catálogo cerrado o libre? ¿escala fija o texto?
9. **Alcance real de Administración en el MVP** (007): roles admin, flujo de reporte in-app, y política de retención/anonimización de datos (protección de datos).
10. **Borrado de herramientas y cuentas** (002, 007): ¿borrado real o archivado, para preservar historiales?
