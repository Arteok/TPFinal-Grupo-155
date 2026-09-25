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
- Manejo de estados: `PENDIENTE`, `CONFIRMADO`, `CANCELADO`, `REALIZADO` y `AUSENTE`.
- Aplicar la matriz de transiciones: `PENDIENTE → CONFIRMADO, CANCELADO` y `CONFIRMADO → REALIZADO, AUSENTE, CANCELADO`. `CANCELADO`, `REALIZADO` y `AUSENTE` son terminales.
- Cierre automático de las reservas `PENDIENTE` que nunca se confirmaron, llamando a `fn_cerrar_turnos_vencidos()` desde una tarea programada.

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

## Fuera del alcance del MVP

En esta primera versión no se desarrollarán:

- Pasarelas de pago online.
- Facturación electrónica.
- Historia clínica integral.
- Aplicaciones móviles nativas.
- Cuenta o inicio de sesión para pacientes.
- Inicio de sesión con Google para pacientes.
- Códigos de descuento.
- Reserva de varios servicios dentro de un mismo turno.
- Múltiples consultorios o arquitectura multi-tenant.

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
