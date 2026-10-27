# Arquitectura del sistema

RRHH es una aplicación web separada en una SPA Angular y un backend Java reactivo. El backend sigue la organización modular de Clean Architecture de Scaffold; PostgreSQL se conecta mediante R2DBC y las operaciones de lote se ejecutan con Spring Batch.

## Componentes y límites

| Capa | Ubicación | Responsabilidad |
|---|---|---|
| Interfaz web | `web/src/app/features/` | Páginas y servicios por capacidad: departamentos, células, cargos, empleados, ausencias, posiciones, asignación funcional e IA. |
| Entrada HTTP | `infrastructure/entry-points/reactive-web` | `RouterRest` define rutas WebFlux; `Handler` valida solicitudes y delega casos de uso. |
| Dominio | `domain/model` | Modelos de negocio y puertos/gateways, sin detalles de persistencia. |
| Casos de uso | `domain/usecase` | Orquesta operaciones de negocio y depende de gateways del dominio. |
| Adaptadores | `infrastructure/driven-adapters` | Implementa persistencia R2DBC, integración OpenAI y procesamiento de archivos con Spring Batch. |
| Composición | `applications/app-service` | Ensambla módulos, configura Spring Boot y contiene `schema.sql`. |

## Caminos principales

### Operaciones desde Angular

La aplicación inicia en la ruta `/departamentos`. Los servicios de cada feature usan `ApiBaseService`, cuya URL base proviene del archivo de entorno Angular. Las llamadas HTTP llegan a rutas `/api/...`, `RouterRest` las dirige a `Handler` y este invoca el caso de uso asociado.

### Cargue de personal

`POST /api/cargue-personal/upload` recibe un archivo Excel multipart, guarda las filas originales en staging y crea un control de cargue. Después se inicia el caso de uso batch y la respuesta HTTP retorna el control sin esperar a que termine el trabajo. El procesamiento filtra filas, sincroniza entidades y actualiza el estado; el cliente puede consultar `/api/cargue-personal/{cargueId}/estado`. Al finalizar, se comparan los cargues para detectar novedades y persistir alertas.

### Persistencia e integraciones

El adaptador `r2dbc-postgresql` usa el pool reactivo para PostgreSQL e inicializa `schema.sql`. El adaptador `spring-batch-adapter` ejecuta jobs por chunks y usa una base H2 en memoria aislada para metadatos de Spring Batch. La integración de IA se configura con `AI_BASE_URL`, `AI_MODEL` y `AI_API_KEY`.

## Diagramas

- [Arquitectura full-stack](../diagrams/architecture.html)
- [Secuencia del cargue de personal](../diagrams/sequence_main.html)
- [Workflow de procesamiento del cargue](../diagrams/workflow_main.html)

La [referencia de API](../reference/api.md) enumera las rutas; la [referencia de esquema](../reference/schemas.md) describe las tablas y sus relaciones.
