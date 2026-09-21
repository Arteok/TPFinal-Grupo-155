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

Diagrama y descripción del modelo en `docs/UML.docx` y `docs/UML.png`.

## Base de datos

Esquema relacional PostgreSQL en `database/schema.sql`.

El modelo incluye a `Paciente` como una entidad independiente. Los datos básicos del paciente (`nombre`, `apellido` y `telefono`) se almacenan una sola vez y cada `Turno` se relaciona mediante `paciente_id`. Esto permite consultar el historial de turnos y actualizar los datos de contacto sin repetirlos en cada reserva.

La existencia de `Paciente` **no implica crear una cuenta de usuario**: para reservar, el paciente solo ingresa sus datos básicos y teléfono, sin contraseña ni validación por correo electrónico.

También se incorpora la entidad `Servicio`. Cada servicio pertenece al profesional y contiene nombre, descripción, duración y precio. Al reservar un turno, el paciente elige un servicio y el sistema calcula la hora de finalización según su duración.

Las entidades que necesitan conservar historial se manejarán con baja lógica (`activo = false`) en lugar de eliminarse físicamente.

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
- Reserva de varios servicios en un mismo turno.
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
