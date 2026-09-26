# Informe de avance — 2.ª entrega

**Proyecto:** Sistema web de gestión de turnos para un consultorio podológico independiente  
**Grupo:** 155 — Ludueña / Mariasch  
**Tutor:** Sergio Andrés Antonini  
**Etapa:** Diseño y modelado (2.ª entrega del Trabajo Final Integrador)  
**Fecha:** 26 de septiembre de 2026

## Qué se completó en esta etapa

| Entregable | Estado | Ubicación |
| --- | --- | --- |
| Propuesta del proyecto | Completado | [`docs/Trabajo Final IntegradorV2.md`](Trabajo%20Final%20IntegradorV2.md) |
| Modelo de datos en UML | Completado | [`docs/Diagrama_Clases_Actualizado_V2_Entrega.md`](Diagrama_Clases_Actualizado_V2_Entrega.md) y [`docs/### Modelo de Datos (UML).md`](###%20Modelo%20de%20Datos%20%28UML%29.md) |
| Diagrama entidad-relación | Completado | [`docs/DER_Actualizado.md`](DER_Actualizado.md) |
| Esquema de base de datos PostgreSQL | Completado | [`database/schema.sql`](../database/schema.sql) |
| Listado de módulos del MVP | Completado | [`docs/MODULOS.md`](MODULOS.md) |
| Arquitectura del sistema | Completado | [`docs/ARQUITECTURA.md`](ARQUITECTURA.md) |
| Flujo de trabajo con Git | Completado | [`docs/FLUJO_GIT.md`](FLUJO_GIT.md) |
| Estructura inicial de backend y frontend | Completado | [`backend/README.md`](../backend/README.md) y [`frontend/README.md`](../frontend/README.md) |
| Mockups o prototipos de pantallas | Pendiente | — |
| Aprobación del tutor | Pendiente | — |
| Aprobación del comité | Pendiente | — |

---

## Avance sobre el plan de trabajo

El plan de trabajo se encuentra definido en la propuesta del proyecto.

| Fase | Avance |
| --- | --- |
| Fase 1 — Diseño y modelado | **En curso.** Se encuentran definidos el modelo de datos, DER, diagrama de clases, módulos, arquitectura y esquema PostgreSQL. Restan los prototipos de interfaz y las aprobaciones correspondientes. |
| Fase 2 — Backend | No iniciada. Se iniciará luego de la aprobación del alcance. |
| Fase 3 — Frontend | No iniciada. |
| Fase 4 — Integración y cierre | No iniciada. |

---

## Estado del diseño

El modelo actual contempla las siguientes entidades principales:

- `Profesional`
- `Agenda`
- `AgendaConfig`
- `ExcepcionAgenda`
- `Servicio`
- `Paciente`
- `Turno`

El paciente forma parte del modelo como entidad independiente, pero **no tendrá cuenta, usuario ni contraseña** dentro del MVP.

El flujo público previsto para realizar una reserva es:

**Servicio → fecha → horario → datos del paciente → confirmación del turno**

Los turnos poseen los siguientes estados:

- `PENDIENTE`
- `CONFIRMADO`
- `CANCELADO`
- `REALIZADO`
- `AUSENTE`

Las transiciones principales son:

- `PENDIENTE` → `CONFIRMADO`, `CANCELADO`
- `CONFIRMADO` → `REALIZADO`, `AUSENTE`, `CANCELADO`

Los estados `CANCELADO`, `REALIZADO` y `AUSENTE` son terminales.

La hora de finalización del turno se calcula en el backend a partir de la duración del servicio y se almacena en `turno.fecha_hora_fin`.

---

## Módulos definidos para el MVP

Se definieron seis módulos:

1. Gestión de agenda y servicios.
2. Reserva pública de turnos.
3. Gestión de pacientes y turnos.
4. Validaciones de negocio.
5. Recordatorios por WhatsApp.
6. Despliegue.

El detalle completo se encuentra en [`MODULOS.md`](MODULOS.md).

La lista deberá contar con aprobación explícita del tutor y posteriormente del comité antes de considerarse definitiva.

---

## Control de versiones

El proyecto utiliza Git y GitHub para el control de versiones.

La rama `main` se reserva para la versión estable del proyecto y los cambios se realizan mediante ramas específicas según la tarea.

Para esta etapa se utiliza:

`docs/segunda-entrega`

La estrategia definida contempla:

- ramas independientes para cada tarea o funcionalidad;
- commits claros y descriptivos;
- publicación de las ramas en el repositorio remoto;
- integración mediante Pull Request;
- merge hacia `main` una vez revisados los cambios.

El flujo completo se encuentra documentado en [`FLUJO_GIT.md`](FLUJO_GIT.md).

---

## Riesgos vigentes

| Riesgo | Mitigación |
| --- | --- |
| El equipo está compuesto por dos integrantes. | Mantener el alcance acotado al MVP y priorizar las funcionalidades esenciales. |
| Dependencia de un servicio externo para WhatsApp. | Mantener como contingencia la consulta de recordatorios pendientes para permitir su envío manual. |
| Dependencia de Vercel y Railway para el despliegue. | Mantener frontend, backend y base de datos desacoplados para permitir cambios de plataforma si fueran necesarios. |
| Cambios en las APIs o servicios externos. | Mantener las integraciones separadas de la lógica principal del sistema. |
| Incorporación de funcionalidades fuera del alcance. | Mantener explícita la sección de funcionalidades fuera del MVP y validar los módulos antes de iniciar el desarrollo. |
| Falta de aprobación formal del alcance. | Obtener primero la aprobación del tutor y posteriormente la del comité antes de avanzar con el desarrollo del MVP. |

---

## Próximos pasos

1. Finalizar la documentación correspondiente a la etapa de diseño.
2. Preparar los mockups o prototipos de las pantallas previstas.
3. Revisar el alcance definido con la profesional.
4. Obtener la aprobación explícita del tutor sobre los módulos y el modelo presentado.
5. Obtener posteriormente la aprobación del comité de Trabajo Final.
6. Integrar los cambios de la rama `docs/segunda-entrega` mediante Pull Request hacia `main`.
7. Iniciar la implementación del backend una vez aprobado el alcance.

---

## Estado actual del proyecto

El proyecto se encuentra en **etapa de diseño y modelado**.

Actualmente están definidos:

- alcance del MVP;
- módulos funcionales;
- arquitectura;
- modelo de clases;
- modelo entidad-relación;
- esquema PostgreSQL;
- reglas principales de negocio;
- estados y transiciones de los turnos;
- estrategia de control de versiones.

Todavía no se inició el desarrollo funcional del backend ni del frontend. Las carpetas correspondientes contienen únicamente la estructura inicial y su documentación.

El siguiente paso del proyecto será cerrar la etapa de diseño mediante las validaciones y aprobaciones correspondientes antes de comenzar la implementación.