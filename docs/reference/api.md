# Referencia de API

## Base y herramientas

En desarrollo, la URL base es `http://localhost:8080/api`. Las rutas no tienen prefijo de versión. El contrato interactivo que genera Springdoc está disponible en:

- Scalar: `http://localhost:8080/docs` o `http://localhost:8080/scalar`
- OpenAPI JSON: `http://localhost:8080/v3/api-docs`

La tabla se deriva de las rutas de `RouterRest` y el comportamiento de `Handler`. Para parámetros, cuerpos y esquemas completos, consulta OpenAPI/Scalar con el backend iniciado. El código de rutas no declara un esquema de autenticación; las políticas de acceso externas al servicio deben confirmarse en cada entorno.

## Departamentos

| Método | Ruta | Descripción |
|---|---|---|
| `POST` | `/api/departamentos` | Crear un departamento. |
| `GET` | `/api/departamentos` | Listar departamentos. |
| `GET` | `/api/departamentos/{id}` | Consultar un departamento por UUID. |
| `DELETE` | `/api/departamentos/{id}` | Eliminar un departamento. |

## Cargos

| Método | Ruta | Parámetros | Descripción |
|---|---|---|---|
| `POST` | `/api/cargos` | Cuerpo `Cargo` | Crear un cargo. |
| `GET` | `/api/cargos` | Query opcional `departamentoId` | Listar cargos, opcionalmente filtrados por departamento. |
| `GET` | `/api/cargos/{id}` | `id` UUID | Consultar un cargo. |
| `PUT` | `/api/cargos/{id}` | `id` UUID; cuerpo `Cargo` | Actualizar un cargo. |
| `DELETE` | `/api/cargos/{id}` | `id` UUID | Eliminar un cargo. |

## Empleados

| Método | Ruta | Parámetros | Descripción |
|---|---|---|---|
| `POST` | `/api/empleados` | Cuerpo `Empleado` | Registrar un empleado. |
| `GET` | `/api/empleados` | Query opcional `celulaId` | Listar empleados, opcionalmente filtrados por célula. |
| `GET` | `/api/empleados/{id}` | `id` UUID | Consultar un empleado. |
| `PUT` | `/api/empleados/{id}` | `id` UUID; cuerpo `Empleado` | Actualizar un empleado. |
| `GET` | `/api/empleados/lider/{liderId}/equipo` | `liderId` UUID | Consultar el equipo de un líder. |
| `DELETE` | `/api/empleados/{id}` | `id` UUID | Eliminar un empleado. |

## Células

| Método | Ruta | Parámetros | Descripción |
|---|---|---|---|
| `POST` | `/api/celulas` | Cuerpo `Celula` | Crear una célula. |
| `GET` | `/api/celulas` | Query opcional `departamentoId` | Listar células, opcionalmente filtradas por departamento. |
| `GET` | `/api/celulas/{id}` | `id` UUID | Consultar una célula. |
| `PUT` | `/api/celulas/{id}` | `id` UUID; cuerpo `Celula` | Actualizar una célula. |
| `DELETE` | `/api/celulas/{id}` | `id` UUID | Eliminar una célula. |
| `POST` | `/api/celulas/{id}/calificar` | `id` UUID; cuerpo `CalificacionCelula` | Registrar una calificación para la célula. |
| `GET` | `/api/celulas/{id}/calificaciones` | `id` UUID | Listar las calificaciones de la célula. |
| `GET` | `/api/tipos-celula` | — | Listar tipos de célula. |

## Ausencias

| Método | Ruta | Parámetros | Descripción |
|---|---|---|---|
| `POST` | `/api/ausencias` | Cuerpo `Ausencia` | Registrar una ausencia. |
| `GET` | `/api/ausencias` | — | Listar ausencias. |
| `GET` | `/api/ausencias/{id}` | `id` UUID | Consultar una ausencia. |
| `GET` | `/api/ausencias/empleado/{empleadoId}` | `empleadoId` UUID | Listar ausencias de un empleado. |
| `POST`, `PATCH` | `/api/ausencias/{id}/aprobar` | `id` UUID | Aprobar una ausencia. |
| `POST`, `PATCH` | `/api/ausencias/{id}/rechazar` | `id` UUID | Rechazar una ausencia. |
| `DELETE` | `/api/ausencias/{id}` | `id` UUID | Eliminar una ausencia. |

## Posiciones, esquemas y subgrupos

