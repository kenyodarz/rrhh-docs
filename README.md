# RRHH - Documentación Técnica Pública

Portal de documentación técnica para desarrolladores y equipos funcionales del sistema RRHH (Backend reactivo Java con Clean Architecture, Frontend Angular y PostgreSQL).

Sitio en vivo: <https://kenyodarz.github.io/rrhh-docs/>.

## Contenido

El portal está estructurado bajo la metodología Diátaxis y los estándares de Spotify Backstage TechDocs:

- **Inicio**: visión general del sistema y guía de acceso por audiencia.
- **Capacidades funcionales (PO)**: estructura organizacional, personas y posiciones, asignaciones funcionales, ausencias, cargues masivos y alertas.
- **Tutorial**: puesta en marcha guiada (backend, frontend y base de datos).
- **Guías prácticas**: desarrollo local, pruebas unitarias/integración y despliegue en Render.
- **Referencia**: API REST, variables de entorno, esquema de base de datos y auditoría estática de dependencias.
- **Arquitectura**: límites y responsabilidades por capa, guía para pares, decisiones de arquitectura (ADR) y diagramas interactivos Archify compilados (rchitecture.html, sequence_main.html, workflow_main.html).
- **Guías complementarias**: Git hooks con validación semántica, gobernanza en Azure DevOps y guía legacy de gestión de personal.

## Desarrollo local

`ash
python -m venv .venv
# En Windows: .venv\Scripts\activate
# En Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt
mkdocs serve
`

## Publicación

El despliegue es completamente automatizado mediante GitHub Actions (.github/workflows/deploy-pages.yml) en cada push a la rama main.
