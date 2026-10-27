# Guía técnica para pares

RRHH combina una SPA Angular 22 con una API Java 25 / Spring Boot WebFlux. El backend está organizado con Scaffold Clean Architecture 5.0.4 y persiste en PostgreSQL mediante R2DBC.

## Ejecutar el proyecto

1. Inicia PostgreSQL con `docker compose up -d postgres`.
2. En una terminal, desde la raíz, ejecuta `bash ./gradlew :app-service:bootRun`.
3. En otra terminal, ejecuta `cd web && npm ci && npm start`.

La [puesta en marcha guiada](../tutorials/quickstart.md) detalla los requisitos, URLs locales y verificación de salud.

## Mapa de módulos

```text
RRHH/
├── applications/app-service/                 # Bootstrap Spring y configuración
├── domain/model/                              # Modelos y gateways de dominio
├── domain/usecase/                            # Casos de uso de negocio
├── infrastructure/entry-points/reactive-web/  # Rutas y handlers HTTP WebFlux
├── infrastructure/driven-adapters/
│   ├── r2dbc-postgresql/                      # Persistencia PostgreSQL
│   ├── openai-adapter/                        # Integración de IA
│   └── spring-batch-adapter/                  # Lectura y procesamiento Excel
└── web/src/app/features/                      # Interfaz Angular por funcionalidad
```

La [visión de arquitectura](architecture-overview.md) explica cómo interactúan estas capas.

## Flujo principal: cargue de personal

La carga por staging recibe el Excel, guarda las filas y el control del lote, dispara el procesamiento y expone una ruta para consultar su estado. El proceso filtra y sincroniza registros; al final puede crear alertas por novedades entre periodos.

| Flujo | Estado | Referencia |
|---|---|---|
| Arquitectura full-stack | Generado | [Diagrama](../diagrams/architecture.html) |
| Procesamiento de cargue por staging | Generado | [Secuencia](../diagrams/sequence_main.html), [workflow](../diagrams/workflow_main.html) |
| CRUD de empleados, cargos, células y ausencias | Disponible | [API](../reference/api.md) |

## Pruebas y referencias

- Backend: `bash ./gradlew build jacocoMergedReport`; las pruebas de integración pueden requerir PostgreSQL.
- Frontend: desde `web/`, `npm test` y `npm run build`.
- [API REST](../reference/api.md)
- [Variables de entorno](../reference/environment-variables.md)
- [Esquema de datos](../reference/schemas.md)
- [Escaneo estático de dependencias](../DEPENDENCY_SCAN_AND_IMPROVEMENTS.md)
