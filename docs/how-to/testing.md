# Ejecutar pruebas

## Backend

Desde la raíz del repositorio:

```bash
bash ./gradlew test
```

Para ejecutar la compilación completa y generar el reporte agregado de cobertura:

```bash
bash ./gradlew build jacocoMergedReport
```

Las pruebas que requieren base de datos necesitan PostgreSQL disponible en la configuración esperada por el entorno de pruebas. Puedes levantar la instancia local con `docker compose up -d postgres`.

## Frontend

Desde `web/`:

```bash
npm ci
npm test
```

El proyecto usa el runner de pruebas integrado con Angular y Vitest.

## Compilar Angular

```bash
cd web
npm run build
```

La compilación usa la configuración de producción por defecto de `angular.json`.
