# 🚀 Guía de Inicio: Aplicación Interna de Gestión de Personal (Java + Angular + IA)

Esta guía contiene la hoja de ruta, arquitectura propuesta, modelo de datos e instrucciones paso a paso para reemplazar el control en Excel por una aplicación web moderna construida con **Java (Spring Boot)**, **Angular** e **Integración de IA**.

---

## 📋 1. Proyectos de Referencia en GitHub

Para inspirarse en el modelado de datos, interfaces y estructura:

* 🏢 **[OrangeHRM Open Source](https://github.com/orangehrm/orangehrm):** Referente principal en gestión de RRHH. Útil para entender flujos de empleados, cargos y jerarquías.
* 🛠️ **[JHipster Generator](https://www.jhipster.tech/):** Herramienta para generar el esqueleto inicial completo de la app (Spring Boot + Angular + DB + Autenticación).
* 📚 **[Spring Boot & Angular Architecture](https://github.com/eugenp/tutorials/tree/master/spring-boot-angular):** Ejemplo clásico de repositorio base para arquitectura limpia REST + SPA.

---

## 🏗️ 2. Arquitectura del Sistema

```text
  [ Frontend: Angular ]  <--->  [ Backend: Spring Boot (REST) ]  <--->  [ Base de Datos: PostgreSQL ]
                                            |
                                            +---> [ Servicios de IA (Spring AI / LLM) ]
```

### **Módulos Principales (MVP)**
1. **Estructura Organizacional:** Gestión de áreas, departamentos, cargos y posiciones.
2. **Ficha del Empleado:** Información personal, fecha de ingreso, cargo actual, salario (opcional/restringido) y líder a cargo.
3. **Panel del Líder (Dashboard):** Vista consolidada del equipo a cargo (organigrama, reporte de colaboradores, cumpleaños, vacantes).
4. **Módulo de IA:** Asistente inteligente para redactar descripciones de cargos y búsquedas avanzadas en lenguaje natural.

---

## 🗄️ 3. Modelo de Datos Recomendado (Entidades)

```sql
-- Departamentos / Áreas
CREATE TABLE departamentos (
    id BIGSERIAL PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    descripcion TEXT
);

-- Cargos / Posiciones
CREATE TABLE cargos (
    id BIGSERIAL PRIMARY KEY,
    titulo VARCHAR(100) NOT NULL,
    nivel VARCHAR(50), -- Junior, Semi-Senior, Senior, Lead
    descripcion TEXT,
    departamento_id BIGINT REFERENCES departamentos(id)
);

-- Empleados
CREATE TABLE empleados (
    id BIGSERIAL PRIMARY KEY,
    nombres VARCHAR(100) NOT NULL,
    apellidos VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    telefono VARCHAR(20),
    fecha_ingreso DATE NOT NULL,
    estado VARCHAR(20) DEFAULT 'ACTIVO', -- ACTIVO, INACTIVO, LICENCIA
    cargo_id BIGINT REFERENCES cargos(id),
    lider_id BIGINT REFERENCES empleados(id) -- Autorreferencia para la jerarquía
);
```

---

## 💡 4. Funcionalidades de Inteligencia Artificial ("Plus")

Aprovechando **Spring AI** en el backend, se pueden implementar estas funciones clave:

1. **Generador de Perfiles y Descripciones de Cargo:**
   * *Entrada:* Título del cargo (ej. "Senior Angular Developer").
   * *Salida IA:* Breve descripción del puesto, responsabilidades clave y requisitos recomendados.
2. **Búsqueda Semántica de Personal:**
   * *Ejemplo:* "Muéstrame los desarrolladores que llevan más de 2 años en el equipo y su líder sea Carlos".
3. **Resumen Inteligente del Equipo:**
   * Generación de resúmenes ejecutivos para los líderes sobre el estado, balance de seniorities y rotación de su equipo.

---

## 🚀 5. Paso a Paso para Iniciar el Desarrollo

### **Paso 1: Configurar el Backend (Spring Boot)**
1. Crear el proyecto en [start.spring.io](https://start.spring.io/) con las dependencias:
   * `Spring Web`
   * `Spring Data JPA`
   * `PostgreSQL Driver`
   * `Spring AI (OpenAI / Ollama)`
2. Configurar el `application.yml` para la conexión a BD y API Key del proveedor de IA.
3. Definir Entidades JPA (`Empleado`, `Cargo`, `Departamento`) y Repositorios.

### **Paso 2: Configurar el Frontend (Angular)**
1. Crear la aplicación con Angular CLI:
   ```bash
   ng new personal-admin-ui --routing --style=scss
   ```
2. Instalar una librería de componentes UI (ej. **PrimeNG** o **Angular Material**):
   ```bash
   ng add @angular/material
   ```
3. Crear módulos y componentes principales:
   * `dashboard-lider`
   * `empleados-list` / `empleado-detail`
   * `cargos-form` (con integración a IA)

### **Paso 3: Integrar la IA en Spring Boot (Ejemplo de Código)**

```java
@RestController
@RequestMapping("/api/ai")
public class AIController {

    private final ChatClient chatClient;

    public AIController(ChatClient.Builder chatClientBuilder) {
        this.chatClient = chatClientBuilder.build();
    }

    @GetMapping("/generar-descripcion")
    public String generarDescripcionCargo(@RequestParam String tituloCargo) {
        String prompt = "Genera una descripción detallada de responsabilidades para el cargo: " + tituloCargo;
        return chatClient.prompt().user(prompt).call().content();
    }
}
```

---

## 📌 Checklist de Entrega MVP

- [ ] Script SQL / Migraciones Liquibase o Flyway listos.
- [ ] Backend REST API funcional con operaciones CRUD de Empleados y Cargos.
- [ ] Frontend Angular responsive con tabla de empleados y asignación de líder.
- [ ] Vista de equipo a cargo filtrada por el líder autenticado.
- [ ] Endpoint de IA integrado generando descripciones de cargos.
