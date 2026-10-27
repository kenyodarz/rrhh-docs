# Esquema de datos

El esquema PostgreSQL se inicializa desde `applications/app-service/src/main/resources/schema.sql` mediante `ConnectionFactoryInitializer` del adaptador R2DBC. La mayoría de las entidades usan UUID como clave primaria. El archivo incluye creación idempotente de tablas, ajustes `ALTER TABLE` y datos iniciales para esquemas de trabajo.

## Tablas

| Tabla | Responsabilidad y relaciones principales |
|---|---|
| `departamentos` | Áreas organizacionales; se relaciona con cargos y células. |
| `cargos` | Catálogo de cargos; referencia opcionalmente a `departamentos`. |
| `empleados` | Datos de personas; referencia cargos, líder y célula, además de catálogos organizacionales. |
| `celulas` | Estructura de células; referencia departamento, unidad y datos funcionales, Product Owner y líder técnico. |
| `posiciones` | Posiciones organizacionales; referencia subgrupo profesional, esquema de trabajo, empleado y unidad organizativa. |
| `historico_posiciones` | Historial asociado a posición y persona. |
| `ausencias` | Ausencias asociadas a empleados. |
| `calificaciones_celula` | Calificaciones asociadas a células. |
| `asignaciones_funcionales` | Asignaciones de empleados a células, con porcentaje y periodo. |
| `cargues_control` | Estado y contadores de cada lote de cargue. |
| `cargue_personal_staging` | Filas crudas de Excel vinculadas a un registro de control. |
| `alertas_cargue` | Alertas asociadas opcionalmente a un cargue. |
| `tipos_recurso` | Catálogo de tipos de recurso. |
| `roles_talento` | Catálogo de roles de talento. |
| `niveles_talento` | Catálogo de niveles de talento. |
| `proveedores` | Catálogo de proveedores. |
| `entornos_funcionales` | Catálogo de entornos funcionales. |
| `evcs_funcionales` | Catálogo de EVC funcionales. |
| `unidades_organizativas` | Catálogo de unidades organizativas. |
| `gestiones_gasto` | Catálogo de gestiones de gasto. |
| `segundos_niveles` | Catálogo de segundos niveles organizacionales. |
| `esquemas_trabajo` | Catálogo de modalidades y distribución de días de trabajo. |
| `subgrupos_profesionales` | Catálogo de subgrupos con rangos salariales. |
| `tipos_celula` | Catálogo de tipos de células. |

## Relaciones y restricciones

- `empleados.lider_id` referencia a otro empleado; `empleados.celula_id` se vincula con `celulas`.
- La eliminación de ciertos catálogos se restringe cuando existen empleados que los usan.
- Las posiciones conservan historial; eliminar una posición o una persona puede eliminar registros históricos según las reglas `ON DELETE CASCADE` del esquema.
- `cargue_personal_staging.cargue_id` referencia el control del cargue; la eliminación del control elimina sus filas staging.
- `alertas_cargue.cargue_id` también referencia el control del lote.
- Los identificadores organizacionales y de negocio se modelan como UUID; los campos de datos de empleado y los nombres de catálogos no son datos de ejemplo para copiar en ambientes reales.

Para nombres de columnas, nulabilidad, defaults e índices exactos, el archivo `schema.sql` es la referencia autoritativa.
