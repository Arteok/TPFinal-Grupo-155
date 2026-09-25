### Modelo de Datos (UML)

Como parte de la definición de la arquitectura del sistema, se completó la tarea de modelado de clases mediante el siguiente diagrama UML. El esquema detalla la estructura principal para gestionar la disponibilidad y las reservas del consultorio podológico:

*   **Usuarios y Servicios:** Las entidades `Profesional` y `Paciente` estructuran los datos de contacto y acceso, mientras que `Servicio` define las prácticas ofrecidas, su duración y precio.
*   **Configuración de Disponibilidad:** La entidad central `Agenda` administra los horarios habituales a través de `AgendaConfig` (días y franjas horarias) y gestiona los bloqueos temporales mediante `ExcepcionAgenda`.
*   **Transaccionalidad:** La entidad `Turno` relaciona al paciente, la agenda y el servicio, controlando el ciclo de vida de la reserva mediante el enumerador `EstadoTurno` (PENDIENTE, CONFIRMADO, CANCELADO, REALIZADO, AUSENTE) y el seguimiento de los envíos de WhatsApp. Las transiciones están restringidas a la matriz `PENDIENTE → CONFIRMADO, CANCELADO` y `CONFIRMADO → REALIZADO, AUSENTE, CANCELADO`, con `CANCELADO`, `REALIZADO` y `AUSENTE` como estados terminales; la base de datos lo impone con el trigger `trg_turno_transicion_estado`. Registra además `fecha_creacion` y `fecha_actualizacion`, esta última mantenida por el trigger `trg_turno_fecha_actualizacion`. Las reservas `PENDIENTE` que nunca se confirman se cierran solas como `CANCELADO` al pasar la hora del turno, mediante `fn_cerrar_turnos_vencidos()`.

*   **Instantes y fechas locales:** los horarios de los turnos se guardan como `TIMESTAMPTZ` (`OffsetDateTime` en Java) porque son instantes reales y el despliegue corre en UTC. En cambio, las franjas de `AgendaConfig` (`TIME`, `LocalTime`) y las fechas de `ExcepcionAgenda` (`DATE`, `LocalDate`) describen un horario o un día local y se mantienen sin zona.

El diagrama de clases vigente está en
[`Diagrama_Clases_Actualizado_V2_Entrega.md`](Diagrama_Clases_Actualizado_V2_Entrega.md), en sintaxis
Mermaid: se renderiza al abrir el archivo en GitHub, o se puede pegar en <https://mermaid.live> para
exportarlo como imagen. Incluye el enum `EstadoTurno` con cinco estados y las columnas de última
modificación.

El diagrama entidad-relación de la base de datos está en [`DER_Actualizado.md`](DER_Actualizado.md).