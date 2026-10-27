# Estructura organizacional

## Propósito

Este flujo permite mantener las referencias con las que RRHH organiza y relaciona colaboradores:
departamentos, cargos, células y catálogos utilizados por los registros organizacionales.

## Qué se puede gestionar

- **Departamentos:** áreas o verticales que agrupan información organizacional.
- **Cargos:** catálogo de cargos que puede asociarse a un departamento.
- **Células:** unidades de trabajo vinculadas a un departamento. Cada célula registra Product Owner
  y líder técnico, además de su nombre y otros datos descriptivos.
- **Catálogos organizacionales:** valores de referencia como unidades organizativas, entornos
  funcionales, EVC, roles y niveles de talento, proveedores y gestiones de gasto.

## Recorrido

1. Se registra o consulta el departamento y el catálogo de cargos.
2. Se crea una célula y se relaciona con el departamento.
3. Se asocian a la célula el Product Owner y el líder técnico, junto con los datos funcionales
   disponibles.
4. Las demás capacidades pueden usar estas referencias para relacionar personas, posiciones y
   asignaciones.

El backend exige nombre y departamento para la célula, y exige que se indiquen Product Owner y líder
técnico. La eliminación de ciertos catálogos se rechaza cuando sus valores están siendo utilizados.

## Resultado y límites

La estructura registrada sirve como contexto para las demás entidades de RRHH. El backend mantiene
relaciones y catálogos; esta capacidad no define por sí sola una jerarquía de aprobación ni las
responsabilidades de negocio de cada rol.

## Referencias

- [API de departamentos, cargos, células y catálogos](../../reference/api.md)
- [Esquema de datos](../../reference/schemas.md)
