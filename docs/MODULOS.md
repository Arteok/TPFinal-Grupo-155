# Módulos a desarrollar — Trabajo Final Integrador

**Proyecto:** Sistema web de gestión de turnos para un consultorio podológico independiente  
**Grupo:** 155 — Ludueña / Mariasch  
**Tutor:** Sergio Andrés Antonini  

## ¿Qué entendemos por módulo?

En este proyecto, un **módulo** es una parte funcional del sistema que agrupa tareas relacionadas y resuelve una necesidad concreta del usuario.

Los módulos definidos corresponden al alcance del **MVP** y sirven como base para organizar el desarrollo del frontend, backend y base de datos.

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
- Ingreso de nombre, apellido y teléfono.
- Búsqueda de un paciente existente.
- Registro de un nuevo paciente cuando corresponda.
- Registro de la reserva.
- Opción para aceptar recordatorios por WhatsApp.

La identificación de pacientes existentes será responsabilidad de la lógica de aplicación. Actualmente el teléfono no posee una restricción `UNIQUE` en la base de datos.

**Entidades relacionadas:** `Servicio`, `Paciente`, `Turno`, `Agenda`.

---

## 3. Gestión de pacientes y turnos

Permite al profesional administrar los datos básicos de los pacientes y los turnos registrados.

**Incluye:**

- Alta de pacientes.
- Consulta y modificación de nombre, apellido y teléfono.
- Baja lógica del paciente.
- Consulta del historial de turnos.
- Consulta de turnos registrados.
- Confirmación de turnos.
- Cancelación de turnos.
- Reprogramación de turnos.
- Registro de turnos realizados.
- Registro de ausencias.

Los datos del paciente se almacenan en una entidad propia y cada turno mantiene una referencia al paciente correspondiente.

El paciente **no posee usuario, contraseña ni inicio de sesión** dentro del MVP.

**Entidades relacionadas:** `Paciente`, `Turno`, `EstadoTurno`.

---

## 4. Validaciones de negocio

Agrupa las reglas necesarias para mantener una agenda consistente.

**Incluye:**

- Evitar turnos duplicados.
- Evitar solapamientos entre turnos pendientes o confirmados.
- Verificar la disponibilidad configurada.
- Verificar excepciones de agenda.
- Calcular la hora de finalización según la duración del servicio.
- Almacenar la hora de finalización en `turno.fecha_hora_fin`.
- Verificar que el turno completo entre dentro de la franja disponible.
- Validar las transiciones de estado.
- Validar las reprogramaciones.

Parte de estas reglas también se refuerzan mediante restricciones en PostgreSQL.

**Entidades relacionadas:** `Turno`, `Servicio`, `Agenda`, `AgendaConfig`, `ExcepcionAgenda`.

---

## 5. Recordatorios por WhatsApp

Permite gestionar recordatorios asociados a los turnos.

**Incluye:**

- Registrar si el paciente aceptó recibir recordatorios.
- Identificar turnos próximos.
- Enviar el recordatorio mediante el servicio que se defina.
- Registrar si el recordatorio fue enviado.
- Evitar envíos duplicados.

Si la integración automática no resulta viable dentro del plazo del proyecto, se podrán consultar los recordatorios pendientes para realizar el envío manualmente.

**Entidades relacionadas:** `Turno`, `Paciente`.

---

## 6. Despliegue

Comprende la publicación y ejecución del sistema.

**Incluye:**

- Despliegue del frontend.
- Despliegue del backend.
- Despliegue o conexión de PostgreSQL.
- Configuración de variables de entorno.
- Comunicación entre frontend, backend y base de datos.

**Tecnologías previstas:** Vercel para frontend y Railway para backend y PostgreSQL.

---

# Reglas de negocio del turno

## Estados

En el backend los estados se representan mediante el enum `EstadoTurno`.

En PostgreSQL se almacenan como `VARCHAR` con una restricción `CHECK`.

| Estado | Significado |
| --- | --- |
| `PENDIENTE` | Reserva creada y todavía no confirmada. |
| `CONFIRMADO` | Turno aceptado por el profesional. |
| `CANCELADO` | Turno anulado. |
| `REALIZADO` | Turno atendido y finalizado. |
| `AUSENTE` | El paciente no se presentó a un turno confirmado. |

Los estados `PENDIENTE` y `CONFIRMADO` ocupan disponibilidad.

## Transiciones permitidas

| Desde | Hacia |
| --- | --- |
| `PENDIENTE` | `CONFIRMADO`, `CANCELADO` |
| `CONFIRMADO` | `REALIZADO`, `AUSENTE`, `CANCELADO` |
| `CANCELADO` | — |
| `REALIZADO` | — |
| `AUSENTE` | — |

`CANCELADO`, `REALIZADO` y `AUSENTE` son estados terminales.

La base de datos valida estas transiciones mediante el trigger `trg_turno_transicion_estado`.

## Reprogramación

La reprogramación se permite solamente para turnos `PENDIENTE` o `CONFIRMADO`.

Al reprogramar se vuelven a calcular y validar:

- fecha y hora de inicio;
- fecha y hora de finalización;
- disponibilidad;
- excepciones;
- solapamientos.

## Vencimiento de reservas

Un turno `PENDIENTE` cuya hora de finalización ya pasó puede cerrarse automáticamente como `CANCELADO`.

La base de datos dispone de la función:

`fn_cerrar_turnos_vencidos()`

El estado `AUSENTE` queda reservado para turnos que estaban previamente `CONFIRMADO`.

## Cancelación

La cancelación libera el horario.

No se realizará un corrimiento automático de otros turnos, ya que podría afectar reservas realizadas por otros pacientes.

## Paciente

`Paciente` es una entidad independiente.

Almacena:

- nombre;
- apellido;
- teléfono;
- estado activo;
- fecha de creación.

El paciente no posee cuenta ni inicio de sesión.

## Duración del turno

Cada servicio define su duración mediante `duracion_minutos`.

El backend calcula:

`fecha_hora_fin = fecha_hora_inicio + duracion_minutos`

y almacena el resultado en `turno.fecha_hora_fin`.

---

## Fuera del alcance del MVP

Las funcionalidades que no forman parte de esta primera versión se encuentran detalladas en el [README](../README.md#fuera-del-alcance-del-mvp).