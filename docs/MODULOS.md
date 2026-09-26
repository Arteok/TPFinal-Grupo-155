# Módulos a desarrollar — Trabajo Final Integrador

**Proyecto:** Sistema web de gestión de turnos para un consultorio podológico independiente  
**Grupo:** 155 — Ludueña / Mariasch  
**Tutor:** Sergio Andrés Antonini  

## ¿Qué entendemos por módulo?

En este proyecto, un **módulo** es una parte funcional del sistema que agrupa tareas relacionadas y resuelve una necesidad concreta del usuario.

Los módulos definidos a continuación corresponden al alcance del **MVP** y serán la base para organizar el desarrollo del frontend, backend y base de datos.

---

## 1. Gestión de agenda y servicios

Permite al profesional administrar la configuración general de su atención y los servicios ofrecidos.

**Incluye:**

- Inicio de sesión del profesional mediante email y contraseña.
- Configuración de días de atención.
- Configuración de franjas horarias disponibles.
- Registro de excepciones de agenda, por ejemplo días no laborables.
- Alta, modificación y desactivación de servicios.
- Definición de nombre, descripción, duración y precio de cada servicio.

**Entidades relacionadas:** `Profesional`, `Agenda`, `AgendaConfig`, `ExcepcionAgenda`, `Servicio`.

---

## 2. Reserva pública de turnos

Permite al paciente reservar un turno sin necesidad de crear una cuenta, iniciar sesión ni validar su correo electrónico.

**Flujo principal:**

**Servicio → fecha → horario → datos del paciente → confirmación del turno**

**Incluye:**

- Consulta de servicios disponibles.
- Selección del servicio.
- Consulta de fechas y horarios disponibles.
- Ingreso de nombre, apellido y teléfono del paciente.
- Búsqueda de un paciente existente y reutilización de sus datos cuando corresponda.
- Registro de un nuevo paciente cuando no exista uno previo.
- Registro de la reserva.
- Opción para aceptar recordatorios por WhatsApp.

La identificación y posible reutilización de un paciente existente será responsabilidad de la lógica de aplicación. La base de datos no establece actualmente una restricción `UNIQUE` sobre el teléfono.

**Entidades relacionadas:** `Servicio`, `Paciente`, `Turno`, `Agenda`.

---

## 3. Gestión de pacientes y turnos

Permite al profesional administrar los datos básicos de los pacientes y los turnos registrados.

**Incluye:**

- Alta de pacientes.
- Consulta y modificación de nombre, apellido y teléfono.
- Baja lógica del paciente cuando corresponda.
- Consulta del historial de turnos de un paciente.
- Consulta de turnos registrados.
- Confirmación de turnos.
- Cancelación de turnos.
- Reprogramación de turnos.
- Registro de turnos realizados.
- Registro de ausencias.
- Consulta del estado de cada turno.

Los datos del paciente se almacenan en una entidad propia y cada turno mantiene una referencia al paciente correspondiente.

El paciente **no posee usuario, contraseña ni inicio de sesión** dentro del MVP.

