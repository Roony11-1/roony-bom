# roony-bom

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Maven Central](https://img.shields.io/maven-central/v/io.github.roony11-1/roony-bom?style=flat-square)](https://search.maven.org/artifact/io.github.roony11-1/roony-bom)

**Bill of Materials (BOM)** oficial del ecosistema `io.github.roony11-1`.

Centraliza y gobierna las versiones de todas las librerías `roony-error` (y futuros módulos) para garantizar la compatibilidad entre ellas. Al usar este BOM, ya no necesitas especificar manualmente la versión de cada librería en tus proyectos.

## Cómo usar el BOM

### 1. Añade el BOM a tu `pom.xml`

En la sección `<dependencyManagement>` de tu proyecto Spring Boot o Quarkus, importa el BOM:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.github.roony11-1</groupId>
            <artifactId>roony-bom</artifactId>
            <version>1.0.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

### 2. Declara las dependencias sin versión

```xml
<dependencies>
    <!-- Spring Boot -->
    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-error-spring</artifactId>
    </dependency>

    <!-- Quarkus -->
    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-error-quarkus</artifactId>
    </dependency>

    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-error-core</artifactId>
    </dependency>
</dependencies>
```

# Maven usará automáticamente las versiones definidas en el BOM que has importado.