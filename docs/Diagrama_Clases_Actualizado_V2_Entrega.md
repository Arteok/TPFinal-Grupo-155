# Diagrama de clases — V2 Entrega

Diagrama en sintaxis [Mermaid](https://mermaid.js.org). Se visualiza directamente en GitHub/GitLab, o
pegando el bloque en <https://mermaid.live>.

Refleja el esquema de `database/schema.sql`: enum `EstadoTurno` con cinco estados y las columnas de
última modificación. `Turno.fechaActualizacion` se mantiene mediante el trigger
`trg_turno_fecha_actualizacion` definido en el mismo script.

Los cinco campos de fecha/hora de `Turno` y `Paciente` se tipan `OffsetDateTime` porque en la base son
`TIMESTAMPTZ`: son instantes reales y el despliegue corre en UTC. `AgendaConfig.horaInicio/horaFin`
(`LocalTime`) y `ExcepcionAgenda.fechaInicio/fechaFin` (`LocalDate`) se quedan sin zona a propósito,
porque describen un horario o un día local.

Los métodos de cambio de estado de `Turno` (`confirmar`, `cancelar`, `marcarRealizado`, `marcarAusente`)
sólo tienen éxito si la transición está en la matriz: `PENDIENTE → CONFIRMADO, CANCELADO` y
`CONFIRMADO → REALIZADO, AUSENTE, CANCELADO`. `CANCELADO`, `REALIZADO` y `AUSENTE` son terminales. La
base de datos lo garantiza con el trigger `trg_turno_transicion_estado`; `reprogramar` sólo aplica
mientras el turno esté en `PENDIENTE` o `CONFIRMADO`.

```mermaid
classDiagram
    class Profesional {
        -id Long
        -nombre String
        -apellido String
        -telefono String
        -email String
        -passwordHash String
        -activo boolean
        +actualizarDatos() void
        +cambiarPassword(nuevoHash String) void
        +desactivar() void
    }

    class Agenda {
        -id Long
        -profesional Profesional
        -activo boolean
        +agregarConfiguracion(config AgendaConfig) void
        +agregarExcepcion(excepcion ExcepcionAgenda) void
        +desactivar() void
    }

    class AgendaConfig {
        -id Long
        -agenda Agenda
        -diaSemana DayOfWeek
        -horaInicio LocalTime
        -horaFin LocalTime
        -activo boolean
        +actualizarRango(horaInicio LocalTime, horaFin LocalTime) void
        +desactivar() void
        +esHorarioValido() boolean
    }

    class ExcepcionAgenda {
        -id Long
        -agenda Agenda
        -fechaInicio LocalDate
        -fechaFin LocalDate
        -motivo String
        -activo boolean
        +actualizarFechas(fechaInicio LocalDate, fechaFin LocalDate) void
        +desactivar() void
        +estaVigente(fecha LocalDate) boolean
    }

    class Servicio {
        -id Long
        -profesional Profesional
        -nombre String
        -descripcion String
        -duracionMinutos Integer
        -precio BigDecimal
        -activo boolean
        +actualizarDatos() void
        +cambiarPrecio(nuevoPrecio BigDecimal) void
        +desactivar() void
    }

    class Paciente {
        -id Long
        -nombre String
        -apellido String
        -telefono String
        -activo boolean
        -fechaCreacion OffsetDateTime
        +actualizarDatos(nombre String, apellido String, telefono String) void
        +desactivar() void
    }

    class Turno {
        -id Long
        -agenda Agenda
        -paciente Paciente
        -servicio Servicio
        -fechaHoraInicio OffsetDateTime
        -fechaHoraFin OffsetDateTime
        -estado EstadoTurno
        -observaciones String
        -aceptaRecordatorioWhatsapp boolean
        -recordatorioEnviado boolean
        -fechaCreacion OffsetDateTime
        -fechaActualizacion OffsetDateTime
        +confirmar() void
        +cancelar() void
        +marcarRealizado() void
        +marcarAusente() void
        +reprogramar(nuevaFechaHora OffsetDateTime) void
        +marcarRecordatorioEnviado() void
        +estaEnEstado(estado EstadoTurno) boolean
    }

    class EstadoTurno {
        <<enum>>
        PENDIENTE
        CONFIRMADO
        CANCELADO
        REALIZADO
        AUSENTE
    }

    Profesional "1" --> "1" Agenda : posee
    Profesional "1" --> "0..*" Servicio : ofrece
    Agenda "1" *-- "0..*" AgendaConfig : compone
    Agenda "1" *-- "0..*" ExcepcionAgenda : compone
    Agenda "1" --> "0..*" Turno : tiene
    Paciente "1" --> "0..*" Turno : realiza
    Servicio "1" --> "0..*" Turno : se reserva en
    Turno --> EstadoTurno : estado
```
