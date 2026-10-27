# Capacidades funcionales de RRHH

> **Para:** Product Owners, líderes de negocio y equipos funcionales.

RRHH ofrece servicios para mantener y consultar información de colaboradores y de la estructura en
la que trabajan. También registra posiciones, dedicaciones por célula y ausencias, y procesa cargues
periódicos de personal para actualizar los datos y señalar diferencias que requieren revisión.

La interfaz Janus presenta estos procesos a sus usuarios. Esta guía explica qué respalda el backend
de RRHH y qué resultado ofrece cada capacidad; no documenta la tecnología visual ni reemplaza el
contrato de API.

## Qué permite gestionar

| Necesidad funcional                                      | Capacidad de RRHH                                           | Resultado                                                                                           |
|----------------------------------------------------------|-------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| Mantener la estructura de referencia                     | Departamentos, cargos, células y catálogos organizacionales | La información organizacional queda registrada y puede relacionarse con colaboradores y posiciones. |
| Consultar la composición de los equipos                  | Colaboradores, relación con líder y célula                  | Se pueden consultar personas y los equipos asociados a un líder o célula.                           |
| Registrar dónde se ubica y cómo se distribuye el talento | Posiciones y asignaciones funcionales por período           | Se conoce la persona asociada a una posición y la dedicación registrada a células.                  |
| Dar seguimiento a ausencias                              | Registro y estados de ausencia                              | Las ausencias pueden consultarse y pasar por estados pendiente, aprobada o rechazada.               |
| Actualizar información desde archivos periódicos         | Cargue, procesamiento e historial                           | Cada lote tiene un control de procesamiento y puede generar alertas para revisión.                  |

## Recorridos funcionales

1. [Estructura organizacional](capacidades-funcionales/estructura-organizacional.md): organiza
   departamentos, cargos y células.
2. [Personas y posiciones](capacidades-funcionales/personas-y-posiciones.md): relaciona
   colaboradores con su información laboral y posiciones.
3. [Asignaciones funcionales](capacidades-funcionales/asignaciones-funcionales.md): registra
   dedicaciones por célula y período.
4. [Ausencias](capacidades-funcionales/ausencias.md): registra y permite gestionar el estado de una
   ausencia.
5. [Cargues y novedades](capacidades-funcionales/cargues-y-novedades.md): procesa lotes de personal
   y presenta diferencias que necesitan validación.

Como apoyo a la gestión de cargos, el backend expone generación de descripciones a partir del título
del cargo. Esta capacidad no equivale a un asistente general de Talento Humano ni toma decisiones de
asignación o contratación.

## Qué no debe inferirse

- Una alerta por ausencia de un colaborador en un cargue posterior señala una diferencia; no
  determina por sí sola si hubo retiro o traslado.
- La visualización de ausencias o asignaciones en Janus no significa que el backend calcule
  automáticamente capacidad disponible o recomiende una distribución óptima.
- La asociación de Product Owner y líder técnico a una célula es información de la estructura; esta
  documentación no define permisos de acceso ni responsabilidades de aprobación.
- Las reglas de negocio que no están implementadas o descritas explícitamente no se presentan como
  comportamiento del sistema.

## Profundizar

- [Referencia de API](../reference/api.md) para operaciones disponibles.
- [Esquema de datos](../reference/schemas.md) para entidades y relaciones persistidas.
- [Visión de arquitectura](architecture-overview.md) para conocer los límites entre interfaz,
  backend y persistencia.
- [Diagramas interactivos](../diagrams/index.md) para recorrer arquitectura y cargue de personal.
