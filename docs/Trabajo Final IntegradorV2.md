# Trabajo Final Integrador

Propuesta de proyecto y definición técnica

Sistema web de gestión de turnos para un consultorio podológico independiente

Alumnos:<br><br>LUDUEÑA BRUNO <br>MARIASCH PABLO

Grupo: 155<br><br>Tutor/a: Sergio Andrés Antonini

Cliente / caso relevado: Consultorio podológico independiente<br><br>Repositorio GitHub: [Repositorio TPFinal-Grupo-155](https://github.com/Arteok/TPFinal-Grupo-155/tree/main)

Fecha de entrega: 30/08/2026

# 1. Contexto, entrevistas y relevamiento

El proyecto surge a partir del relevamiento realizado con una profesional de la podología que trabaja de manera independiente. En una primera etapa gestionaba sus turnos en papel y posteriormente adoptó una plataforma web comercial para administrar la agenda.

La entrevista permitió identificar beneficios concretos de la digitalización —principalmente el ahorro de tiempo y los recordatorios de turnos—, pero también problemas de costo, usabilidad y dependencia del soporte técnico que justifican analizar una alternativa más simple y ajustada al contexto del consultorio.

## 1.1. Problemas detectados

**Profesional: **la plataforma actual mejoró la gestión respecto del registro en papel, pero presenta un costo mensual considerado elevado. Además, la configuración de días y horarios disponibles resulta poco intuitiva: la profesional debe navegar por distintas opciones y, en ocasiones, solicitar ayuda al soporte para completar cambios en su agenda.

**Pacientes: **el proceso de reserva es relativamente sencillo, pero los pacientes nuevos deben validar la operación mediante un código enviado por correo electrónico. Ese paso agrega fricción y puede generar confusión, especialmente en personas adultas mayores.

## 1.2. Flujo de trabajo actual

**Profesional: **ingresa al sistema → intenta configurar la agenda → encuentra una interfaz poco clara → en algunos casos depende del soporte técnico para completar la configuración.

**Paciente: **selecciona un turno en la web → el sistema solicita validación por correo electrónico → debe salir del flujo para buscar un código → vuelve al sitio → confirma la reserva → recibe recordatorio por WhatsApp.

## 1.3. Actores y necesidades

| Actor | Necesidad principal | Resultado esperado |
| --- | --- | --- |
| Profesional | Autogestionar días, horarios y excepciones de forma rápida e intuitiva, reduciendo la dependencia del soporte. | Menor tiempo operativo y una agenda fácil de mantener. |
| Paciente | Reservar en pocos pasos, sin obligación de crear una cuenta ni validar el correo electrónico. | Reserva más simple y accesible, incluyendo a personas adultas mayores. |
| Administrador del sistema | Mantener una solución clara, segura y desplegable sin una infraestructura compleja. | Sistema viable para el MVP y con posibilidad de evolución futura. |

## 1.4. Impacto y datos a medir

El relevamiento cualitativo confirma la existencia del problema, pero antes de la entrega final de la propuesta conviene completar una medición básica del impacto. No se incorporan valores numéricos que no hayan sido informados por la profesional.

Costo mensual aproximado de la plataforma actual.

Tiempo promedio requerido para configurar o modificar la agenda.

Frecuencia con la que la profesional necesita solicitar asistencia al soporte.

Cantidad aproximada de pacientes que tienen dificultades o abandonan la reserva por la validación mediante correo electrónico.

Cantidad aproximada de turnos administrados por semana o por mes.

# 2. Definición de la problemática

Una profesional podóloga independiente utiliza una plataforma web comercial que le permite gestionar turnos y enviar recordatorios, pero enfrenta un costo recurrente que considera elevado y una configuración de agenda poco intuitiva que, en determinados casos, la obliga a recurrir al soporte técnico. Al mismo tiempo, el proceso de reserva exige a los pacientes nuevos validar la operación mediante un código recibido por correo electrónico, lo que agrega pasos innecesarios y puede dificultar la experiencia de personas adultas mayores. Una solución web enfocada en la simplicidad podría reducir el esfuerzo operativo de la profesional, facilitar la reserva de turnos y mantener los beneficios de la gestión digital.

# 3. Propuesta de solución y valor agregado

Se propone desarrollar una aplicación web de gestión de turnos orientada inicialmente a un consultorio podológico independiente. La solución priorizará una agenda de autogestión simple para la profesional y un flujo público de reserva en el que el paciente elige un servicio, una fecha y un horario antes de completar sus datos.

## 3.1. Valor agregado

Reducir el costo recurrente asociado a plataformas comerciales, considerando que la nueva solución también tendrá costos de infraestructura y, eventualmente, de mensajería.

Disminuir el tiempo dedicado a configurar la agenda y a solicitar soporte técnico.

Simplificar la reserva para el paciente, evitando el requisito de validar un correo electrónico para completar el turno.

Mantener recordatorios de turnos mediante una integración de mensajería, con WhatsApp como canal preferente.

Generar una base técnica que pueda evolucionar en el futuro hacia otros profesionales independientes, sin convertir esa expansión en requisito del MVP.

## 3.2. Objetivo general

Desarrollar una aplicación web para la gestión de turnos de un profesional independiente que simplifique la administración de la agenda y permita a los pacientes elegir un servicio y reservar un turno mediante una experiencia de uso clara y accesible.

## 3.3. Objetivos específicos

Permitir al profesional configurar días, horarios disponibles, excepciones y los servicios que ofrece desde una interfaz sencilla.

Permitir a los pacientes elegir un servicio, consultar disponibilidad y reservar turnos sin necesidad de crear una cuenta ni validar un correo electrónico, manteniendo sus datos básicos e historial de turnos en una entidad Paciente.

Evitar inconsistencias en la agenda, incluida la asignación simultánea o superpuesta de horarios, considerando la duración del servicio elegido.

Incorporar recordatorios de turnos mediante un servicio de mensajería, priorizando WhatsApp y respetando la aceptación del paciente.

Diseñar una interfaz simple y comprensible, considerando especialmente a usuarios adultos mayores.

Desplegar la solución en servicios online, cumpliendo con el requisito académico de mantener al menos un componente principal operativo en la nube.

# 4. Alcance del Producto Mínimo Viable (MVP)

## 4.1. Módulos incluidos en el MVP

### Módulo 1 — Gestión de agenda y servicios (profesional)

- Configurar días de atención, franjas horarias disponibles, excepciones y servicios ofrecidos con nombre, descripción, duración y precio.
- Ingresar al panel administrativo mediante email y contraseña.

### Módulo 2 — Reserva pública (paciente)

- Elegir un servicio, consultar disponibilidad y reservar un turno en una fecha y horario disponibles.
- El paciente ingresa sus datos básicos y teléfono. El sistema registra un nuevo Paciente o reutiliza uno existente, sin exigir cuenta, contraseña ni validación por correo electrónico.
- Aceptar opcionalmente el envío de recordatorios por WhatsApp.

### Módulo 3 — Gestión de pacientes y turnos

- Registrar, buscar y actualizar los datos básicos de los pacientes, y consultar su historial de turnos.
- Gestionar altas, modificaciones, cancelaciones y estados de los turnos. Los turnos se crean como PENDIENTE, pasan a CONFIRMADO cuando la profesional los acepta, a REALIZADO cuando se atienden y a AUSENTE cuando el paciente no se presenta, y a CANCELADO cuando se anulan.

### Módulo 4 — Validaciones de negocio

- Impedir turnos duplicados o solapados mediante controles en la base de datos y validaciones desde el backend. La duración del servicio elegido se utilizará para calcular la hora de finalización del turno.

### Módulo 5 — Recordatorios (WhatsApp)

- Generar y enviar recordatorios de turnos por WhatsApp cuando el paciente haya aceptado recibirlos, sujeto a las condiciones técnicas y económicas de la API elegida.
- Si la integración no es viable en el plazo académico, generar y listar los recordatorios pendientes para envío manual.

### Módulo 6 — Despliegue

- Despliegue online del frontend, backend y/o base de datos según la arquitectura seleccionada.

Paciente como entidad independiente: aunque el paciente no tendrá una cuenta de usuario, sus datos básicos (nombre, apellido y teléfono) se guardarán en una entidad propia. Esto permite actualizar su información una sola vez, consultar su historial de turnos y evitar repetir esos datos en cada reserva.

Servicio como entidad independiente: cada servicio ofrecido por la profesional tendrá nombre, descripción, duración y precio. Cada Turno se relacionará con un Servicio y la duración se utilizará para calcular la hora de finalización y validar la disponibilidad completa del horario.

## 4.2. Fuera del alcance del MVP

Pasarelas de pago online.

Facturación electrónica.

Historia clínica integral o almacenamiento de información clínica compleja.

Aplicaciones móviles nativas.

Cuenta o inicio de sesión para pacientes.

Inicio de sesión con Google para pacientes.

Códigos de descuento.

Reserva de varios servicios dentro de un mismo turno.

Modelo comercial de suscripciones para múltiples consultorios.

Arquitectura multi-tenant o gestión de múltiples organizaciones en la primera versión.

## 4.3. Plan de contingencia para recordatorios

Si durante el desarrollo las restricciones de la API de WhatsApp —costos, límites, aprobación o configuración— impiden automatizar el envío dentro del plazo académico, el MVP deberá al menos generar y listar los recordatorios pendientes para que la profesional pueda enviarlos manualmente. De esta manera, la dependencia de un servicio externo no bloquea la finalización del proyecto.

## 4.4. Evolución futura

Una vez validado el MVP para un único profesional, la solución podrá evaluarse para incorporar múltiples profesionales o consultorios, configuraciones independientes y eventualmente un modelo de comercialización. Esa evolución no forma parte de la primera versión.

# 5. Stack tecnológico propuesto

| Componente | Tecnología | Justificación |
| --- | --- | --- |
| Backend | Java + Spring Boot + Gradle | Se prioriza un stack conocido por el equipo y adecuado para una aplicación web transaccional con reglas de negocio de agenda y reservas. |
| Frontend | React + TypeScript + Vite + HTML5/CSS3 | React permite construir una interfaz por componentes; TypeScript aporta tipado en el cliente y Vite facilita un entorno de desarrollo liviano y rápido. |
| Base de datos | PostgreSQL (relacional / SQL) | Las entidades del sistema presentan relaciones claras. PostgreSQL aporta transacciones, restricciones e integridad para implementar controles consistentes sobre turnos y disponibilidad. |
| Comunicación | API REST | Permite separar frontend y backend mediante una interfaz clara de servicios para agenda, servicios ofrecidos, pacientes y turnos. |
| Despliegue | PaaS: Vercel para frontend; Railway para backend y PostgreSQL | Evita administrar servidores manualmente y permite cumplir con el requisito de disponer de componentes principales funcionando online. |
| Control de versiones | Git + GitHub | El proyecto se mantendrá en un único repositorio, conforme al requisito de la asignatura. |

## 5.1. Consideraciones sobre consistencia de turnos

PostgreSQL aporta transacciones y restricciones, pero por sí solo no resuelve toda la lógica de disponibilidad. En el esquema se evita que existan dos turnos activos con la misma hora de inicio en una agenda, y el backend validará además que no haya solapamientos con otros intervalos antes de confirmar la reserva.

## 5.2. Criterio de elección

La elección prioriza tecnologías conocidas y maduras, un alcance realista y un despliegue sencillo. Se evita introducir microservicios, orquestadores u otras capas de infraestructura que no sean necesarias para el tamaño del MVP.

# 6. Competencia y diferenciación

El análisis de competencia es preliminar y deberá contrastarse antes de la entrega definitiva con información actual de cada servicio. A partir del relevamiento realizado, se consideran los siguientes grupos:

**Competidores directos: **plataformas SaaS de gestión de turnos para profesionales, como Doctoralia o TuTurno, que resuelven el mismo problema general.

**Competidores indirectos: **gestión manual mediante WhatsApp, llamadas telefónicas, agendas en papel o planillas, alternativas de bajo costo pero con mayor carga operativa.

**Diferenciador propuesto: **una experiencia de reserva con menos pasos y una configuración de agenda simplificada, con especial atención a la accesibilidad de personas adultas mayores.

## 6.1. Matriz FODA

| Dimensión | Análisis |
| --- | --- |
| Fortalezas | Interfaz enfocada en simplicidad y accesibilidad; stack conocido por el equipo; alcance acotado a un problema concreto. |
| Oportunidades | Posible adaptación futura para otros profesionales independientes que enfrenten problemas similares de costo y usabilidad. |
| Debilidades | Equipo reducido de dos integrantes; dependencia parcial de servicios externos de mensajería y despliegue. |
| Amenazas | Cambios en precios o condiciones de APIs y plataformas PaaS; mejoras de competidores actuales que reduzcan las diferencias de costo o experiencia de uso. |

# 7. Análisis de viabilidad

**Viabilidad técnica: **alta para el MVP propuesto, siempre que el equipo mantenga el alcance definido. El stack seleccionado es conocido por los integrantes y cuenta con herramientas maduras para aplicaciones web transaccionales.

**Viabilidad operativa: **favorable. La profesional ya utiliza una solución digital para gestionar turnos, por lo que existe experiencia previa con este tipo de herramienta. La adopción deberá validarse mediante pruebas de uso y no se considera garantizada de antemano.

**Viabilidad temporal: **factible si se priorizan las funciones esenciales del MVP y se postergan funcionalidades comerciales, historias clínicas complejas, pagos y expansión multi-consultorio.

# 8. Plan de trabajo inicial

| Etapa | Objetivo | Entregables | Riesgo y mitigación |
| --- | --- | --- | --- |
| Fase 1 - Diseño y modelado | Cerrar alcance y modelar la solución. | Modelo de datos; módulos; mockups/prototipos de pantallas. | Riesgo: incorporar funciones innecesarias. Mitigación: validar alcance y prototipos con la profesional antes de desarrollar. |
| Fase 2 - Backend | Construir la lógica de agenda, servicios, pacientes y reservas. | API REST en Spring Boot; PostgreSQL conectado; endpoints de servicios, pacientes, agenda y turnos; validaciones de disponibilidad. | Riesgo: inconsistencias de turnos o problemas de infraestructura. Mitigación: pruebas tempranas y despliegue inicial del backend en la PaaS. |
| Fase 3 - Frontend | Construir las interfaces del profesional y del paciente. | Panel administrativo con agenda, servicios, pacientes y turnos; reserva pública con selección de servicio en React/TypeScript. | Riesgo: fricción de uso.<br>Mitigación: pruebas de usabilidad con la profesional y, si es posible, con al menos dos usuarios adultos mayores. |
| Fase 4 - Integración y cierre | Completar el flujo extremo a extremo y preparar la entrega. | Integración de recordatorios; despliegue online; pruebas; documentación; demo y video. | Riesgo: demora de servicios externos o ajustes estéticos. Mitigación: activar el plan de contingencia de recordatorios y postergar mejoras no esenciales. |

# Decisiones de diseno sobre el modelo de datos

Estas decisiones se tomaron despues de una revision critica del modelo generado como borrador, y quedan incorporadas tanto en database/schema.sql como en la documentacion Markdown del repositorio.

## 1. Transiciones de estado del turno

PENDIENTE puede pasar a CONFIRMADO o a CANCELADO. CONFIRMADO puede pasar a REALIZADO, AUSENTE o CANCELADO.

CANCELADO, REALIZADO y AUSENTE son estados terminales: no admiten ninguna salida.

No se admite CONFIRMADO hacia PENDIENTE, porque desconfirmar un turno ya confirmado ensuciaria el historial del paciente. Reprogramar, es decir cambiar la fecha y la hora, si se permite mientras el turno este en PENDIENTE o CONFIRMADO.

La matriz se valida en la base de datos con el trigger trg_turno_transicion_estado, que rechaza con check_violation cualquier cambio de estado no previsto. Los servicios de aplicacion validan la misma matriz para poder devolver un mensaje de negocio en lugar de una excepcion de SQL.

## 2. Vencimiento de reservas sin confirmar

Un turno PENDIENTE cuya hora de fin ya paso se cierra automaticamente como CANCELADO. La operacion la realiza la funcion fn_cerrar_turnos_vencidos, que el backend invoca de forma periodica y que devuelve cuantos turnos cerro. El cierre deja constancia en observaciones sin pisar lo que ya hubiera y es idempotente.

Consecuencia asumida: al cerrar al pasar la hora, un turno pasado ya no se puede confirmar. En el historial del paciente, una reserva nunca confirmada figura como CANCELADO y nunca como AUSENTE; AUSENTE queda reservado para los turnos que estaban CONFIRMADO y el paciente no se presento.

## 3. Zonas horarias

Un turno es un instante real en el calendario. Por eso turno.fecha_hora_inicio, turno.fecha_hora_fin, turno.fecha_creacion, turno.fecha_actualizacion y paciente.fecha_creacion se guardan como TIMESTAMPTZ, con zona horaria. El despliegue en Railway corre en UTC: guardar la hora sin zona haria que un turno de las 10:00 se mostrara corrido al cambiar de huso.

En cambio, agenda_config.hora_inicio y agenda_config.hora_fin siguen siendo TIME, y excepcion_agenda.fecha_inicio y excepcion_agenda.fecha_fin siguen siendo DATE: describen un horario o un dia local, no un instante. En el backend se mapearan con OffsetDateTime, no con LocalDateTime.

## 4. Ultima modificacion, no auditoria

Paciente y Turno registran fecha_creacion, y Turno tiene ademas fecha_actualizacion, mantenida por el trigger trg_turno_fecha_actualizacion porque un DEFAULT CURRENT_TIMESTAMP solo se aplica al insertar.

Esto es ultima modificacion, no auditoria: la columna no registra quien cambio el turno ni que cambio. Una auditoria real exigiria una tabla de historial, que queda fuera del alcance del MVP.
