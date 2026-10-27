# Variables de entorno

Los valores predeterminados se definen en `applications/app-service/src/main/resources/application.yaml`, el perfil local de CORS y `render.yaml`. Para procesos iniciados desde la terminal, exporta las variables antes de ejecutar Gradle; Spring Boot no carga automáticamente `.env`.

## Aplicación backend

| Variable | Propiedad / uso | Valor predeterminado local | Obligatoria |
|---|---|---:|---|
| `PORT` | Puerto HTTP (`server.port`) | `8080` | No |
| `SPRING_PROFILE` | Perfil activo (`spring.profiles.active`) | `local` | No |
| `DB_HOST` | Host PostgreSQL | `localhost` | Sí para conectar a la base |
| `DB_PORT` | Puerto PostgreSQL | `5432` | Sí para conectar a la base |
| `DB_NAME` | Base de datos | `rrhh_db` | Sí para conectar a la base |
| `DB_SCHEMA` | Esquema PostgreSQL | `public` | No |
| `DB_USER` | Usuario PostgreSQL | `postgres` | Sí para conectar a la base |
| `DB_PASSWORD` | Contraseña PostgreSQL | `postgres` | Sí para conectar a la base |
| `AI_API_KEY` | Clave del proveedor de IA | Vacía | Solo para operaciones de IA |
| `AI_BASE_URL` | URL base del proveedor | `https://api.openai.com/v1` | No |
| `AI_MODEL` | Modelo solicitado | `gpt-4o-mini` | No |
| `CORS_ORIGINS` | Orígenes configurados por propiedad | `http://localhost:4200,http://localhost:8080` | No |
| `CORS_ALLOWED_ORIGINS` | Fallback para `CORS_ORIGINS` | No definido | No |

El filtro CORS del perfil `local` restringe el origen a `http://localhost:4200`; los perfiles `dev`, `qa` y `pdn` tienen reglas distintas en `CorsConfig`.

## Encabezados de seguridad

`SecurityHeadersConfig` admite estas propiedades Spring, que pueden configurarse mediante sus nombres de entorno equivalentes en mayúsculas y con guiones bajos.

| Propiedad Spring | Valor predeterminado |
|---|---|
| `security.headers.content-security-policy` | `default-src 'self'; frame-ancestors 'self'; form-action 'self'` |
| `security.headers.strict-transport-security` | `max-age=31536000;` |
| `security.headers.x-content-type-options` | `nosniff` |
| `security.headers.cache-control` | `no-store` |
| `security.headers.pragma` | `no-cache` |
| `security.headers.referrer-policy` | `strict-origin-when-cross-origin` |

## Frontend

| Variable / configuración | Uso | Valor actual |
|---|---|---|
| `environment.apiUrl` | URL base del API en desarrollo | `http://localhost:8080/api` |
| `window.__env.apiUrl` | Override en producción, si está presente | No definido en el repositorio |
| Fallback de producción | URL base utilizada sin override | `https://rrhh-v8ut.onrender.com/api` |

## Compose y Render

`docker-compose.yml` usa `DB_NAME`, `DB_USER`, `DB_PASSWORD` y `POSTGRES_PORT` para configurar PostgreSQL; si faltan, aplica los valores locales indicados en el propio Compose. `render.yaml` declara `PORT`, la conexión a la base y `CORS_ORIGINS`; Render suministra las credenciales de la base mediante referencias al recurso.

`.env.example` también contiene `APP_NAME`, `APP_VERSION`, `ACTUATOR_ENDPOINTS` y `HEALTH_PROBES_ENABLED`. Esos nombres no aparecen enlazados en la configuración actual del backend y no deben tratarse como propiedades efectivas sin confirmar su integración.

## Manejo de secretos

No versionar `.env` con credenciales reales ni claves del proveedor de IA. Usa las variables seguras del entorno o del gestor de secretos correspondiente.
