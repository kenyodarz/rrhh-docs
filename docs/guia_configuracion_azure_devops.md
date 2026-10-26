# Guía Integral de Configuración y Gobernanza en `.azuredevops`

Este documento proporciona una guía completa y reutilizable sobre las configuraciones, plantillas y
buenas prácticas recomendadas para organizar el directorio `.azuredevops/` en repositorios de Azure
Repos y Azure DevOps Pipelines.

---

## 1. Estructura Recomendada del Directorio

```text
.azuredevops/
├── pull_request_template.md             # Plantilla predeterminada de Pull Request
├── PULL_REQUEST_TEMPLATE/               # Plantillas adicionales y especializadas de PR
│   ├── hotfix.md                        # Plantilla para correcciones críticas en producción
│   ├── release.md                       # Plantilla para despliegues a producción y Semantic Release
│   └── branches/
│       ├── main.md                      # Plantilla aplicada automáticamente a PRs hacia main
│       └── trunk.md                     # Plantilla aplicada automáticamente a PRs hacia trunk
├── CODEOWNERS                           # Asignación automática de revisores por capa y ruta
├── CONTRIBUTING.md                      # Guía de contribución para desarrolladores y agentes de IA
├── SECURITY.md                          # Políticas de divulgación responsable y escaneo de seguridad
└── pipelines/
    └── templates/                       # Plantillas YAML modulares y reutilizables para CI/CD
        ├── validate-commits.yaml        # Step de validación de commits para Semantic Release
        ├── sonar-analysis.yaml          # Step de escaneo estático y Quality Gate
        └── gradle-build-test.yaml       # Step de compilación, tests y cobertura Jacoco
```

---

## 2. Plantillas de Pull Request Especializadas (`PULL_REQUEST_TEMPLATE/`)

Azure DevOps permite disponer de múltiples plantillas de PR ubicadas en la carpeta
`.azuredevops/PULL_REQUEST_TEMPLATE/`. El desarrollador puede seleccionarlas desde la interfaz de
usuario al crear el PR o configurar plantillas específicas por rama en
`.azuredevops/PULL_REQUEST_TEMPLATE/branches/<branch_name>.md`.

### A. Plantilla de Hotfix (`PULL_REQUEST_TEMPLATE/hotfix.md`)

```markdown
# 🚨 Solicitud de Hotfix Crítico

## 1. Incidente y Causa Raíz

- **ID de Incidente / Work Item:** [ej. #12345]
- **Descripción del Bug:**
- **Causa Raíz Identificada:**

## 2. Solución Aplicada

<!-- Describe de forma concisa el cambio y por qué es el enfoque de menor riesgo -->

## 3. Checklist de Seguridad y Calidad Urgente

- [ ] ¿El commit sigue el formato `fix(scope): descripción` o `security(scope): descripción`?
- [ ] ¿El cambio está acotado exclusivamente a la corrección del problema (sin refactors
  innecesarios)?
- [ ] ¿Se crearon pruebas unitarias reproduciendo el bug y validando su solución?
- [ ] ¿Se verificó que no existan secretos ni credenciales expuestas (Sonar S2068)?
- [ ] ¿Plan de Rollback definido en caso de fallo en producción?

## 4. Aprobaciones Requeridas

- [ ] Tech Lead / Arquitectura
- [ ] Responsable de Seguridad (si aplica)
```

---

### B. Plantilla de Release (`PULL_REQUEST_TEMPLATE/release.md`)

```markdown
# 🚀 Preparación de Release / Pase a Producción

## 1. Versión y Alcance

- **Versión Calculada:** [SemVer, ej. v1.4.0]
- **Tipo de Incremento:** [ ] MAJOR (Breaking) | [ ] MINOR (Feature) | [ ] PATCH (Fix)

## 2. Checklist de Semantic Release

- [ ] ¿Todos los commits en la rama cumplen con la convención de `COMMIT_RULES.md`?
- [ ] ¿El pipeline de CI pasó exitosamente con 100% de tests unitarios y Jacoco coverage?
- [ ] ¿Quality Gate de SonarQube en estado verde (sin vulnerabilidades bloqueantes)?
- [ ] ¿Se generó el CHANGELOG correspondiente?

## 3. Verificación de Despliegue

- [ ] Variables de entorno y secretos configurados en Azure Key Vault / App Configuration.
- [ ] Migraciones de base de datos validadas y probadas en ambiente inferior.
```

---

## 3. Asignación Automática de Revisores (`CODEOWNERS`)

El archivo `.azuredevops/CODEOWNERS` define qué personas o equipos deben ser asignados como
revisores obligatorios u opcionales según los directorios o archivos modificados en el Pull Request.

### Ejemplo de Configuración:

```text
# ==============================================================================
# Definición de Propietarios de Código (CODEOWNERS)
# ==============================================================================

# Gobernanza, Infraestructura y CI/CD
/rules/                        @TechLead @ArquitecturaTeam
/deployment/                   @DevOpsTeam
/.azuredevops/                 @DevOpsTeam @TechLead
/build.gradle                  @TechLead
/main.gradle                   @TechLead

# Capas de Arquitectura Hexagonal / Clean Architecture
/domain/                       @ArquitecturaTeam @SeniorDevelopers
/infrastructure/entry-points/  @BackendDevelopers
/infrastructure/driven-adapters/ @BackendDevelopers @DatabaseSpecialists
/applications/app-service/     @TechLead @BackendDevelopers

# Documentación
/docs/                         @DocsReviewers
```

---

## 4. Guía de Contribución (`CONTRIBUTING.md`)

