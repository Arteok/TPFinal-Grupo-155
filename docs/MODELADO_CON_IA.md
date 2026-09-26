# Modelado con apoyo de IA

Durante la etapa de diseño se utilizaron herramientas de IA como apoyo para revisar y mejorar el modelo del sistema.

El proceso no consistió en aceptar directamente una propuesta generada, sino en usarla como punto de partida y después analizarla según las necesidades reales del proyecto.

## Proceso realizado

A partir de los requerimientos se revisaron las entidades principales, sus relaciones y las reglas de negocio.

Luego se realizaron distintos ajustes hasta lograr una versión consistente entre el diagrama de clases, el DER y el esquema de base de datos.

## Algunas decisiones que se fueron refinando

- Se definió a `Paciente` como una entidad independiente, pero sin cuenta ni inicio de sesión.
- Se incorporó `Servicio` como entidad relacionada con el profesional y los turnos.
- Se definieron los estados `PENDIENTE`, `CONFIRMADO`, `CANCELADO`, `REALIZADO` y `AUSENTE`.
- Se establecieron las transiciones permitidas entre estados.
- Se definió que la hora de finalización se calcula a partir de la duración del servicio.
- Se revisaron las relaciones entre `Profesional`, `Agenda`, `Servicio`, `Paciente` y `Turno`.
- Se ajustaron el DER, el diagrama de clases y el esquema SQL para representar el mismo modelo.

## Herramientas de representación

Los diagramas se realizaron con Mermaid porque permite definirlos como texto, modificarlos fácilmente y mantenerlos versionados dentro del repositorio.

Se utilizaron:

- diagrama de clases;
- diagrama entidad-relación;
- diagramas simples de arquitectura.

## Resultado

El uso de estas herramientas ayudó a comparar alternativas y detectar inconsistencias durante el modelado.

Las decisiones finales se tomaron teniendo en cuenta los requerimientos y el alcance definido para el MVP.