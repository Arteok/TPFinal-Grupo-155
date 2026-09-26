# TPFinal-Grupo-155

Trabajo Final Integrador — Grupo 155

## Sistema de Gestión de Turnos

Aplicación web para la gestión de turnos de un profesional independiente.

El sistema permite al profesional administrar su agenda y los servicios que ofrece, mientras que los pacientes pueden reservar un turno de forma sencilla, sin necesidad de crear una cuenta, iniciar sesión ni validar su correo electrónico.

El flujo principal de reserva es:

**Servicio → fecha → horario → datos del paciente → confirmación del turno**

---

## Módulos del MVP

1. **Gestión de agenda y servicios** — el profesional configura sus días y horarios de atención, excepciones de agenda y los servicios ofrecidos.

2. **Reserva pública de turnos** — el paciente consulta servicios y disponibilidad, ingresa sus datos y realiza una reserva sin necesidad de crear una cuenta.

3. **Gestión de pacientes y turnos** — el profesional consulta y modifica datos básicos de pacientes, revisa su historial y administra los turnos registrados.

4. **Validaciones de negocio** — controla disponibilidad, excepciones, solapamientos, duración de los servicios y transiciones de estado.

5. **Recordatorios por WhatsApp** — gestiona recordatorios para los pacientes que acepten recibirlos.

6. **Despliegue** — publicación del frontend, backend y base de datos.

El detalle completo de los módulos y sus reglas de negocio se encuentra en:

[`docs/MODULOS.md`](docs/MODULOS.md)

---

## Estados de los turnos

| Estado | Significado |
| --- | --- |
| `PENDIENTE` | Reserva creada y todavía no confirmada por el profesional. |
| `CONFIRMADO` | Turno aceptado por el profesional. |
| `CANCELADO` | Turno anulado. |
| `REALIZADO` | Turno atendido y finalizado. |
| `AUSENTE` | El paciente no se presentó a un turno previamente confirmado. |

Las transiciones permitidas son:

- `PENDIENTE` → `CONFIRMADO`, `CANCELADO`
- `CONFIRMADO` → `REALIZADO`, `AUSENTE`, `CANCELADO`

`CANCELADO`, `REALIZADO` y `AUSENTE` son estados terminales.

Los turnos `PENDIENTE` y `CONFIRMADO` ocupan disponibilidad dentro de la agenda.

---

## Pacientes

`Paciente` forma parte del modelo como una entidad independiente.

Se almacenan sus datos básicos:

- nombre;
- apellido;
- teléfono;
- estado activo;
- fecha de creación.

Cada turno se vincula con un paciente.

El paciente **no posee cuenta, usuario ni contraseña** dentro del MVP.

---

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
- Arquitectura multi-tenant o gestión de múltiples organizaciones.

---

## Estructura del proyecto

| Carpeta | Contenido |
| --- | --- |
| `docs/` | Propuesta, UML, DER, módulos, arquitectura e informes. |
| `database/` | Esquema relacional PostgreSQL definido en `schema.sql`. |
| `backend/` | API REST del sistema. |
| `frontend/` | Interfaz web del sistema. |

---

## Documentación

- [`docs/INFORME_AVANCE.md`](docs/INFORME_AVANCE.md) — estado de la segunda entrega, riesgos y próximos pasos.
- [`docs/MODULOS.md`](docs/MODULOS.md) — módulos del MVP y reglas de negocio.
- [`docs/ARQUITECTURA.md`](docs/ARQUITECTURA.md) — arquitectura general del sistema.
- [`docs/FLUJO_GIT.md`](docs/FLUJO_GIT.md) — estrategia de ramas, commits e integración utilizada por el equipo.
- [`docs/Diagrama_Clases_Actualizado_V2_Entrega.md`](docs/Diagrama_Clases_Actualizado_V2_Entrega.md) — diagrama de clases UML.
- [`docs/### Modelo de Datos (UML).md`](docs/###%20Modelo%20de%20Datos%20%28UML%29.md) — descripción del modelo de datos.
- [`docs/DER_Actualizado.md`](docs/DER_Actualizado.md) — diagrama entidad-relación.
- [`database/schema.sql`](database/schema.sql) — implementación del esquema PostgreSQL.
- Propuesta del proyecto: [`docs/Trabajo Final IntegradorV2.md`](docs/Trabajo%20Final%20IntegradorV2.md)
- Documento original: [`docs/Trabajo Final IntegradorV2.docx`](docs/Trabajo%20Final%20IntegradorV2.docx)

---

## Tecnologías

| Componente | Tecnologías |
| --- | --- |
| Backend | Java · Spring Boot · Gradle |
| Frontend | React · TypeScript · Vite |
| Base de datos | PostgreSQL |
| Comunicación | API REST |
| Frontend hosting | Vercel |
| Backend / PostgreSQL | Railway |

---

## Control de versiones

El proyecto utiliza Git y GitHub.

La rama `main` contiene la versión estable del proyecto.

Las modificaciones se realizan mediante ramas independientes según la tarea o funcionalidad.

Para la documentación de esta entrega se utiliza:

`docs/segunda-entrega`

El flujo de trabajo completo se encuentra documentado en:

[`docs/FLUJO_GIT.md`](docs/FLUJO_GIT.md)

---

## Integrantes

- Pablo Mariasch
- Bruno Ludueña

---

## Estado actual

El proyecto se encuentra en la etapa de **diseño y modelado correspondiente a la segunda entrega**.

Actualmente se encuentran definidos:

- propuesta del proyecto;
- módulos del MVP;
- arquitectura;
- diagrama de clases;
- diagrama entidad-relación;
- esquema PostgreSQL;
- reglas principales de negocio;
- flujo de trabajo con Git.

El desarrollo funcional del backend y frontend comenzará luego de cerrar las validaciones y aprobaciones correspondientes.