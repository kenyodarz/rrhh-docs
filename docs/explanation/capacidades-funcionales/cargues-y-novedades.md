# Cargues de personal y novedades

## Propósito

El flujo de cargue permite incorporar información de personal desde archivos Excel, procesarla por
lotes y consultar el resultado. El control del lote conserva su estado e historial para que el
procesamiento pueda seguirse sin depender de que termine dentro de la misma solicitud.

## Recorrido

1. Se carga el archivo y se crea un control del lote con sus filas en staging.
2. El backend procesa el lote y sincroniza los registros que pasan las reglas del procesamiento.
3. El control informa el estado y los contadores de filas procesadas y descartadas; el historial
   permite consultar lotes anteriores.
4. Cuando el lote se procesa, el backend puede comparar sus colaboradores con el último cargue
   procesado anterior y crear alertas por registros que ya no aparecen.
5. La alerta queda pendiente para revisión y puede marcarse como resuelta.

Los estados del lote contemplan cargado, en proceso, procesado y fallido. Una alerta por un
colaborador ausente en el cargue actual indica que debe verificarse una posible novedad, como retiro
o traslado; no clasifica ni ejecuta esa novedad automáticamente.

## Resultado y límites

Este flujo facilita actualizar registros a partir de una fuente periódica y hace visibles
diferencias entre períodos. La información faltante no constituye por sí sola confirmación de
retiro, traslado o cualquier otro cambio laboral; la interpretación requiere validación funcional.

## Referencias

- [API de cargues, estados y alertas](../../reference/api.md)
- [Diagrama de secuencia del cargue](../../diagrams/sequence_main.html)
- [Workflow de procesamiento](../../diagrams/workflow_main.html)
- [Esquema de datos](../../reference/schemas.md)
