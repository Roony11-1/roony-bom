# roony-bom

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Maven Central](https://img.shields.io/maven-central/v/io.github.roony11-1/roony-bom?style=flat-square)](https://search.maven.org/artifact/io.github.roony11-1/roony-bom)

**Bill of Materials (BOM)** oficial del ecosistema `io.github.roony11-1`.

Centraliza y gobierna las versiones de todas las librerías del ecosistema (error, specification y futuros módulos) para garantizar la compatibilidad entre ellas. Al importar este BOM, ya no necesitas especificar manualmente la versión de cada librería en tus proyectos.

> El BOM solamente gestiona **versiones**; no declara dependencias entre los módulos.

## Versiones gestionadas

| Artefacto | Versión |
|---|---|
| `roony-error-core` | 1.0.2 |
| `roony-error-rest` | 1.0.2 |
| `roony-error-spring` | 1.1.1 |
| `roony-error-quarkus` | 1.2.0 *(publicada, sin soporte activo)* |
| `roony-specification-core` | 1.1.0 |
| `roony-specification-jpa` | 1.0.0 |
| `roony-specification-query-params` | 1.0.0 |
| `roony-specification-error-spring` | 1.0.0 |
| `roony-specification-spring` | 1.1.0 |
| `roony-specification-r2dbc` | 1.0.0 |

## Arquitectura de `roony-specification`

Los filtros dinámicos se reparten en módulos con responsabilidades estrictamente separadas:

```text
HTTP/query params
       ↓
roony-specification-query-params  → QueryParamsFilterParser     (Map<String,String> → FilterConditions)
       ↓
roony-specification-core          → modelo y parsing            (FilterCondition, FilterConditions, FilterOperator, FilterParser, FilterException, ValueConverter)
       ↓
   ┌──────────┴───────────┐
   ↓                      ↓
roony-specification-jpa   roony-specification-r2dbc
JpaPredicateBuilder        R2dbcCriteriaBuilder
(FilterConditions → Predicate)   (FilterConditions → Criteria)
   ↓                      ↓
roony-specification-spring
FilterSpecificationBuilder
(FilterConditions → Specification<T>)
```

Cada conversión vive en su módulo. En particular, `roony-specification-spring` **no depende** de `roony-specification-query-params`: el primero convierte `FilterConditions → Specification<T>`, mientras que la conversión `Map<String,String> → FilterConditions` pertenece únicamente a `roony-specification-query-params`.

## Cómo usar el BOM

### 1. Añade el BOM a tu `pom.xml`

En la sección `<dependencyManagement>` de tu proyecto Spring Boot o Quarkus, importa el BOM:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.github.roony11-1</groupId>
            <artifactId>roony-bom</artifactId>
            <version>1.2.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### 2. Declara las dependencias sin versión

```xml
<dependencies>
    <!-- Manejo de errores -->
    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-error-core</artifactId>
    </dependency>
    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-error-spring</artifactId>
    </dependency>

    <!-- Filtros dinámicos para JPA -->
    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-specification-core</artifactId>
    </dependency>
    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-specification-jpa</artifactId>
    </dependency>
    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-specification-query-params</artifactId>
    </dependency>
    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-specification-spring</artifactId>
    </dependency>
    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-specification-r2dbc</artifactId>
    </dependency>
    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-specification-error-spring</artifactId>
    </dependency>
</dependencies>
```

Maven usará automáticamente las versiones definidas en el BOM importado.