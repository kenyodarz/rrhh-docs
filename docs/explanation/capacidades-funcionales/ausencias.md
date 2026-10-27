# Ausencias

## Propósito

Este flujo registra una ausencia vinculada a un colaborador y permite consultar y actualizar su
estado. La interfaz Janus presenta la información en vistas de calendario; el backend respalda el
registro y la decisión de estado.

## Recorrido

1. Se registra el colaborador, el tipo de ausencia y las fechas de inicio y fin. Puede incluirse un
   motivo u observación.
2. El backend valida que las fechas requeridas existan y que la fecha final no sea anterior a la
   inicial.
3. Si no se indica otro estado, la ausencia queda pendiente.
4. La operación de aprobación o rechazo cambia el estado correspondiente. También se pueden
   consultar ausencias por colaborador.

## Resultado y límites

El registro mantiene el período y estado de la ausencia para consulta. El backend no establece qué
persona debe aprobarla ni qué políticas determinan la decisión. Aunque la interfaz muestre una
matriz o un cronograma, estas operaciones no demuestran por sí solas un cálculo automático de
capacidad disponible.

## Referencias

- [API de ausencias](../../reference/api.md)
- [Esquema de datos](../../reference/schemas.md)
