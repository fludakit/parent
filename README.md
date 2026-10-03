# FluDa Kit :: Parent

[![Build](https://github.com/fludakit/parent/actions/workflows/build.yml/badge.svg)](https://github.com/fludakit/parent/actions/workflows/build.yml)

Shared parent POM for all FluDa Kit modules. Consolidates dependency versions, plugin configurations, and build settings to ensure consistency across the project ecosystem.

## What it provides

### Build requirements

- **JDK 21+** (enforced via `maven-enforcer-plugin`)
- **UTF-8** source encoding
- **Maven 3.9+**

### Managed dependencies

All FluDa Kit modules (use without `<version>` in child POMs):

| Module | Artifact |
|---|---|
| SQL Init | `fluda-sql-init-core`, `fluda-sql-init-config`, `fluda-sql-init-cdi` |
| JDBC Client | `fluda-jdbc-client-core`, `fluda-jdbc-client-config`, `fluda-jdbc-client-cdi` |
| Transaction | `fluda-tx-core`, `fluda-tx-cdi`, `fluda-tx-jdbc`, `fluda-tx-jpa` |

BOMs:

| BOM | Version |
|---|---|
| `jakarta.jakartaee-bom` | 11.0.0 |
| `microprofile` | 7.1 |
| `junit-bom` | 6.1.3 |

Libraries:

| Library | Version |
|---|---|
| H2 | 2.5.252 |
| PostgreSQL | 42.7.13 |
| Weld JUnit 5 | 5.0.3.Final |
| SmallRye Config | 4.0.0 |
| HikariCP | 7.1.0 |
| Hibernate ORM | 7.0.2.Final |

### Managed plugins

| Plugin | Version |
|---|---|
| maven-compiler-plugin | 3.16.0 |
| maven-surefire-plugin | 3.6.0 |
| maven-failsafe-plugin | 3.6.0 |
| maven-jar-plugin | 3.4.2 |
| maven-javadoc-plugin | 3.12.0 |
| maven-source-plugin | 3.4.0 |
| maven-gpg-plugin | 3.2.8 |
| maven-deploy-plugin | 3.2.0 |
| maven-enforcer-plugin | 3.5.0 |
| moditect-maven-plugin | 1.3.0.Final |
| central-publishing-maven-plugin | 1.3.1 |

### Release profile

The `release` profile configures publishing to Maven Central:
- GPG artifact signing
- Javadoc and source JAR generation
- Publishing via `central-publishing-maven-plugin`

## Usage

In your module's `pom.xml`:

```xml
<parent>
    <groupId>io.github.fludakit</groupId>
    <artifactId>fludakit-parent</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <relativePath>../parent/pom.xml</relativePath>
</parent>

<artifactId>fluda-your-module-parent</artifactId>
<packaging>pom</packaging>
```

Then in submodule POMs:

```xml
<parent>
    <groupId>io.github.fludakit</groupId>
    <artifactId>fluda-your-module-parent</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <relativePath>../pom.xml</relativePath>
</parent>

<artifactId>fluda-your-module-core</artifactId>
```

Dependencies managed by the parent can be added without versions:

```xml
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <scope>test</scope>
    </dependency>
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

## Building

```bash
mvn clean install
```

## Publishing

### SNAPSHOTs (automatic)

On every push to `main`, the GitHub Actions workflow publishes SNAPSHOTs to GitHub Packages:

```
https://maven.pkg.github.com/fludakit/parent
```

### Releases (manual)

To publish a release to Maven Central:

1. Go to GitHub Actions → "Publish package to the Maven Central Repository"
2. Click "Run workflow"
3. Enter the release version (e.g., `1.0.0`)
4. Enter the next development version (e.g., `1.1.0-SNAPSHOT`)

The workflow will:
- Set the release version
- Commit, tag, and push
- Build with `-P release` (signs, generates javadoc/sources, publishes to Maven Central)
- Bump to the next SNAPSHOT version

## Related repositories

- [fludakit/sql-init](https://github.com/fludakit/sql-init) — SQL schema initialization for Jakarta EE / CDI.
- [fludakit/jdbc-client](https://github.com/fludakit/jdbc-client) — Fluent JDBC client for Jakarta EE / CDI.
- [fludakit/tx](https://github.com/fludakit/tx) — Transaction support for Jakarta EE / CDI.
- [fludakit/template](https://github.com/fludakit/template) — Project template for new modules.
- [fludakit/examples](https://github.com/fludakit/examples) — Runnable example applications.
- [fludakit/fludakit.github.io](https://github.com/fludakit/fludakit.github.io) — Reference documentation site.

## License

Apache License, Version 2.0. See [LICENSE](LICENSE).
