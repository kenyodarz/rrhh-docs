# Revisión estática de dependencias y salud técnica

**Fecha de revisión:** 2026-10-26  
**Alcance:** manifiestos Gradle, propiedades del proyecto, manifiestos Angular y configuración de despliegue.

Este documento registra versiones declaradas o resueltas. No es un escaneo CVE: no se ejecutó una base de datos de vulnerabilidades ni un analizador de composición de software.

## Versiones observadas

| Área | Versión observada | Evidencia |
|---|---|---|
| Scaffold Clean Architecture | `5.0.4` | `settings.gradle`, `gradle.properties` |
| Java | `25` | `gradle.properties`, Dockerfile de despliegue |
| Spring Boot | `4.1.2` | Resolución de `:reactive-web` runtime classpath |
| Springdoc OpenAPI | `2.8.5` | `reactive-web/build.gradle` |
| Jackson 2 | `2.22.4` | Overrides en `build.gradle` |
| Jackson 3 | `3.2.4` | Overrides en `build.gradle` |
| Micrometer Prometheus | `1.17.2` | Resolución de `:reactive-web` runtime classpath |
| Angular | `22.1.x` | `web/package.json` |
| Node.js | `>=22.22.3` | `web/package.json` |
| npm | `10.9.8` | `web/package.json` |
| TypeScript | `~6.0.2` | `web/package.json` |
| Vitest | `^4.0.8` | `web/package.json` |
| PostgreSQL local | `16-alpine` | `docker-compose.yml` |

## Capacidad y configuración observadas

- Actuator expone `health` y `prometheus`.
- La API usa WebFlux y R2DBC para PostgreSQL; Spring Batch mantiene sus metadatos en H2 en memoria.
- La aplicación tiene `schema.sql` inicializado por el adaptador R2DBC.
- El frontend fija sus dependencias con `web/package-lock.json`.
- `render.yaml` define los recursos de despliegue del backend, frontend y PostgreSQL.

## Seguimiento recomendado

1. Ejecutar periódicamente un escaneo institucional de dependencias y registrar su fecha, herramienta, base de datos de CVEs y resultado.
2. Mantener alineadas las versiones de Java y Node entre desarrollo, Docker y Render.
3. Revisar las versiones de Springdoc y otros adaptadores al actualizar Spring Boot, verificando compatibilidad mediante el build y la generación OpenAPI.
4. Mantener actualizados Gradle y npm lockfiles como parte de cambios revisados y probados.

No se asigna severidad de vulnerabilidad ni se declara que las dependencias estén libres de CVEs; esa conclusión requiere un escaneo dedicado y actualizado.
