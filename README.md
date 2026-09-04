# AceMQ Maven repository

Published artifacts for `org.acemq`, served over GitHub Pages at
**https://acemq-company.github.io/maven/**

Four libraries live here: the AMQP core, its Spring Boot starter, the RabbitMQ
management API, and the load generator. Their version lines are deliberately
independent — the admin library tracks RabbitMQ's management API, not AceMQ's
messaging API, and releasing them together would mean shipping one because the
other changed.

## Using it

Add the repository alongside your dependencies:

```xml
<repositories>
  <repository>
    <id>acemq</id>
    <name>AceMQ</name>
    <url>https://acemq-company.github.io/maven/</url>
  </repository>
</repositories>

<dependencies>
  <dependency>
    <groupId>org.acemq</groupId>
    <artifactId>acemq-amqp-core</artifactId>
    <version>0.2.10</version>
  </dependency>
  <dependency>
    <groupId>org.acemq</groupId>
    <artifactId>acemq-transport-rabbitmq</artifactId>
    <version>0.2.10</version>
    <scope>runtime</scope>
  </dependency>
</dependencies>
```

Gradle:

```kotlin
repositories {
    mavenCentral()
    maven { url = uri("https://acemq-company.github.io/maven/") }
}
```

No credentials are needed. Anyone can resolve from it.

## What is here

### AceMQ for Java — `0.2.10`

[Documentation](https://acemq.org/acemq-java-amqp/) ·
[GitHub](https://github.com/AceMQ-Company/acemq-java-amqp)

| Artifact | What it is |
|---|---|
| `acemq-amqp-api` | The types: `Message`, `Envelope`, `Publisher`, `RetryPolicy`, `Ack` |
| `acemq-amqp-core` | The engine: `AceMq`, publishers, consumers, pipelines, replay |
| `acemq-transport-spi` | The transport contract |
| `acemq-transport-rabbitmq` | RabbitMQ transport — add this at runtime |
| `acemq-amqp-codec-json` | JSON, the default format |
| `acemq-amqp-codec-xml` / `-yaml` / `-toml` / `-avro` / `-protobuf` | Other formats, optional |
| `acemq-amqp-patterns` | Outbox, idempotency stores |
| `acemq-amqp-crypto` | Payload encryption |
| `acemq-amqp-test` | In-memory broker for tests |
| `acemq-security-api` | Pluggable transport security |

### Spring Boot starter — `0.1.0`

One artifact serves **Spring Boot 3 and 4**. The auto-configure module also runs on
Boot 2.7.
[Documentation](https://acemq.org/acemq-java-amqp-spring-boot-starter/) ·
[GitHub](https://github.com/AceMQ-Company/acemq-java-amqp-spring-boot-starter)

| Artifact | What it is |
|---|---|
| `acemq-spring-boot-starter` | The one dependency to add |
| `acemq-spring-boot-autoconfigure` | The auto-configuration alone, to bring your own transport or codec |
| `acemq-spring-boot-health-boot3` | Health indicator for Boot 2.7 and 3 |
| `acemq-spring-boot-health-boot4` | Health indicator for Boot 4 |

```xml
<dependency>
  <groupId>org.acemq</groupId>
  <artifactId>acemq-spring-boot-starter</artifactId>
  <version>0.1.0</version>
</dependency>
```

### RabbitMQ Admin — `0.1.0`

RabbitMQ's HTTP management API for Java. Java 11 bytecode, so a Boot 2.7
application can use it alongside `acemq-amqp-core`.
[Documentation](https://acemq.org/acemq-java-rabbitmq-admin/) ·
[GitHub](https://github.com/AceMQ-Company/acemq-java-rabbitmq-admin)

| Artifact | What it is |
|---|---|
| `acemq-java-rabbitmq-admin` | Users, vhosts, permissions, policies, federation, shovels, health checks, definitions |

### Workloads — `0.1.0`

A load generator for AMQP brokers.
[Documentation](https://acemq.org/acemq-java-amqp-workloads/) ·
[GitHub](https://github.com/AceMQ-Company/acemq-java-amqp-workloads)

| Artifact | What it is |
|---|---|
| `acemq-java-amqp-workloads` | The library: workload DSL, open-loop schedule, latency percentiles, rules |

The **CLI** is not here. It is a shaded jar attached to the
[release](https://github.com/AceMQ-Company/acemq-java-amqp-workloads/releases/latest),
because a shaded jar is a download rather than a dependency.

Sources and Javadoc jars are published alongside every artifact.

## Not on the JVM?

The .NET libraries — C# and VB.NET from one assembly — are not here. They are NuGet
packages, on a static feed run the same way as this one:

```xml
<!-- nuget.config, beside your solution -->
<configuration>
  <packageSources>
    <add key="acemq" value="https://acemq.org/nuget/index.json" />
  </packageSources>
</configuration>
```

| Package | |
|---|---|
| `AceMq.Amqp` | the library |
| `AceMq.Amqp.RabbitMq` | the RabbitMQ transport |
| `AceMq.Amqp.Diagnostics` | Prometheus metrics and health over HTTP |

[Browse the feed](https://acemq.org/nuget/) ·
[Documentation](https://acemq.org/acemq-dotnet-amqp/) ·
[GitHub](https://github.com/AceMQ-Company/acemq-dotnet-amqp)

## A note on this repository

This is a plain Maven repository kept in Git: a directory layout of files served
over HTTPS, which is all a Maven repository has ever been. It is written by
`scripts/publish-maven-repo.sh` in the libraries workspace, and by the release
workflow in
[acemq-java-amqp](https://github.com/AceMQ-Company/acemq-java-amqp) — never by
hand.

It is deliberately **not** Maven Central, for now. Central is permanent — an
artifact published there can never be removed or changed — which is the wrong
trade before 1.0, while coordinates and API shape are still settling. Here a bad
release can be deleted.

The trade going the other way is real and worth knowing before you depend on
this: because these artifacts are not on Central, anyone who publishes a library
that depends on AceMQ will oblige *their* consumers to add this repository too.
That is fine for applications and awkward for libraries. AceMQ will move to
Maven Central for 1.0.
