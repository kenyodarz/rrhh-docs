# Guía Universal para Configuración de Git Hook: Validación de Commits (Semantic Release)

Este documento contiene la especificación y los scripts genéricos para instalar de manera automática
el hook de Git (`commit-msg`) en cualquier repositorio. Está diseñado para ser interpretado y
ejecutado tanto por **agentes de IA** como por **desarrolladores**, garantizando que ningún commit
malformado ingrese al historial local.

---

## 1. Estándar de Commits Soportado

- **Formato Estándar:** `COMMIT_TYPE(scope_name): descripción`
- **Formato Breaking Change:** `COMMIT_TYPE(scope_name)!: descripción`
- **Tipos Permitidos (`COMMIT_TYPE`):**
    - `feat`: Nuevas funcionalidades (**MINOR**).
    - `fix`: Corrección de errores (**PATCH**).
    - `perf`: Mejoras de rendimiento (**PATCH**).
    - `security`: Parches de seguridad (**PATCH**).
    - `docs`, `style`, `refactor`, `test`, `build`, `ci`, `chore`, `deprecated`, `removed`: Sin
      release.
- **Scope (`SCOPE`):** Minúsculas y guiones bajos en formato `snake_case` (ej. `auth_service`,
  `core_engine`, `user_module`).
- **Descripción:** Minúscula inicial, sin punto final.

---

## 2. Script del Hook (`.git/hooks/commit-msg`)

A continuación se presenta el script en Bash puro (compatible con Linux, macOS y Windows con Git
Bash / WSL / MSYS2):

```bash
#!/usr/bin/env bash

# ==============================================================================
# Git Hook: commit-msg
# Validador estricto de Conventional Commits para Semantic Release
# ==============================================================================

COMMIT_MSG_FILE="$1"
COMMIT_MSG=$(head -n 1 "$COMMIT_MSG_FILE")

# Expresión regular para validar el formato:
# type(scope): description  o  type(scope)!: description
REGEX="^(feat|fix|docs|style|refactor|perf|test|build|ci|removed|deprecated|security|chore)\([a-z0-9_]+\)!?:\s+.+$"

# Permitir commits automáticos generados por Git (Merge, Revert, etc.)
if [[ "$COMMIT_MSG" =~ ^Merge[[:space:]] ]] || [[ "$COMMIT_MSG" =~ ^Revert[[:space:]] ]] || [[ "$COMMIT_MSG" =~ ^fixup![[:space:]] ]] || [[ "$COMMIT_MSG" =~ ^squash![[:space:]] ]]; then
    exit 0
fi

# Validar el mensaje contra la expresión regular
if [[ ! "$COMMIT_MSG" =~ $REGEX ]]; then
    echo ""
    echo "❌ [ERROR] El mensaje de commit no cumple con el estándar de Semantic Release:"
    echo "------------------------------------------------------------------------------"
    echo "   Mensaje ingresado: \"$COMMIT_MSG\""
    echo ""
    echo "   Formato esperado:"
    echo "      COMMIT_TYPE(scope_name): descripción"
    echo "      COMMIT_TYPE(scope_name)!: descripción (Breaking Change)"
    echo ""
    echo "   Tipos permitidos:"
    echo "      feat, fix, perf, security, docs, style, refactor, test, build, ci, chore, deprecated, removed"
    echo ""
    echo "   Reglas de Scope:"
    echo "      - Debe escribirse estrictamente en snake_case (minúsculas, números y guiones bajos)."
    echo ""
    echo "   Ejemplos correctos:"
    echo "      feat(auth_service): agregar filtro reactivo jwt"
    echo "      fix(user_module): corregir validacion de credenciales"
    echo "      feat(core_engine)!: migrar contrato v1 a v2"
    echo "------------------------------------------------------------------------------"
    exit 1
fi

exit 0
```

---

## 3. Instrucción para Agentes de IA: Instalación Automatizada

Cuando un agente deba configurar este hook en un repositorio nuevo, debe seguir estos pasos:

### Opción A: Instalación directa en `.git/hooks/commit-msg`

Ejecutar el siguiente comando en la terminal desde la raíz del repositorio:

```bash
cat << 'EOF' > .git/hooks/commit-msg
#!/usr/bin/env bash
COMMIT_MSG_FILE="$1"
COMMIT_MSG=$(head -n 1 "$COMMIT_MSG_FILE")
REGEX="^(feat|fix|docs|style|refactor|perf|test|build|ci|removed|deprecated|security|chore)\([a-z0-9_]+\)!?:\s+.+$"

if [[ "$COMMIT_MSG" =~ ^Merge[[:space:]] ]] || [[ "$COMMIT_MSG" =~ ^Revert[[:space:]] ]]; then
    exit 0
fi

if [[ ! "$COMMIT_MSG" =~ $REGEX ]]; then
    echo ""
    echo "❌ [ERROR] Formato de commit inválido para Semantic Release:"
    echo "   Mensaje: \"$COMMIT_MSG\""
    echo "   Formato: COMMIT_TYPE(scope_name): descripción"
    echo "   Tipos:   feat, fix, perf, security, docs, style, refactor, test, build, ci, chore, deprecated, removed"
    echo ""
    exit 1
fi
exit 0
EOF
chmod +x .git/hooks/commit-msg
```

---

## 4. Métodos de Instalación Compartida en el Repositorio

Dado que `.git/hooks/` no se sincroniza en el control de versiones, se recomienda utilizar uno de
los siguientes mecanismos para que se instale automáticamente al clonar el proyecto:

### Método 1: Directorio Versionado `.githooks` (Recomendado para cualquier stack)

1. Crear el directorio `.githooks/` en la raíz del proyecto y guardar allí el archivo `commit-msg`.
2. Configurar Git para usar ese directorio:
   ```bash
   git config core.hooksPath .githooks
   chmod +x .githooks/commit-msg
   ```

### Método 2: Integración con Gradle (Para proyectos Java / JVM)

Agregar en `build.gradle` o `main.gradle`:

```groovy
tasks.register('installGitHooks', Copy) {
    from new File(rootProject.rootDir, '.githooks/commit-msg')
    into new File(rootProject.rootDir, '.git/hooks')
    fileMode 0777
}

tasks.matching { it.name in ['build', 'compileJava'] }.configureEach {
    dependsOn 'installGitHooks'
}
```

### Método 3: Integración con NPM / Husky (Para proyectos Node.js)

```bash
npx husky-init && npm install
echo 'npx --no -- commitlint --edit "$1"' > .husky/commit-msg
```

---

## 5. Verificación del Hook

Para validar que la instalación fue exitosa:

1. **Prueba de commit inválido (debe fallar):**
   ```bash
   git commit -m "update code"
   # Salida esperada: ❌ [ERROR] El mensaje de commit no cumple con el estándar...
   ```

2. **Prueba de commit válido (debe pasar):**
   ```bash
   git commit -m "fix(auth_service): corregir validacion de token"
   # Salida esperada: Commit creado exitosamente.
   ```
