# Documentación pública

Sitio en español con Material para MkDocs:
<https://kenyodarz.github.io/rrhh-docs/>.

## Alcance

Publicar únicamente contenido genérico revisado y aprobado para difusión pública.
No copiar el historial ni documentos internos del repositorio de origen.
Tras revisar todos los archivos de `docs/`, se incorporaron estas guías genéricas:

- `docs/git_hooks_semantic_release.md`
- `docs/guia_configuracion_azure_devops.md`
- `docs/guia_inicio_gestion_personal.md`

Se excluyen los análisis de negocio, los planes de migración y sprint, y las
especificaciones internas de gestión de personal por contener información del
proyecto o de la organización. Tampoco se publica el archivo de prueba vacío.
La selección es manual: no sincronizar automáticamente toda la carpeta de origen.

## Desarrollo local

Se recomienda Python 3.12, la misma versión utilizada en GitHub Actions.

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
mkdocs serve
```

Para validar antes de publicar:

```bash
mkdocs build --strict
```

La carpeta generada `site/` no se versiona.

## Publicación

En este repositorio público, seleccionar **Settings → Pages → Build and
deployment → Source → GitHub Actions**. No es necesario modificar la visibilidad
ni los permisos de acceso de ningún repositorio.

El flujo `.github/workflows/deploy-pages.yml` construye el sitio en modo estricto
y despliega el artefacto de Pages cuando recibe cambios en `main`. También se
puede ejecutar desde **Actions → Publicar documentación → Run workflow**,
seleccionando `main`, una vez que el flujo exista en esa rama.
