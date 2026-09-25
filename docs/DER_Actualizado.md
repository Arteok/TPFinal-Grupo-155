# DER — Diagrama entidad-relación (V2 Entrega)

Diagrama en sintaxis [Mermaid](https://mermaid.js.org). Se visualiza directamente en GitHub/GitLab, o
pegando el bloque en <https://mermaid.live>.

Generado a partir de `database/schema.sql`. Refleja el enum `EstadoTurno` con cinco estados, las columnas
de última modificación y el trigger `trg_turno_fecha_actualizacion`.

```mermaid
erDiagram
    PROFESIONAL ||--|| AGENDA : "posee"
    PROFESIONAL ||--o{ SERVICIO : "ofrece"
    AGENDA ||--o{ AGENDA_CONFIG : "compone"
    AGENDA ||--o{ EXCEPCION_AGENDA : "compone"
    AGENDA ||--o{ TURNO : "recibe"
    PACIENTE ||--o{ TURNO : "reserva"
    SERVICIO ||--o{ TURNO : "se reserva en"

    PROFESIONAL {
        bigint id PK
        varchar nombre
        varchar apellido
        varchar telefono
        varchar email UK
        varchar password_hash
        boolean activo
    }

    AGENDA {
        bigint id PK
        boolean activo
        bigint profesional_id "FK, UK"
    }

    AGENDA_CONFIG {
        bigint id PK
        varchar dia_semana
        time hora_inicio
        time hora_fin
        boolean activo
        bigint agenda_id FK
    }

    EXCEPCION_AGENDA {
        bigint id PK
        date fecha_inicio
        date fecha_fin
        varchar motivo
        boolean activo
        bigint agenda_id FK
    }

    PACIENTE {
        bigint id PK
        varchar nombre
        varchar apellido
        varchar telefono
        boolean activo
        timestamptz fecha_creacion
    }

    SERVICIO {
        bigint id PK
        varchar nombre
        varchar descripcion
        integer duracion_minutos
        numeric precio
        boolean activo
        bigint profesional_id FK
    }

    TURNO {
        bigint id PK
        timestamptz fecha_hora_inicio
        timestamptz fecha_hora_fin
        varchar estado
        varchar observaciones
        boolean acepta_recordatorio_whatsapp
        boolean recordatorio_enviado
        timestamptz fecha_creacion
        timestamptz fecha_actualizacion
        bigint agenda_id FK
        bigint paciente_id FK
        bigint servicio_id FK
    }
```

## Cardinalidad y reglas de integridad

* `PROFESIONAL` → `AGENDA`: exactamente una agenda por profesional. Lo garantiza el `UNIQUE` en
  `agenda.profesional_id`.
* `PROFESIONAL` → `SERVICIO`: un profesional ofrece varios servicios.
* `AGENDA` → `AGENDA_CONFIG` y `AGENDA` → `EXCEPCION_AGENDA`: composición. Un horario o una excepción
  sin agenda no tiene sentido, por eso `ON DELETE RESTRICT`. La configuración no referencia al
  profesional de forma directa: se llega a través de la agenda.
* `PACIENTE` → `TURNO`, `SERVICIO` → `TURNO`, `AGENDA` → `TURNO`: un turno referencia exactamente un
  paciente, un servicio y una agenda. La baja del paciente, del servicio o de la agenda con turnos
  asociados está bloqueada por `ON DELETE RESTRICT`; la baja lógica se hace con `activo = false`.
* La hora de fin del turno no se guarda calculada en la base: se deriva de
  `servicio.duracion_minutos` en el backend.

## Restricciones que no se ven en el diagrama

| Restricción | Definición |
| --- | --- |
| Horario del turno | `CHECK (fecha_hora_fin > fecha_hora_inicio)` |
| Estados del turno | `CHECK (estado IN ('PENDIENTE','CONFIRMADO','CANCELADO','REALIZADO','AUSENTE'))` |
| Turno activo único | `uq_turno_agenda_inicio_activo`, índice único parcial sobre `(agenda_id, fecha_hora_inicio)` cuando `estado IN ('PENDIENTE','CONFIRMADO')` |
| Franja horaria | `CHECK (hora_fin > hora_inicio)` en `agenda_config` |
| Rango de excepción | `CHECK (fecha_fin >= fecha_inicio)` en `excepcion_agenda` |
| Duración y precio | `CHECK (duracion_minutos > 0)` y `CHECK (precio >= 0)` en `servicio` |
| Última modificación | `trg_turno_fecha_actualizacion` actualiza `turno.fecha_actualizacion` en cada `UPDATE`. No es auditoría: no registra quién cambió ni qué |
| Transiciones de estado | `trg_turno_transicion_estado` (función `valida_turno_transicion_estado`) rechaza con `check_violation` todo cambio de estado fuera de la matriz |
| Vencimiento de reservas | `fn_cerrar_turnos_vencidos()` pasa a `CANCELADO` los `PENDIENTE` cuya `fecha_hora_fin` ya pasó, deja la constancia en `observaciones` y devuelve cuántos cerró |
| Zona horaria | `turno.fecha_hora_inicio`, `turno.fecha_hora_fin`, `turno.fecha_creacion`, `turno.fecha_actualizacion` y `paciente.fecha_creacion` son `TIMESTAMPTZ`. `agenda_config.hora_inicio/hora_fin` siguen siendo `TIME` y `excepcion_agenda.fecha_inicio/fecha_fin` siguen siendo `DATE` |
