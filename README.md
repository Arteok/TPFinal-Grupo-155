# TPFinal-Grupo-155
Trabajo Final Integrador — Grupo 155

## Sistema de Gestión de Turnos

Aplicación web para la gestión de turnos de un profesional independiente. Permite al profesional administrar su agenda y los servicios que ofrece, y a los pacientes reservar un turno de forma sencilla, sin necesidad de crear una cuenta ni validar su correo electrónico.

El flujo principal de reserva es:

**Servicio → fecha → horario → datos del paciente → confirmación del turno.**

## Módulos del MVP

1. **Gestión de agenda y servicios** — el profesional configura sus días y horarios de atención, las excepciones de agenda y los servicios que ofrece, con nombre, descripción, duración y precio.
2. **Reserva pública de turnos** — el paciente consulta los servicios, ve los horarios disponibles, elige fecha y horario, ingresa sus datos y reserva el turno.
3. **Gestión de pacientes y turnos** — el profesional consulta y modifica los datos de los pacientes, ve su historial de turnos, y crea, reprograma, cancela o cambia el estado de un turno.
4. **Validaciones de negocio** — el sistema controla la disponibilidad, evita superposiciones y turnos duplicados, y calcula la hora de finalización a partir de la duración del servicio.
5. **Recordatorios por WhatsApp** — recordatorios de los turnos cercanos para los pacientes que acepten recibirlos.
6. **Despliegue** — publicación online del frontend, del backend y de la base de datos.

El detalle de cada módulo, las reglas de negocio del turno y la tabla de aprobación del tutor y el comité están en [`docs/MODULOS.md`](docs/MODULOS.md).

## Estados de los turnos

| Estado | Significado |
| --- | --- |
| `PENDIENTE` | Reserva creada por el paciente y todavía sin confirmar. |
| `CONFIRMADO` | Turno aceptado por el profesional. |
| `CANCELADO` | Turno cancelado; el horario queda libre. |
| `REALIZADO` | Turno atendido. |
| `AUSENTE` | El paciente no se presentó. |

Los turnos `PENDIENTE` y `CONFIRMADO` ocupan el horario disponible; los demás lo liberan.

## Fuera del alcance del MVP

No forman parte de esta primera versión:

- Pasarelas de pago online.
- Facturación electrónica.
- Historia clínica integral o almacenamiento de información clínica compleja.
- Aplicaciones móviles nativas.
- Cuenta o inicio de sesión para pacientes.
- Inicio de sesión con Google para pacientes.
- Códigos de descuento.
- Reserva de varios servicios dentro de un mismo turno.
- Modelo comercial de suscripciones para múltiples consultorios.
- Arquitectura multi-tenant o gestión de múltiples organizaciones en la primera versión.

## Estructura del proyecto

| Carpeta | Contenido |
| --- | --- |
| `docs/` | Propuesta, modelo UML, diagrama entidad-relación, módulos y arquitectura. |
| `database/` | Esquema relacional de PostgreSQL en `schema.sql`. |
| `backend/` | API REST del sistema. |
| `frontend/` | Interfaz web del sistema. |

## Documentación

- [`docs/MODULOS.md`](docs/MODULOS.md) — los seis módulos del MVP, las reglas de negocio del turno y la tabla de aprobación.
- [`docs/ARQUITECTURA.md`](docs/ARQUITECTURA.md) — arquitectura por capas y decisiones del modelo de datos.
- [`docs/Diagrama_Clases_Actualizado_V2_Entrega.md`](docs/Diagrama_Clases_Actualizado_V2_Entrega.md) — diagrama de clases.
- [`docs/### Modelo de Datos (UML).md`](docs/###%20Modelo%20de%20Datos%20%28UML%29.md) — descripción del modelo de datos.
- [`docs/DER_Actualizado.md`](docs/DER_Actualizado.md) — diagrama entidad-relación y restricciones del modelo.
- Propuesta del proyecto: [versión vigente en Markdown](docs/Trabajo%20Final%20IntegradorV2.md) · [original en Word](docs/Trabajo%20Final%20IntegradorV2.docx)

## Tecnologías

| Componente | Tecnologías |
| --- | --- |
| Backend | Java · Spring Boot · Gradle |
| Frontend | React · TypeScript · Vite · HTML5 · CSS3 |
| Base de datos | PostgreSQL |
| Comunicación | API REST |
| Despliegue | Vercel (frontend) · Railway (backend y PostgreSQL) |

La justificación de cada elección está en el [§5 de la propuesta](docs/Trabajo%20Final%20IntegradorV2.md).

## Integrantes

- Pablo Mariasch
- Bruno Ludueña

## Estado

Etapa de diseño (2.ª entrega). El desarrollo del MVP comienza tras la aprobación del tutor y del comité.
