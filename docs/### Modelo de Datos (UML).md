### Modelo de Datos (UML)

Como parte de la definición de la arquitectura del sistema, se completó la tarea de modelado de clases. El modelo representa la estructura principal necesaria para gestionar la disponibilidad, los servicios y las reservas del consultorio podológico.

* **Profesional, Paciente y Servicios:** `Profesional` representa al usuario que administra el sistema y contiene sus datos de acceso y contacto. `Paciente` representa a la persona que reserva un turno y almacena únicamente sus datos necesarios para la gestión de las reservas, sin cuenta ni inicio de sesión. `Servicio` define las prácticas ofrecidas por el profesional, incluyendo nombre, descripción, duración y precio.

* **Configuración de disponibilidad:** `Agenda` centraliza la disponibilidad del profesional. Los horarios habituales se configuran mediante `AgendaConfig`, que define días y franjas horarias, mientras que `ExcepcionAgenda` permite registrar períodos en los que la disponibilidad habitual debe modificarse o bloquearse.

* **Gestión de turnos:** `Turno` relaciona una agenda, un paciente y un servicio. Registra la fecha y hora de inicio, la fecha y hora de finalización, el estado de la reserva, observaciones y la información necesaria para el envío de recordatorios por WhatsApp.

* **Estados del turno:** en el backend, el estado se representa mediante el enum `EstadoTurno`, con los valores `PENDIENTE`, `CONFIRMADO`, `CANCELADO`, `REALIZADO` y `AUSENTE`. En PostgreSQL se almacena como `VARCHAR` y se restringen los valores permitidos mediante un `CHECK`.

  Las transiciones admitidas son:

  - `PENDIENTE` → `CONFIRMADO`, `CANCELADO`
  - `CONFIRMADO` → `REALIZADO`, `AUSENTE`, `CANCELADO`

  Los estados `CANCELADO`, `REALIZADO` y `AUSENTE` son terminales. La base de datos controla estas transiciones mediante el trigger `trg_turno_transicion_estado`.

* **Duración del turno:** la hora de finalización se calcula en el backend a partir de `Servicio.duracionMinutos` y se almacena en `Turno.fechaHoraFin`.

* **Seguimiento de modificaciones:** `Turno` registra `fechaCreacion` y `fechaActualizacion`. La columna de última modificación se actualiza mediante el trigger `trg_turno_fecha_actualizacion`.

* **Cierre de reservas vencidas:** los turnos que permanecen en estado `PENDIENTE` luego de haber pasado su hora de finalización pueden ser cerrados automáticamente como `CANCELADO` mediante la función `fn_cerrar_turnos_vencidos()`.

* **Recordatorios por WhatsApp:** `Turno` registra si el paciente aceptó recibir un recordatorio y si dicho recordatorio ya fue enviado.

* **Instantes y fechas locales:** las fechas y horas de los turnos y las marcas de creación y actualización almacenadas como `TIMESTAMPTZ` se representan mediante `OffsetDateTime` en Java. Las franjas horarias de `AgendaConfig` utilizan `TIME` / `LocalTime`, mientras que las fechas de `ExcepcionAgenda` utilizan `DATE` / `LocalDate`, ya que representan horarios y fechas locales de atención.

El diagrama de clases vigente se encuentra en
[`Diagrama_Clases_Actualizado_V2_Entrega.md`](Diagrama_Clases_Actualizado_V2_Entrega.md).

Está definido mediante sintaxis Mermaid y puede visualizarse directamente desde GitHub o mediante [Mermaid Live](https://mermaid.live).

El diagrama entidad-relación correspondiente al esquema PostgreSQL se encuentra en
[`DER_Actualizado.md`](DER_Actualizado.md).

Ambos modelos se encuentran alineados con el esquema definido en:

`database/schema.sql`