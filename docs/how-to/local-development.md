# Desarrollo local

## Servicios locales

`docker-compose.yml` levanta únicamente PostgreSQL 16. El backend y el frontend se ejecutan con sus herramientas de desarrollo, en procesos separados.

```bash
# Desde la raíz del repositorio
docker compose up -d postgres
docker compose ps

# En otra terminal: backend
bash ./gradlew :app-service:bootRun

# En otra terminal: frontend
cd web
npm ci
npm start
```

El backend escucha en el puerto `8080`; Angular escucha en el `4200` y su entorno local usa `http://localhost:8080/api`. El perfil local permite el origen `http://localhost:4200`.

## Configurar valores distintos a los predeterminados

Spring obtiene su configuración del entorno del proceso. Exporta las variables antes de ejecutar Gradle; copiar `.env.example` a `.env` no exporta automáticamente sus valores al proceso local Java. Compose sí lee `.env` para sustituir variables de su archivo.

```bash
export DB_HOST=localhost
export DB_PORT=5432
export DB_NAME=rrhh_db
export DB_SCHEMA=public
export DB_USER=postgres
export DB_PASSWORD=postgres
export SPRING_PROFILE=local
bash ./gradlew :app-service:bootRun
```

Consulta la [referencia de variables](../reference/environment-variables.md) antes de cambiar configuración. La clave de IA puede omitirse si no se utiliza esa funcionalidad.

## Endpoints de diagnóstico

- Health: `GET http://localhost:8080/actuator/health`
- OpenAPI JSON: `GET http://localhost:8080/v3/api-docs`
- Scalar: `http://localhost:8080/docs` o `http://localhost:8080/scalar`
- Frontend: `http://localhost:4200`

Para consultar cambios persistidos, utiliza la API o conecta una herramienta PostgreSQL a `localhost:5432` con los valores configurados en Compose.