Documento clave para que los nuevos miembros del equipo y los agentes de IA comprendan las normas
del repositorio:

```markdown
# Guía de Contribución al Repositorio

¡Bienvenido! Para mantener la calidad y consistencia del código, sigue estas pautas:

## 1. Flujo de Ramas (Git Flow / Trunk-Based)

- Ramas de funcionalidades: `feature/<nombre_corto>`
- Ramas de corrección de bugs: `fix/<nombre_corto>`
- Ramas de hotfixes urgentes: `hotfix/<nombre_corto>`

## 2. Preparación Local Obligatoria

Antes de realizar tu primer commit, instala el hook de Git ejecutando:
`git config core.hooksPath .githooks` (o consulta `docs/git_hooks_semantic_release.md`).

## 3. Mensajes de Commit

- Todos los mensajes deben seguir el formato `COMMIT_TYPE(scope): descripción`.
- La descripción debe estar **en español** y en minúsculas.
- Consulta `COMMIT_RULES.md` para ver el impacto en versiones de Semantic Release.

## 4. Verificación de Calidad Local

Antes de abrir un Pull Request, ejecuta:
`./gradlew clean build test jacocoMergedReport`
```

---

## 5. Plantillas YAML Modulares para Azure Pipelines (`pipelines/templates/`)

Dividir los pipelines monolíticos en plantillas dentro de `.azuredevops/pipelines/templates/`
facilita el mantenimiento, la reutilización en múltiples componentes y la auditoría.

### A. Template de Validación de Commits (`.azuredevops/pipelines/templates/validate-commits.yaml`)

```yaml
# Step reutilizable para validar mensajes de commit según COMMIT_RULES.md
steps:
  - bash: |
      echo "=========================================================="
      echo "🔍 VALIDANDO CONVENCIÓN DE COMMITS PARA SEMANTIC RELEASE"
      echo "=========================================================="
      REGEX="^(feat|fix|docs|style|refactor|perf|test|build|ci|removed|deprecated|security|chore)\([a-z0-9_]+\)!?:\s+.+$"
      FAILED=0

      if [ "$(Build.Reason)" == "PullRequest" ]; then
        TARGET_BRANCH="$(System.PullRequest.TargetBranchName)"
        git fetch origin "$TARGET_BRANCH" --depth=1000
        MERGE_BASE=$(git merge-base HEAD "origin/$TARGET_BRANCH")
        COMMITS=$(git log $MERGE_BASE..HEAD --pretty=format:"%H")
      else
        COMMITS="$(Build.SourceVersion)"
      fi

      for HASH in $COMMITS; do
        MSG=$(git log -1 --pretty=format:"%s" "$HASH")
        if [[ "$MSG" =~ ^Merge[[:space:]] ]]; then continue; fi
        if [[ ! "$MSG" =~ $REGEX ]]; then
          echo "❌ [ERROR] Commit inválido: $HASH -> \"$MSG\""
          FAILED=1
        else
          echo "✅ [$HASH] $MSG"
        fi
      done

      if [ $FAILED -ne 0 ]; then
        echo "💥 Uno o más commits no cumplen con COMMIT_RULES.md."
        exit 1
      fi
    displayName: 'Validate Commit Messages (Semantic Release)'
```

---

### B. Template de SonarQube (`.azuredevops/pipelines/templates/sonar-analysis.yaml`)

```yaml
parameters:
  - name: componentName
    type: string
  - name: sonarConnection
    type: string
    default: 'SonarQube'

steps:
  - task: SonarQubePrepare@7
    displayName: 'Prepare SonarQube Analysis'
    inputs:
      SonarQube: ${{ parameters.sonarConnection }}
      scannerMode: Other
      extraProperties: |
        sonar.projectKey=$(Build.Repository.Name)_${{ parameters.componentName }}
        sonar.projectName=$(Build.Repository.Name)_${{ parameters.componentName }}
        sonar.project.version=$(Build.BuildNumber)

  - task: Sonar-buildbreaker@8
    displayName: 'Break Build on SonarQube Quality Gate Failure'
    inputs:
      SonarQube: ${{ parameters.sonarConnection }}
```

---

### C. Cómo consumir las plantillas en el pipeline principal (`deployment/azure_build.yaml`):

```yaml
stages:
  - stage: CI
    jobs:
      - job: build
        displayName: 'Build & Quality Assurance'
        pool:
          name: Build
        steps:
          # 1. Validación de Commits
          - template: ../.azuredevops/pipelines/templates/validate-commits.yaml

          # 2. Análisis de Calidad SonarQube
          - template: ../.azuredevops/pipelines/templates/sonar-analysis.yaml
            parameters:
              componentName: '$(componentName)'
```

---

## 6. Política de Seguridad (`SECURITY.md`)

```markdown
# Política de Seguridad y Manejo de Vulnerabilidades

## 1. Reporte Responsable

Si descubres una vulnerabilidad de seguridad en este proyecto, **no abras un Pull Request ni un
Issue público**. Por favor repórtalo directamente al equipo de ciberseguridad a través del canal
oficial del equipo.

## 2. Reglas de Prevención Obligatorias

- **S2068 (No Hardcoded Credentials):** Queda estrictamente prohibido incluir tokens, API keys,
  passwords o connection strings en el código fuente.
- **S2259 (Null Pointer Protection):** Todo flujo reactivo o consulta a adaptadores debe manejar
  `Optional` o verificaciones explícitas de nulabilidad.
- **Validación de Dependencias:** Toda librería externa debe ser analizada para descartar
  vulnerabilidades críticas (CVEs).
```
