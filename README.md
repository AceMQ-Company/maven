# AceMQ Maven repository

Published artifacts for `org.acemq`, served over GitHub Pages at
**https://acemq-company.github.io/maven/**

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
    <version>0.2.7</version>
  </dependency>
  <dependency>
    <groupId>org.acemq</groupId>
    <artifactId>acemq-transport-rabbitmq</artifactId>
    <version>0.2.7</version>
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

| Artifact | What it is |
|---|---|
| `acemq-amqp-api` | The types: `Message`, `Envelope`, `Publisher`, `RetryPolicy`, `Ack` |
| `acemq-amqp-core` | The engine: `AceMq`, publishers, consumers, pipelines, replay |
| `acemq-transport-spi` | The transport contract |
| `acemq-transport-rabbitmq` | RabbitMQ transport — add this at runtime |
| `acemq-amqp-codec-json` | JSON, the default format |
| `acemq-amqp-codec-xml` / `-yaml` / `-avro` / `-protobuf` | Other formats, optional |
| `acemq-amqp-patterns` | Outbox, idempotency stores |
| `acemq-amqp-test` | In-memory broker for tests |
| `acemq-security-api` | Pluggable transport security |

Sources and Javadoc jars are published alongside every artifact.

## A note on this repository

This is a plain Maven repository kept in Git: a directory layout of files served
over HTTPS, which is all a Maven repository has ever been. It is written by the
release workflow in
[acemq-java-amqp](https://github.com/AceMQ-Company/acemq-java-amqp), never by
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
