# Desplegar en Render

El repositorio incluye `render.yaml` para describir la base de datos PostgreSQL, el servicio Docker del backend y el sitio estático Angular. Render usa `deployment/Dockerfile` para construir el backend y ejecuta `npm install && npm run build` desde `web` para el frontend.

## Crear o actualizar el despliegue

1. En Render, conecta el repositorio y crea un Blueprint usando `render.yaml`.
2. Revisa los servicios y la región declarados en el manifiesto antes de aplicar cambios.
3. Configura en el dashboard los secretos que el entorno requiera; no los incluyas en el repositorio.
4. Espera a que la base PostgreSQL esté lista y verifica el backend en `/actuator/health`.
5. Abre el sitio estático del frontend y confirma que su URL API coincide con el entorno desplegado.

La guía `DEPLOYMENT.md` en la raíz del repositorio contiene detalles operativos adicionales. El manifiesto `render.yaml` y las variables configuradas en Render son la fuente para los valores efectivos del despliegue.

## Notas

- El contenedor del backend compila con Amazon Corretto 25 y ejecuta el JAR de `app-service`.
- El frontend se publica desde `web/dist/web/browser` y configura una reescritura SPA a `index.html`.
- La URL de API de producción tiene un fallback en `web/src/environments/environment.prod.ts`; la aplicación puede sustituirla mediante `window.__env.apiUrl`.
- No documentes ni compartas contraseñas, URLs privadas de bases de datos o claves de IA en TechDocs.