Los estados posibles, las transiciones permitidas y el vencimiento de las reservas sin confirmar se detallan en [Reglas de negocio del turno](#reglas-de-negocio-del-turno).

**Entidades relacionadas:** `Paciente`, `Turno`, `EstadoTurno`.

---

## 4. Validaciones de negocio

Agrupa las reglas necesarias para mantener una agenda consistente.

**Incluye:**

- Evitar turnos duplicados.
- Evitar solapamientos entre turnos pendientes o confirmados.
- Verificar que el horario elegido se encuentre dentro de la disponibilidad configurada.
- Verificar las excepciones de agenda.
- Calcular la hora de finalización del turno según la duración del servicio elegido.
- Almacenar la hora de finalización calculada en `turno.fecha_hora_fin`.
- Verificar que el turno completo entre dentro de la franja horaria disponible.
- Validar las transiciones de estado antes de aplicar un cambio.
- Validar que una reprogramación sólo se realice cuando el turno esté en un estado permitido.

La base de datos también posee restricciones para reforzar parte de estas reglas. Los servicios del backend deberán validarlas previamente para devolver mensajes de negocio adecuados al usuario.

**Entidades relacionadas:** `Turno`, `Servicio`, `Agenda`, `AgendaConfig`, `ExcepcionAgenda`.

---

## 5. Recordatorios por WhatsApp

Permite gestionar recordatorios asociados a los turnos.

**Incluye:**

- Registrar si el paciente aceptó recibir recordatorios por WhatsApp.
- Identificar turnos próximos que requieran recordatorio.
- Enviar el recordatorio mediante el servicio de mensajería que se defina.
- Registrar si el recordatorio fue enviado.
- Evitar envíos duplicados mediante el campo `recordatorio_enviado`.

Como contingencia, el sistema podrá listar los recordatorios pendientes para que el profesional pueda enviarlos manualmente si la integración automática no resulta viable dentro del plazo académico.

**Entidades relacionadas:** `Turno`, `Paciente`.

---

## 6. Despliegue

Comprende la publicación y ejecución del sistema en servicios online.

**Incluye:**

- Despliegue del frontend.
- Despliegue del backend.
- Despliegue o conexión de la base de datos PostgreSQL.
- Configuración de variables de entorno.
- Configuración de la comunicación entre frontend, backend y base de datos.

**Tecnologías previstas:** Vercel para frontend y Railway para backend y PostgreSQL.

---

# Reglas de negocio del turno

## Estados

En el backend, los estados se representan mediante el enum `EstadoTurno`.

En PostgreSQL, el estado se almacena como `VARCHAR` y los valores permitidos se restringen mediante un `CHECK`.

| Estado | Significado |
| --- | --- |
| `PENDIENTE` | Reserva creada y todavía no confirmada por el profesional. |
| `CONFIRMADO` | Turno aceptado por el profesional. |
| `CANCELADO` | Turno anulado. |
| `REALIZADO` | Turno atendido y finalizado. |
| `AUSENTE` | El paciente no se presentó a un turno previamente confirmado. |

Los estados `PENDIENTE` y `CONFIRMADO` ocupan disponibilidad dentro de la agenda.

Los estados `CANCELADO`, `REALIZADO` y `AUSENTE` no bloquean disponibilidad para futuras reservas.

## Transiciones permitidas

| Desde | Hacia |
| --- | --- |
| `PENDIENTE` | `CONFIRMADO`, `CANCELADO` |
| `CONFIRMADO` | `REALIZADO`, `AUSENTE`, `CANCELADO` |
| `CANCELADO` | — |
| `REALIZADO` | — |
| `AUSENTE` | — |

Los estados `CANCELADO`, `REALIZADO` y `AUSENTE` son terminales.

La matriz se valida en la base de datos mediante el trigger `trg_turno_transicion_estado`, cuya función asociada es `valida_turno_transicion_estado()`.

El backend también deberá validar estas transiciones antes de ejecutar la modificación, para devolver un mensaje de negocio adecuado en lugar de depender únicamente de una excepción SQL.

### Decisiones sobre cambios de estado

- **No se permite `CONFIRMADO → PENDIENTE`.**
- Un estado terminal no puede volver a abrirse.
- La reprogramación sólo se permite cuando el turno está en estado `PENDIENTE` o `CONFIRMADO`.

## Reprogramación

Reprogramar un turno implica modificar:

- `fecha_hora_inicio`
- `fecha_hora_fin`

La nueva hora de finalización se calcula nuevamente a partir de `servicio.duracion_minutos`.

Antes de efectuar la modificación deberán volver a comprobarse:

- disponibilidad de agenda;
- excepciones;
- solapamientos;
- duración completa del turno;
- estado actual del turno.

## Vencimiento de reservas sin confirmar

Un turno `PENDIENTE` cuya hora de finalización ya pasó se cierra automáticamente como `CANCELADO`.

La operación se encuentra implementada en la base mediante:

`fn_cerrar_turnos_vencidos()`

El backend podrá ejecutar esta función periódicamente mediante una tarea programada.

La función:

- busca turnos vencidos en estado `PENDIENTE`;
- cambia su estado a `CANCELADO`;
- deja constancia en `observaciones`;
- conserva las observaciones existentes;
- devuelve la cantidad de turnos cerrados.

Una vez cancelado, el turno no vuelve a ser procesado por la función.

**Consecuencia asumida:** un turno que nunca fue confirmado y cuya fecha ya pasó queda registrado como `CANCELADO`.

El estado `AUSENTE` queda reservado para un turno que se encontraba previamente `CONFIRMADO` y al cual el paciente no se presentó.

## Cancelación de turnos

La cancelación de un turno libera su disponibilidad.

No se realizará un corrimiento automático de otros turnos ante una cancelación.

Si el profesional desea cubrir un horario liberado, deberá gestionarlo manualmente.

Esta decisión evita modificar automáticamente turnos ya reservados por otros pacientes.

## Paciente como entidad independiente

`Paciente` forma parte del modelo UML y del esquema de base de datos.

Se almacenan:

- `nombre`
- `apellido`
- `telefono`
- `activo`
- `fecha_creacion`

Cada turno referencia al paciente mediante una clave foránea.

Esta estructura evita repetir los datos personales dentro de cada turno y permite consultar el historial asociado a un paciente.

El paciente **no tendrá cuenta, usuario ni contraseña** dentro del MVP.

## Cálculo de duración

Cada `Servicio` define su duración mediante `duracion_minutos`.

Cuando se crea o reprograma un turno, el backend calcula:

`fecha_hora_fin = fecha_hora_inicio + duracion_minutos`

El resultado se almacena en `turno.fecha_hora_fin`.

Esto permite conocer directamente el intervalo ocupado y realizar las validaciones de disponibilidad y solapamiento.

---

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