| Método | Ruta | Parámetros | Descripción |
|---|---|---|---|
| `POST` | `/api/posiciones` | Cuerpo `Posicion` | Crear una posición. |
| `GET` | `/api/posiciones` | — | Listar posiciones. |
| `GET` | `/api/posiciones-personal` | — | Alias de listado de posiciones. |
| `GET` | `/api/posiciones/{id}` | `id` UUID | Consultar una posición. |
| `PUT` | `/api/posiciones/{id}` | `id` UUID | Actualizar una posición. |
| `DELETE` | `/api/posiciones/{id}` | `id` UUID | Eliminar una posición. |
| `GET` | `/api/posiciones/{id}/historico` | `id` UUID | Consultar el historial de una posición. |
| `GET` | `/api/esquemas-trabajo` | — | Listar esquemas de trabajo. |
| `GET`, `POST` | `/api/subgrupos-profesionales` | POST recibe el cuerpo del subgrupo | Consultar o crear subgrupos profesionales. |

## Asignaciones funcionales

| Método | Ruta | Parámetros | Descripción |
|---|---|---|---|
| `POST` | `/api/asignaciones-funcionales` | Cuerpo `AsignacionFuncional` | Crear una asignación funcional. |
| `GET` | `/api/asignaciones-funcionales` | — | Listar asignaciones funcionales. |
| `PUT` | `/api/asignaciones-funcionales/{id}` | `id` UUID | Actualizar una asignación funcional. |
| `DELETE` | `/api/asignaciones-funcionales/{id}` | `id` UUID | Eliminar una asignación funcional. |

## Cargue masivo de personal

Los endpoints de carga reciben el archivo Excel como `multipart/form-data`, en el campo `archivo`. Los parámetros `mes` y `anio` son opcionales en las rutas donde se indican.

| Método | Ruta | Parámetros | Descripción |
|---|---|---|---|
| `POST` | `/api/cargue-personal/upload` | Archivo `archivo`; query opcional `mes`, `anio` | Guarda el archivo en staging e inicia el procesamiento del lote. Responde con el control del cargue antes de que finalice el procesamiento. |
| `GET` | `/api/cargue-personal/historial` | — | Consultar el historial de lotes. |
| `POST` | `/api/cargue-personal/{cargueId}/procesar` | `cargueId` UUID; query opcional `mes`, `anio` | Procesar un lote que ya está en staging. |
| `GET` | `/api/cargue-personal/{cargueId}/estado` | `cargueId` UUID | Consultar el estado y contadores del lote. |
| `POST` | `/api/cargue-personal/procesar` | Archivo `archivo`; query opcional `mes`, `anio` | Procesar el archivo directamente sin el flujo de staging. |

El flujo de staging usa estados `CARGADO`, `EN_PROCESO`, `PROCESADO` y `FALLIDO`. Tras un lote procesado, el caso de uso compara cargues para detectar registros ausentes y crear alertas.

## Catálogos organizacionales

| Método | Ruta | Parámetros | Descripción |
|---|---|---|---|
| `GET` | `/api/catalogos/{tipo}` | `tipo` | Listar elementos del catálogo indicado. |
| `POST` | `/api/catalogos/{tipo}` | `tipo`; cuerpo `ItemCatalogo` | Crear un elemento del catálogo. |
| `DELETE` | `/api/catalogos/{tipo}/{id}` | `tipo`; `id` UUID | Eliminar un elemento si no está en uso. |

Los tipos declarados en la descripción OpenAPI son `tipos_recurso`, `roles_talento`, `niveles_talento`, `proveedores`, `entornos_funcionales`, `evcs_funcionales`, `unidades_organizativas`, `gestiones_gasto` y `segundos_niveles`.

## Inteligencia artificial

| Método | Ruta | Parámetros | Descripción |
|---|---|---|---|
| `GET` | `/api/ai/generar-descripcion` | Query requerido `tituloCargo` | Generar una descripción de cargo mediante el adaptador de IA. |

La disponibilidad del proveedor depende de `AI_API_KEY` y de la configuración de `AI_BASE_URL` y `AI_MODEL`.

## Alertas y novedades

| Método | Ruta | Parámetros | Descripción |
|---|---|---|---|
| `GET` | `/api/alertas` | Query opcional `cargueId` | Consultar alertas pendientes, opcionalmente por lote. |
| `PATCH` | `/api/alertas/{id}/resolver` | `id` UUID | Marcar una alerta como resuelta. |

## Respuestas y errores

Las operaciones de creación responden normalmente con `201`; las consultas y actualizaciones, con `200`; y los borrados, con `204`. Un recurso no encontrado puede responder `404`. Los errores de validación e identificadores inválidos se convierten en `400`; los errores no clasificados usan `500`. Consulta la implementación del handler correspondiente y la especificación OpenAPI para confirmar la respuesta de una operación particular.

Los errores JSON generados por el backend incluyen `status`, `error` y `message`. La respuesta exacta depende del handler y del tipo de error.
