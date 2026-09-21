### Modelo de Datos (UML)

Como parte de la definición de la arquitectura del sistema, se completó la tarea de modelado de clases mediante el siguiente diagrama UML. El esquema detalla la estructura principal para gestionar la disponibilidad y las reservas del consultorio podológico:

*   **Usuarios y Servicios:** Las entidades `Profesional` y `Paciente` estructuran los datos de contacto y acceso, mientras que `Servicio` define las prácticas ofrecidas, su duración y precio.
*   **Configuración de Disponibilidad:** La entidad central `Agenda` administra los horarios habituales a través de `AgendaConfig` (días y franjas horarias) y gestiona los bloqueos temporales mediante `ExcepcionAgenda`.
*   **Transaccionalidad:** La entidad `Turno` relaciona al paciente, la agenda y el servicio, controlando el ciclo de vida de la reserva mediante el enumerador `EstadoTurno` (PENDIENTE, CONFIRMADO, CANCELADO) y el seguimiento de los envíos de WhatsApp.

![Diagrama UML](./docs/Diagrama_UML.jpg)