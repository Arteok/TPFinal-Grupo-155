# TPFinal-Grupo-155
Trabajo Final Integrador Grupo 155 Ludueña-Mariasch

## Sistema de Gestión de Turnos

Proyecto desarrollado para el Trabajo Integrador Final de la
Tecnicatura Universitaria en Programación.

## Descripción

El proyecto consiste en el desarrollo de una aplicación web para la gestión de turnos de un profesional independiente.

La solución busca simplificar la administración de la agenda y facilitar la reserva de turnos por parte de los pacientes. Como referencia de uso se incorpora la selección de un **servicio** antes de elegir fecha y horario, manteniendo un alcance acotado para el MVP.

El flujo principal de reserva será:

**Servicio → fecha → horario → datos del paciente → confirmación del turno.**

## Modelo UML

Diagrama de clases en sintaxis Mermaid (se renderiza en GitHub) en [`docs/Diagrama_Clases_Actualizado_V2_Entrega.md`](docs/Diagrama_Clases_Actualizado_V2_Entrega.md), y su descripción en [`docs/### Modelo de Datos (UML).md`](docs/###%20Modelo%20de%20Datos%20%28UML%29.md).

## Base de datos

Esquema relacional PostgreSQL en `database/schema.sql`.

Diagrama entidad-relación en [`docs/DER_Actualizado.md`](docs/DER_Actualizado.md): incluye las siete
entidades, las claves foráneas, los índices y las restricciones `CHECK` del script.

El modelo incluye a `Paciente` como una entidad independiente. Los datos básicos del paciente (`nombre`, `apellido` y `telefono`) se almacenan una sola vez y cada `Turno` se relaciona mediante `paciente_id`. Esto permite consultar el historial de turnos y actualizar los datos de contacto sin repetirlos en cada reserva.

La existencia de `Paciente` **no implica crear una cuenta de usuario**: para reservar, el paciente solo ingresa sus datos básicos y teléfono, sin contraseña ni validación por correo electrónico.

También se incorpora la entidad `Servicio`. Cada servicio pertenece al profesional y contiene nombre, descripción, duración y precio. Al reservar un turno, el paciente elige un servicio y el sistema calcula la hora de finalización según su duración.

Las entidades que necesitan conservar historial se manejarán con baja lógica (`activo = false`) en lugar de eliminarse físicamente.

### Última modificación

`Paciente` y `Turno` registran `fecha_creacion`. Además, `Turno` tiene `fecha_actualizacion`, que se mantiene al día mediante el trigger `trg_turno_fecha_actualizacion` definido en `database/schema.sql`: un `DEFAULT CURRENT_TIMESTAMP` solo se aplica al insertar, por lo que sin el trigger la columna quedaría congelada con el valor de la creación en cada actualización.

Esto es **última modificación, no auditoría**: la columna no registra quién cambió el turno ni qué cambió. Una auditoría real exigiría una tabla de historial, fuera del alcance del MVP.

### Decisión de diseño: zonas horarias

Un turno es un instante real en el calendario, por lo que `turno.fecha_hora_inicio`, `turno.fecha_hora_fin`, `turno.fecha_creacion`, `turno.fecha_actualizacion` y `paciente.fecha_creacion` se guardan como `TIMESTAMPTZ` (con zona horaria). El despliegue en Railway corre en UTC: guardar la hora sin zona haría que un turno de las 10:00 se mostrara corrido al cambiar de huso.

En cambio, `agenda_config.hora_inicio` y `agenda_config.hora_fin` siguen siendo `TIME`, y `excepcion_agenda.fecha_inicio` y `excepcion_agenda.fecha_fin` siguen siendo `DATE`: describen un horario o un día local, no un instante, y no les corresponde zona horaria. En el backend se mapearán con `OffsetDateTime` (o `Instant`), no con `LocalDateTime`.

## Documentación

- [`docs/MODULOS.md`](docs/MODULOS.md) — definición de los seis módulos del MVP, entidades relacionadas por módulo y la tabla de aprobación del tutor y el comité.
- [`docs/Trabajo Final IntegradorV2.docx`](docs/Trabajo%20Final%20IntegradorV2.docx) — propuesta de proyecto (versión vigente).
- [`docs/Trabajo Final Integrador.docx`](docs/Trabajo%20Final%20Integrador.docx) — versión anterior de la propuesta, conservada como respaldo.

## Módulos a desarrollar (MVP)

1. **Gestión de agenda y servicios** — acceso al panel del profesional mediante email y contraseña; configuración de días de atención, franjas horarias (`AgendaConfig`), excepciones (`ExcepcionAgenda`) y servicios ofrecidos con nombre, descripción, duración y precio.
2. **Reserva pública de turnos** — selección de servicio, consulta de disponibilidad, elección de fecha y horario, carga de datos del paciente y reserva de `Turno` sin registro obligatorio ni validación por correo electrónico. El paciente podrá aceptar o no recibir recordatorios por WhatsApp.
3. **Gestión de pacientes y turnos** — alta, consulta y modificación de datos básicos de `Paciente`, consulta de su historial de turnos, y alta, modificación, cancelación y cambio de estado de `Turno` mediante el enum `EstadoTurno`.
4. **Validaciones de negocio** — impedir turnos duplicados y controlar solapamientos de horarios. La duración del `Servicio` se utilizará para calcular la hora de finalización y validar que el turno completo esté disponible.
5. **Recordatorios (WhatsApp)** — generar y enviar recordatorios de turnos cuando el paciente haya aceptado recibirlos, con plan de contingencia para envío manual si la integración no resulta viable dentro del plazo académico.
6. **Despliegue** — despliegue online del frontend, backend y base de datos.

### Regla de negocio: servicios

- Un profesional puede ofrecer varios servicios.
- Cada servicio tiene nombre, descripción, duración en minutos, precio y estado activo/inactivo.
- Cada turno corresponde a un solo servicio.
- La duración del servicio se utiliza para calcular la hora de finalización del turno.
- En este MVP no se reservarán varios servicios dentro de un mismo turno.

### Regla de negocio: estados del turno

- `PENDIENTE`: reserva creada por el paciente y todavía no confirmada por la profesional.
- `CONFIRMADO`: turno aceptado por la profesional.
- `CANCELADO`: turno anulado y liberado para una nueva reserva.
- `REALIZADO`: turno atendido y finalizado.
- `AUSENTE`: el paciente no se presentó a la hora reservada.

Solo `PENDIENTE` y `CONFIRMADO` ocupan el horario: los demás estados lo liberan.

#### Transiciones permitidas

| Desde | Hacia |
| --- | --- |
| `PENDIENTE` | `CONFIRMADO`, `CANCELADO` |
| `CONFIRMADO` | `REALIZADO`, `AUSENTE`, `CANCELADO` |
| `CANCELADO` | — (terminal) |
| `REALIZADO` | — (terminal) |
| `AUSENTE` | — (terminal) |

La matriz se valida en la base de datos mediante el trigger `trg_turno_transicion_estado` (función `valida_turno_transicion_estado`), que rechaza cualquier cambio de estado no previsto con un error `check_violation`. Los servicios de aplicación deben validar la misma matriz antes de invocar la operación, para poder devolver un mensaje de negocio en lugar de una excepción de SQL.

Dos decisiones explícitas:

- **No se permite `CONFIRMADO → PENDIENTE`.** Desconfirmar un turno ya confirmado ensuciaría el historial del paciente, que es el registro que consulta para saber qué pasó con cada visita.
- **Reprogramar** (cambiar `fecha_hora_inicio` y `fecha_hora_fin`) sí se permite mientras el turno esté en `PENDIENTE` o `CONFIRMADO`. No se admite sobre un estado terminal.

### Regla de negocio: vencimiento de reservas sin confirmar

Un turno `PENDIENTE` cuya hora de fin ya pasó se cierra automáticamente como `CANCELADO`. La operación la realiza la función `fn_cerrar_turnos_vencidos()`, que el backend invoca de forma periódica (`@Scheduled`) y que devuelve cuántos turnos cerró. El cierre deja constancia en `observaciones` sin pisar lo que ya hubiera, y la operación es idempotente.

**Consecuencia asumida:** al cerrar al pasar la hora, un turno pasado ya no se puede confirmar. En el historial del paciente, un turno que nunca fue confirmado figura como `CANCELADO` y nunca como `AUSENTE`; `AUSENTE` queda reservado para los turnos que estaban `CONFIRMADO` y el paciente no se presentó.

### Regla de negocio: cancelación de turnos

Se descarta el corrimiento automático de turnos ante una cancelación. Si el profesional necesita cubrir un turno liberado, lo gestiona manualmente contactando a otro paciente, ya que mover turnos automáticamente podría afectar la organización de los demás pacientes.

### Decisión de diseño: Paciente como entidad independiente

`Paciente` forma parte del modelo UML y del esquema de base de datos. Se guardan sus datos básicos (`nombre`, `apellido`, `telefono`) en una tabla propia y los turnos se vinculan mediante una clave foránea.

Esta decisión agrega algo de trabajo al MVP porque requiere alta, búsqueda y actualización de pacientes, pero evita repetir datos en cada turno y permite contar con un historial por paciente. El paciente no tendrá usuario ni contraseña en esta primera versión.

### Fuera del alcance del MVP

- Pasarelas de pago online.
- Facturación electrónica.
- Historia clínica integral.
- Aplicaciones móviles nativas.
- Cuenta o inicio de sesión para pacientes.
- Inicio de sesión con Google para pacientes.
- Códigos de descuento.
- Reserva de varios servicios dentro de un mismo turno.
- Múltiples consultorios o arquitectura multi-tenant.

### Aprobación de módulos

| Módulo | Tutor (fecha/medio) | Comité (fecha) |
| --- | --- | --- |
| 1. Gestión de agenda y servicios | Pendiente | Pendiente |
| 2. Reserva pública de turnos | Pendiente | Pendiente |
| 3. Gestión de pacientes y turnos | Pendiente | Pendiente |
| 4. Validaciones de negocio | Pendiente | Pendiente |
| 5. Recordatorios (WhatsApp) | Pendiente | Pendiente |
| 6. Despliegue | Pendiente | Pendiente |

## Tecnologías

### Backend
- Java
- Spring Boot
- Gradle

### Frontend
- React
- TypeScript
- Vite
- HTML5
- CSS3

### Base de datos
- PostgreSQL

### Comunicación
- API REST

### Despliegue
- Vercel
- Railway

## Integrantes

- Pablo Mariasch
- Bruno Ludueña

## Estado

Etapa de diseño (2.ª entrega): modelo UML, esquema de base de datos y módulos del MVP definidos. Pendiente de aprobación explícita del tutor y del comité antes de iniciar el desarrollo principal.
