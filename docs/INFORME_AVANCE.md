# Informe de avance — 2.ª entrega

**Proyecto:** Sistema web de gestión de turnos para un consultorio podológico independiente  
**Grupo:** 155 — Ludueña / Mariasch  
**Tutor:** Sergio Andrés Antonini  
**Etapa:** Diseño y modelado  
**Fecha:** 26 de septiembre de 2026

## Qué se completó en esta etapa

| Entregable | Estado | Ubicación |
| --- | --- | --- |
| Propuesta del proyecto | Completado | [`Trabajo Final IntegradorV2.md`](Trabajo%20Final%20IntegradorV2.md) |
| Modelo de datos UML | Completado | [`Diagrama_Clases_Actualizado_V2_Entrega.md`](Diagrama_Clases_Actualizado_V2_Entrega.md) |
| Diagrama entidad-relación | Completado | [`DER_Actualizado.md`](DER_Actualizado.md) |
| Esquema PostgreSQL | Completado | [`../database/schema.sql`](../database/schema.sql) |
| Módulos del MVP | Completado | [`MODULOS.md`](MODULOS.md) |
| Arquitectura | Completado | [`ARQUITECTURA.md`](ARQUITECTURA.md) |
| Flujo de trabajo con Git | Completado | [`FLUJO_GIT.md`](FLUJO_GIT.md) |
| Modelado con apoyo de IA | Completado | [`MODELADO_CON_IA.md`](MODELADO_CON_IA.md) |
| Estructura inicial backend/frontend | Completado | `backend/` y `frontend/` |
| Mockups o prototipos | Pendiente | — |

## Avance del proyecto

| Fase | Avance |
| --- | --- |
| Fase 1 — Diseño y modelado | **En curso.** Se encuentran definidos el modelo de datos, DER, diagrama de clases, módulos, arquitectura y esquema PostgreSQL. Restan los prototipos de interfaz. |
| Fase 2 — Backend | No iniciada. |
| Fase 3 — Frontend | No iniciada. |
| Fase 4 — Integración y cierre | No iniciada. |

## Estado del diseño

El modelo contempla las siguientes entidades principales:

- `Profesional`
- `Agenda`
- `AgendaConfig`
- `ExcepcionAgenda`
- `Servicio`
- `Paciente`
- `Turno`

El paciente es una entidad independiente pero no posee cuenta ni inicio de sesión.

El flujo de reserva definido es:

**Servicio → fecha → horario → datos del paciente → confirmación del turno**

Los estados posibles son:

- `PENDIENTE`
- `CONFIRMADO`
- `CANCELADO`
- `REALIZADO`
- `AUSENTE`

Las principales transiciones son:

- `PENDIENTE` → `CONFIRMADO`, `CANCELADO`
- `CONFIRMADO` → `REALIZADO`, `AUSENTE`, `CANCELADO`

La hora de finalización se calcula a partir de la duración del servicio y se almacena en el turno.

## Control de versiones

El proyecto utiliza Git y GitHub.

Se trabaja con una rama principal `main` y ramas independientes para realizar cambios sin modificar directamente la versión estable.

Durante esta etapa se utilizaron ramas de documentación, commits descriptivos y Pull Requests para integrar los cambios.

El flujo utilizado se encuentra en [`FLUJO_GIT.md`](FLUJO_GIT.md).

## Modelado

Durante el diseño se utilizaron herramientas de apoyo para revisar las entidades, relaciones y reglas del sistema.

El modelo fue ajustado de forma iterativa hasta mantener consistencia entre:

- diagrama de clases;
- DER;
- esquema PostgreSQL;
- reglas de negocio.

El proceso se resume en [`MODELADO_CON_IA.md`](MODELADO_CON_IA.md).

## Riesgos vigentes

| Riesgo | Mitigación |
| --- | --- |
| El equipo está compuesto por dos integrantes. | Mantener el alcance acotado al MVP. |
| Dependencia de WhatsApp u otro servicio de mensajería. | Mantener una alternativa de envío manual. |
| Dependencia de Vercel y Railway. | Mantener separados frontend, backend y base de datos. |
| Incorporación de funciones fuera del alcance. | Mantener definido el MVP y postergar funciones secundarias. |

## Próximos pasos

1. Finalizar la documentación correspondiente a la etapa de diseño.
2. Preparar los mockups o prototipos de las pantallas.
3. Revisar el alcance definido para el MVP.
4. Iniciar la implementación del backend.
5. Continuar posteriormente con el frontend.

## Estado actual

El proyecto se encuentra en etapa de diseño y modelado.

Actualmente están definidos el modelo de datos, arquitectura, módulos, reglas de negocio y esquema de base de datos.

Todavía no se inició el desarrollo funcional del backend ni del frontend.

El siguiente paso será finalizar la etapa de diseño y comenzar la implementación del MVP.