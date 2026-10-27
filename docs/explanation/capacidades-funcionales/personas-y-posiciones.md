# Personas y posiciones

## Propósito

Este flujo relaciona la información de los colaboradores con la estructura organizacional y con las
posiciones registradas. Permite consultar quién ocupa una posición y conservar los cambios de
ocupación en un historial.

## Colaboradores

El backend registra nombre, apellidos y correo como datos requeridos. También puede mantener estado,
fecha de ingreso y relaciones con cargo, líder, célula y unidad organizativa. Se pueden consultar
los colaboradores y el equipo asociado a un líder.

## Posiciones

Una posición se identifica mediante un código y un nombre. Puede incluir tipo, bolsa o categoría,
esquema de trabajo, subgrupo profesional y colaborador ocupante. Cuando cambia el ocupante, el
backend registra el movimiento en el historial y libera la posición anterior del colaborador para
evitar que figure como ocupante actual de varias posiciones.

Los esquemas de trabajo y subgrupos profesionales funcionan como catálogos asociados a las
posiciones. El código también permite registrar rangos en subgrupos; esta guía no publica valores
salariales ni datos personales.

## Apoyo para descripciones de cargos

El backend permite solicitar una descripción de cargo usando su título como entrada. La respuesta es
contenido generado para apoyar esa tarea; no reemplaza la definición aprobada del cargo ni asigna
automáticamente una persona a una posición.

## Resultado y límites

La información permite consultar colaboradores, sus relaciones organizacionales y la ocupación e
historial de posiciones. No se debe interpretar como un sistema de nómina, contratación o evaluación
individual: esas capacidades no se desprenden de las operaciones descritas aquí.

## Referencias

- [API de empleados, posiciones e inteligencia artificial](../../reference/api.md)
- [Esquema de datos](../../reference/schemas.md)
