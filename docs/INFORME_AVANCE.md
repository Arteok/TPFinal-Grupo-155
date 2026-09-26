# Informe de avance — 2.ª entrega

**Proyecto:** Sistema web de gestión de turnos para un consultorio podológico independiente  
**Grupo:** 155 — Ludueña / Mariasch  
**Tutor:** Sergio Andrés Antonini  
**Etapa:** Diseño y modelado (2.ª entrega del Trabajo Final Integrador)  
**Fecha:** 26 de septiembre de 2026

## Qué se entregó en esta etapa

| Entregable | Estado | Ubicación |
| --- | --- | --- |
| Propuesta del proyecto | Completado | [`docs/Trabajo Final IntegradorV2.md`](Trabajo%20Final%20IntegradorV2.md) |
| Modelo de datos en UML | Completado | [`docs/Diagrama_Clases_Actualizado_V2_Entrega.md`](Diagrama_Clases_Actualizado_V2_Entrega.md) y [`docs/### Modelo de Datos (UML).md`](###%20Modelo%20de%20Datos%20%28UML%29.md) |
| Diagrama entidad-relación | Completado | [`docs/DER_Actualizado.md`](DER_Actualizado.md) |
| Esquema de la base de datos | Completado | [`database/schema.sql`](../database/schema.sql) |
| Listado de módulos del MVP | Completado | [`docs/MODULOS.md`](MODULOS.md) |
| Arquitectura del sistema | Completado | [`docs/ARQUITECTURA.md`](ARQUITECTURA.md) |
| Estructura inicial de backend y frontend | Completado | [`backend/README.md`](../backend/README.md) y [`frontend/README.md`](../frontend/README.md) |
| Mockups o prototipos de pantallas | **Pendiente** | — |

## Avance sobre el plan de trabajo

El plan de trabajo con sus cuatro fases está en el [§8 de la propuesta](Trabajo%20Final%20IntegradorV2.md).

| Fase | Avance |
| --- | --- |
| Fase 1 — Diseño y modelado | **En curso.** Modelo, módulos, esquema y arquitectura listos. Falta validar el alcance con la profesional y agregar los prototipos de pantallas. |
| Fase 2 — Backend | No iniciada. Comienza tras la aprobación del alcance. |
| Fase 3 — Frontend | No iniciada. Comienza tras la aprobación del alcance. |
| Fase 4 — Integración y cierre | No iniciada. |

## Riesgos vigentes

| Riesgo | Mitigación |
| --- | --- |
| El equipo son dos integrantes. | Plan de trabajo acotado al MVP: lo esencial primero y las funciones comerciales postergadas. |
| Dependencia de servicios externos: mensajería de WhatsApp, Vercel y Railway. | El módulo de recordatorios tiene plan de contingencia de envío manual. La profesional ya usa una herramienta digital, así que la adopción no parte de cero. |
| Cambios en las condiciones de las APIs y de las plataformas PaaS. | Los componentes Deployment se pueden cambiar sin afectar el modelo de datos. El análisis completo está en la [matriz FODA de la propuesta](Trabajo%20Final%20IntegradorV2.md). |
| Incorporar funciones innecesarias al alcance. | Validar el alcance y los módulos con la profesional antes de iniciar el desarrollo. |

## Próximos pasos

1. Validar el alcance y los módulos definidos con la profesional.
2. Agregar los mockups o prototipos de pantallas, que son el último entregable pendiente de la Fase 1.
3. Obtener la aprobación explícita del tutor y del comité sobre el alcance de los módulos. El estado de esa aprobación está en la [tabla de aprobación de módulos](MODULOS.md), con las seis filas en estado *Pendiente*.
4. Iniciar la Fase 2 (backend) una vez aprobado el alcance.

## Estado del proyecto

El proyecto se encuentra en etapa de diseño. No hay código de aplicación desarrollado todavía: las carpetas `backend/` y `frontend/` tienen únicamente su estructura inicial y su documentación.
