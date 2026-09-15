# roony-bom

Bill of Materials (BOM) para el ecosistema de librerías Java `io.github.roony11-1`.

`roony-bom` centraliza las versiones de las librerías Roony para permitir incorporarlas a un proyecto sin tener que declarar manualmente la versión de cada módulo.

## ¿Qué problema resuelve?

Cuando un proyecto utiliza varios módulos de una misma familia de librerías, declarar sus versiones individualmente puede generar inconsistencias:

```xml
<dependency>
    <groupId>io.github.roony11-1</groupId>
    <artifactId>roony-specification-core</artifactId>
    <version>1.1.0</version>
</dependency>

<dependency>
    <groupId>io.github.roony11-1</groupId>
    <artifactId>roony-specification-jpa</artifactId>
    <version>1.0.0</version>
</dependency>
```

El BOM permite centralizar estas versiones:

```text
roony-bom
    │
    ├── roony-error-*
    │
    └── roony-specification-*
```

Una vez importado, las dependencias del ecosistema pueden declararse sin especificar su versión.

## Características

* Centralización de versiones de las librerías Roony.
* Gestión de compatibilidad entre módulos.
* Uso mediante Maven `dependencyManagement`.
* Permite declarar las dependencias sin repetir versiones.
* Compatible con proyectos Maven, incluyendo aplicaciones Spring Boot y Quarkus.
* Publicado como artefacto Maven independiente.

## Instalación

### Maven

Importa `roony-bom` dentro de la sección `dependencyManagement`:

```xml
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>io.github.roony11-1</groupId>
            <artifactId>roony-bom</artifactId>
            <version>1.1.0</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

Una vez importado, Maven utilizará las versiones definidas por el BOM.

## Declaración de dependencias

Las librerías administradas por el BOM pueden declararse sin especificar su versión:

```xml
<dependencies>

    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-specification-core</artifactId>
    </dependency>

    <dependency>
        <groupId>io.github.roony11-1</groupId>
        <artifactId>roony-specification-jpa</artifactId>
    </dependency>

</dependencies>
```

Maven resolverá automáticamente las versiones definidas por `roony-bom`.

## Librerías administradas

### Roony Specification

Familia de librerías para la construcción y adaptación de filtros dinámicos.

| Artefacto                          | Responsabilidad                                     |
| ---------------------------------- | --------------------------------------------------- |
| `roony-specification-core`         | Modelo y semántica de las condiciones de filtrado   |
| `roony-specification-query-params` | Conversión de query parameters a `FilterConditions` |
| `roony-specification-jpa`          | Adaptación a Jakarta Criteria API                   |
| `roony-specification-spring`       | Integración con Spring Data JPA                     |

### Roony Error

Familia de librerías para el manejo de errores y sus integraciones.

| Artefacto             | Responsabilidad                          |
| --------------------- | ---------------------------------------- |
| `roony-error-core`    | Modelo base de errores                   |
| `roony-error-rest`    | Representación de errores para APIs REST |
| `roony-error-spring`  | Integración con Spring                   |
| `roony-error-quarkus` | Integración con Quarkus                  |

## Arquitectura

El BOM no contiene lógica de negocio ni código de ejecución.

Su responsabilidad es exclusivamente gestionar las versiones de los módulos publicados:

```text
                         roony-bom
                            │
             ┌──────────────┴──────────────┐
             │                             │
             ▼                             ▼
      Roony Specification             Roony Error
             │                             │
      ┌──────┼──────┐              ┌───────┼───────┐
      ▼      ▼      ▼              ▼       ▼       ▼
    core    jpa   spring         core     rest   spring
```

Esto permite que las aplicaciones consumidoras dependan de una versión coherente del ecosistema.

## Versionado

`roony-bom` utiliza versionado semántico.

Cada versión del BOM define un conjunto concreto de versiones compatibles entre los módulos que administra.

Para conocer las versiones administradas por una versión específica del BOM, consulta el `pom.xml` correspondiente.

## Ecosistema

Las librerías están publicadas bajo el grupo Maven:

```text
io.github.roony11-1
```

Los módulos pueden utilizarse individualmente o mediante `roony-bom` cuando se utilizan varios componentes del ecosistema.

## Licencia

Este proyecto está disponible bajo la licencia MIT.