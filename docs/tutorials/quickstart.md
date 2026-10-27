# Levantar RRHH en local

Este tutorial levanta PostgreSQL, la API Java y la interfaz Angular en tres terminales. Necesitas Docker con Compose, JDK 25, Node.js `>=22.22.3` y npm `10.9.8`.

## 1. Inicia PostgreSQL

Desde la raíz del repositorio:

```bash
docker compose up -d postgres
docker compose ps
```

Espera a que el servicio `postgres` aparezca como saludable. Compose crea por defecto la base `rrhh_db` y la expone en el puerto `5432`; el backend usa esos mismos valores predeterminados.

## 2. Inicia el backend

En otra terminal, desde la raíz:

```bash
bash ./gradlew :app-service:bootRun
```

El backend escucha en `http://localhost:8080`. El adaptador R2DBC ejecuta `schema.sql` al inicializar la conexión.

Verifica el estado del servicio:

```bash
curl http://localhost:8080/actuator/health
```

La documentación interactiva de la API está disponible en `http://localhost:8080/docs` y `http://localhost:8080/scalar`.

## 3. Inicia Angular

En una tercera terminal:

```bash
cd web
npm ci
npm start
```

Abre `http://localhost:4200`. El entorno de desarrollo apunta por defecto a `http://localhost:8080/api`.

## 4. Prueba la aplicación

La ruta inicial abre la sección de departamentos. La API permite administrar departamentos, cargos, empleados, células, ausencias y otros recursos; consulta la [referencia de API](../reference/api.md).

La clave `AI_API_KEY` no es necesaria para iniciar el servicio, pero sí para usar la integración con el proveedor de IA. No incluyas claves reales en archivos versionados.

## Detener PostgreSQL

```bash
docker compose down
```

Este comando conserva el volumen de datos local.
