# TPFinal-Grupo-155
Trabajo Final Integrador Grupo 155 Ludueña-Mariasch

## Sistema de Gestión de Turnos

Proyecto desarrollado para el Trabajo Integrador Final de la
Tecnicatura Universitaria en Programación.

## Descripción

El proyecto consiste en el desarrollo de una aplicación web para
la gestión de turnos de un profesional independiente.

La solución busca simplificar la administración de la agenda y
facilitar la reserva de turnos por parte de los pacientes.

## Modelo UML

### Clases y atributos

**Profesional**
- `id`: Long
- `nombre`: String
- `apellido`: String
- `telefono`: String
- Métodos: `inicializarAgenda(): void`, `configurarExcepcion(): void`

**Agenda**
- `id`: Long
- `activo`: boolean
- Métodos: `agregarConfiguracion(): void`, `agregarExcepcion(): void`

**AgendaConfig**
- `id`: Long
- `diaSemana`: DayOfWeek
- `horaInicio`: LocalTime
- `horaFin`: LocalTime
- `motivo`: String
- Métodos: `validarHorario(): boolean`

**ExcepcionAgenda**
- `id`: Long
- `fechaInicio`: LocalDate
- `fechaFin`: LocalDate
- `motivo`: String
- Métodos: `estaBloqueado(LocalDateTime): boolean`

**Turno**
- `id`: Long
- `fechaHoraInicio`: LocalDateTime
- `fechaHoraFin`: LocalDateTime
- `estado`: EstadoTurno
- `pacienteNombre`: String
- `pacienteTelefono`: String
- Métodos: `confirmar(): void`, `verificarDisponibilidad(): boolean`

**Enum EstadoTurno**
- `PENDIENTE`
- `CONFIRMADO`
- `CANCELADO`

### Relaciones

- `Profesional (1) —◆ Agenda (1)`: composición. Un profesional tiene exactamente una agenda; la agenda no existe sin el profesional.
- `Agenda (1) —◆ AgendaConfig (0..*)`: composición. Una agenda tiene cero o más configuraciones de horario.
- `Agenda (1) —◆ ExcepcionAgenda (0..*)`: composición. Una agenda tiene cero o más excepciones.
- `Agenda (1) — Turno (0..*)`: asociación. Una agenda tiene cero o más turnos.
- `Turno ··→ EstadoTurno`: dependencia. El turno usa el enum para su estado.

## Base de datos

Esquema relacional (PostgreSQL) del modelo UML en `database/schema.sql`.

Nota MVP: el esquema no modela una entidad `Paciente`; los datos mínimos del paciente se guardan embebidos en `turno` (`paciente_nombre`, `paciente_telefono`).

## Módulos a desarrollar (MVP)

Normalizados a partir de las clases del modelo UML.

1. **Gestión de agenda** — módulo administrativo de `Profesional` para configurar su `Agenda`: días de atención y franjas horarias (`AgendaConfig`) y excepciones (`ExcepcionAgenda`).
2. **Reserva pública de turnos** — consulta de disponibilidad y reserva de `Turno` sin registro obligatorio ni validación por correo electrónico.
3. **Gestión de turnos** — alta, baja, modificación y estados de `Turno` (enum `EstadoTurno`).
4. **Validaciones de negocio** — impedir turnos duplicados o solapados.
5. **Recordatorios (WhatsApp)** — generar y enviar recordatorios de turnos, con plan de contingencia para envío manual.
6. **Despliegue** — despliegue online del frontend, backend y base de datos.

### Regla de negocio: cancelación de turnos

Se descarta el corrimiento automático de turnos ante una cancelación. Si el profesional necesita cubrir un turno liberado, lo gestiona manualmente contactando telefónicamente a pacientes (incluido algún caso de urgencia), ya que el corrimiento automático exigiría que todos los pacientes modifiquen su agenda.

### Decisión de diseño (normalización MVP)

La entidad `Paciente` queda **fuera del modelo** UML y del esquema de base de datos. Los datos mínimos del paciente se guardan como atributos embebidos en `Turno` (`pacienteNombre`, `pacienteTelefono`). Esto elimina el registro de cuenta y la validación de correo en la reserva, priorizando la simplicidad para el paciente.

### Fuera del alcance del MVP

- Pasarelas de pago online.
- Facturación electrónica.
- Historia clínica integral.
- Aplicaciones móviles nativas.
- Múltiples consultorios o arquitectura multi-tenant.

### Aprobación de módulos

| Módulo | Tutor (fecha/medio) | Comité (fecha) |
| --- | --- | --- |
| 1. Gestión de agenda | Pendiente | Pendiente |
| 2. Reserva pública de turnos | Pendiente | Pendiente |
| 3. Gestión de turnos | Pendiente | Pendiente |
| 4. Validaciones de negocio | Pendiente | Pendiente |
| 5. Recordatorios (WhatsApp) | Pendiente | Pendiente |
| 6. Despliegue | Pendiente | Pendiente |
| Decisión: Paciente fuera del MVP | Pendiente | Pendiente |

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

Etapa de diseño (2.ª entrega): modelo UML, esquema de base de datos y módulos del MVP definidos. Pendiente de aprobación de módulos por el tutor y el comité para iniciar el desarrollo.
