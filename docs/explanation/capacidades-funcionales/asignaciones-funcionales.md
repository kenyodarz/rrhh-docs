# Asignaciones funcionales

## Propósito

Este flujo registra la dedicación de un colaborador a una célula durante un período. Da visibilidad
sobre cómo se distribuye la dedicación declarada, sin calcular ni recomendar una asignación óptima.

## Recorrido

1. Se selecciona el colaborador y la célula.
2. Se indica el período y el porcentaje de dedicación.
3. RRHH guarda la asignación si la suma registrada para ese colaborador y período no supera el 100%.
4. Las asignaciones pueden consultarse por colaborador, célula o período, y actualizarse o
   eliminarse.

Al crear una asignación, el backend requiere colaborador, célula, período y un porcentaje mayor que
cero. También permite registrar una calificación de célula de 0 a 5 para un período; esa
calificación corresponde a la célula y no a la evaluación de desempeño de una persona.

## Resultado y límites

El resultado es un registro de dedicación por período y una calificación de célula cuando se utiliza
esa operación. El backend no interpreta el porcentaje como horas, no recomienda cambios de equipo ni
define quién está autorizado para asignar o calificar.

## Referencias

- [API de asignaciones funcionales y calificaciones](../../reference/api.md)
- [Esquema de datos](../../reference/schemas.md)
