# Módulos a desarrollar — Trabajo Final Integrador

**Proyecto:** Sistema web de gestión de turnos para un consultorio podológico independiente  
**Grupo:** 155 — Ludueña / Mariasch  
**Tutor:** Sergio Andrés Antonini  

## ¿Qué entendemos por módulo?

En este proyecto, un **módulo** es una parte funcional del sistema que agrupa tareas relacionadas y resuelve una necesidad concreta del usuario. Los módulos definidos a continuación corresponden al alcance del **MVP** y serán la base para organizar el desarrollo del frontend, backend y base de datos.

---

## 1. Gestión de agenda y servicios

Permite a la profesional administrar la configuración general de su atención.

**Incluye:**
- Inicio de sesión de la profesional mediante email y contraseña.
- Configuración de días de atención.
- Configuración de franjas horarias disponibles.
- Registro de excepciones de agenda, por ejemplo días no laborables.
- Alta, modificación y desactivación de servicios.
- Definición de nombre, descripción, duración y precio de cada servicio.

**Entidades relacionadas:** `Profesional`, `Agenda`, `AgendaConfig`, `ExcepcionAgenda`, `Servicio`.

---

## 2. Reserva pública de turnos

Permite al paciente reservar un turno sin necesidad de crear una cuenta ni validar su correo electrónico.

**Flujo principal:**

**Servicio → fecha → horario → datos del paciente → confirmación del turno**

**Incluye:**
- Consulta de servicios disponibles.
- Selección del servicio.
- Consulta de fechas y horarios disponibles.
- Ingreso de nombre, apellido y teléfono del paciente.
- Registro de un `Paciente` nuevo o reutilización del registro existente cuando los datos ya están cargados, para no duplicar la persona.
- Registro de la reserva.
- Opción para aceptar recordatorios por WhatsApp.

**Entidades relacionadas:** `Servicio`, `Paciente`, `Turno`, `Agenda`.

---

## 3. Gestión de pacientes y turnos

Permite administrar los datos básicos de los pacientes y los turnos registrados.

**Incluye:**
- Alta de pacientes.
- Consulta y modificación de nombre, apellido y teléfono.
- Baja lógica del paciente cuando corresponda.
- Consulta del historial de turnos de un paciente.
- Consulta de turnos reservados.
- Confirmación de turnos.
- Cancelación de turnos.
- Reprogramación de turnos.
- Registro de turnos realizados y de ausencias.
- Los datos del paciente se guardan una sola vez en una entidad propia, lo que permite consultar su historial sin repetirlos en cada reserva.

Los estados posibles, las transiciones permitidas, el vencimiento de las reservas sin confirmar y el criterio de cancelación están en [Reglas de negocio del turno](#reglas-de-negocio-del-turno).

**Entidades relacionadas:** `Paciente`, `Turno`, `EstadoTurno`.

---

## 4. Validaciones de negocio

Agrupa las reglas necesarias para mantener una agenda consistente.

**Incluye:**
- Evitar turnos duplicados.
- Evitar solapamientos entre turnos pendientes o confirmados.
- Verificar que el horario elegido se encuentre dentro de la disponibilidad de la agenda.
- Verificar excepciones de agenda.
- Calcular la hora de finalización del turno según la duración del servicio elegido.
- Verificar que el turno completo entre dentro del horario disponible.
- Validar la matriz de transiciones de estado antes de aplicar el cambio, para devolver un mensaje de negocio en lugar de depender de la excepción del trigger `trg_turno_transicion_estado`.

**Entidades relacionadas:** `Turno`, `Servicio`, `Agenda`, `AgendaConfig`, `ExcepcionAgenda`.

---

## 5. Recordatorios por WhatsApp

Permite gestionar los recordatorios de los turnos reservados.

**Incluye:**
- Registrar si el paciente aceptó recibir recordatorios por WhatsApp.
- Generar recordatorios de turnos próximos.
- Enviar el recordatorio mediante el servicio de mensajería que se defina.
- Registrar si el recordatorio fue enviado.
- Como contingencia, listar los recordatorios pendientes para que la profesional pueda enviarlos manualmente si la integración automática no resulta viable dentro del plazo académico.

**Entidades relacionadas:** `Turno`, `Paciente`.

---

## 6. Despliegue

Permite publicar y ejecutar el sistema en servicios online.

**Incluye:**
- Despliegue del frontend.
- Despliegue del backend.
- Despliegue o conexión de la base de datos PostgreSQL.
- Configuración necesaria para comunicar frontend, backend y base de datos.

**Tecnologías previstas:** Vercel para frontend y Railway para backend/PostgreSQL.

---

## Reglas de negocio del turno

### Estados

| Estado | Significado |
| --- | --- |
| `PENDIENTE` | Reserva creada por el paciente y todavía no confirmada por la profesional. |
| `CONFIRMADO` | Turno aceptado por la profesional. |
| `CANCELADO` | Turno anulado y liberado para una nueva reserva. |
| `REALIZADO` | Turno atendido y finalizado. |
| `AUSENTE` | El paciente no se presentó a la hora reservada. |

Solo `PENDIENTE` y `CONFIRMADO` ocupan el horario: los demás estados lo liberan.

### Transiciones permitidas

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

### Vencimiento de reservas sin confirmar

Un turno `PENDIENTE` cuya hora de fin ya pasó se cierra automáticamente como `CANCELADO`. La operación la realiza la función `fn_cerrar_turnos_vencidos()`, que el backend invoca de forma periódica (`@Scheduled`) y que devuelve cuántos turnos cerró. El cierre deja constancia en `observaciones` sin pisar lo que ya hubiera, y la operación es idempotente.

**Consecuencia asumida:** al cerrar al pasar la hora, un turno pasado ya no se puede confirmar. En el historial del paciente, un turno que nunca fue confirmado figura como `CANCELADO` y nunca como `AUSENTE`; `AUSENTE` queda reservado para los turnos que estaban `CONFIRMADO` y el paciente no se presentó.

### Cancelación de turnos

Se descarta el corrimiento automático de turnos ante una cancelación. Si el profesional necesita cubrir un turno liberado, lo gestiona manualmente contactando a otro paciente, ya que mover turnos automáticamente podría afectar la organización de los demás pacientes.

### Paciente como entidad independiente

`Paciente` forma parte del modelo UML y del esquema de base de datos. Se guardan sus datos básicos (`nombre`, `apellido`, `telefono`) en una tabla propia y los turnos se vinculan mediante una clave foránea.

Esta decisión agrega algo de trabajo al MVP porque requiere alta, búsqueda y actualización de pacientes, pero evita repetir datos en cada turno y permite contar con un historial por paciente. El paciente no tendrá usuario ni contraseña en esta primera versión.

## Fuera del alcance del MVP

Lo que no se desarrolla en esta primera versión está detallado en el [README](../README.md#fuera-del-alcance-del-mvp).

---

## Aprobación de módulos

La lista de módulos deberá ser revisada y aprobada explícitamente primero por el tutor y luego por el comité de Trabajo Final antes de tomarla como alcance definitivo del desarrollo.

| Módulo | Tutor (fecha/medio) | Comité (fecha) |
| --- | --- | --- |
| 1. Gestión de agenda y servicios | Pendiente | Pendiente |
| 2. Reserva pública de turnos | Pendiente | Pendiente |
| 3. Gestión de pacientes y turnos | Pendiente | Pendiente |
| 4. Validaciones de negocio | Pendiente | Pendiente |
| 5. Recordatorios por WhatsApp | Pendiente | Pendiente |
| 6. Despliegue | Pendiente | Pendiente |
