# RRHH

RRHH reúne una interfaz Angular y una API reactiva para gestionar información de personas y estructura organizacional. El backend usa Clean Architecture, PostgreSQL y Spring Batch para procesar cargues masivos de personal.

## Ruta para Product Owners y equipos funcionales

Conoce [qué capacidades ofrece RRHH](explanation/capacidades-funcionales-rrhh.md) y recorre sus procesos desde la perspectiva del negocio:

- [Estructura organizacional](explanation/capacidades-funcionales/estructura-organizacional.md)
- [Personas y posiciones](explanation/capacidades-funcionales/personas-y-posiciones.md)
- [Asignaciones funcionales](explanation/capacidades-funcionales/asignaciones-funcionales.md)
- [Ausencias](explanation/capacidades-funcionales/ausencias.md)
- [Cargues y novedades](explanation/capacidades-funcionales/cargues-y-novedades.md)

Estas páginas describen capacidades respaldadas por el backend. La interfaz Janus sirve como contexto de uso; las reglas y límites documentados se contrastan con la implementación de RRHH.

## Empieza aquí

- [Puesta en marcha local](tutorials/quickstart.md): prepara PostgreSQL y ejecuta frontend y backend.
- [Desarrollo local](how-to/local-development.md): configura servicios y variables.
- [Referencia de API](reference/api.md): consulta rutas y enlaces a OpenAPI/Scalar.
- [Arquitectura](explanation/architecture-overview.md): conoce las capas y los principales flujos.
- [Diagramas interactivos](diagrams/index.md): explora arquitectura, secuencia y workflow con Archify.

## Componentes

| Componente | Tecnología | Responsabilidad |
|---|---|---|
| Frontend | Angular 22 | Interfaz de gestión de personal y estructura organizacional. |
| API | Java 25, Spring WebFlux | Expone operaciones de negocio y coordina casos de uso. |
| Persistencia | PostgreSQL y R2DBC | Guarda las entidades, staging de cargues y alertas. |
| Procesamiento por lotes | Spring Batch | Filtra y sincroniza archivos de personal. |

## Mapa documental

- **Capacidades funcionales:** qué resuelve el backend y cómo se agrupan sus procesos para Product Owners.
- **Tutoriales:** aprendizaje guiado para levantar el sistema.
- **Guías prácticas:** desarrollo, pruebas y despliegue.
- **Referencia:** API, variables de configuración y esquema de datos.
- **Explicación:** arquitectura y decisiones técnicas.

El README de la raíz ofrece un inicio rápido. Esta documentación refleja el código y la configuración del repositorio; para dudas sobre el comportamiento, consulta las rutas, handlers y casos de uso enlazados en la referencia.
