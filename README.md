# Dataverse ARK Persistent Identifier Provider

A standalone Java Service Provider Interface plugin enabling native **Archival Resource Key** persistent identifier support in **Harvard Dataverse 6.x+** without modifying core application code.

Maintained by [OBRA Incorporated](https://obra.openscience.ph) and shared with the Global Dataverse Community Consortium.

---

## Overview

Starting with version 6.x, Harvard Dataverse introduced a modular Service Provider Interface (`dataverse.spi.pidproviders.directory`) for persistent identifiers. This plugin implements Dataverse's `PidProvider` and `PidProviderFactory` interfaces, enabling research repositories to mint, catalog, and resolve ARKs directly through global resolvers such as [N2T.net](https://n2t.net) and [ARKs.org](https://arks.org).

### Key Features
* **Zero Core Modifications**: Installs as an external JAR into Dataverse's SPI directory.
* **Global Resolver Integration**: Pre-configured for N2T and ARKs.org longest-prefix matching.
* **Flexible Namespace Partitioning**: Supports arbitrary organizational shoulders (e.g. `x1`, `x2`, `x3`).
* **MicroProfile & JVM Config**: Fully configurable via standard Dataverse container environment variables and Payara system properties.
* **Zero Marginal Cost**: Eliminates recurring per-deposit commercial registration fees for academic institutions.

---

## Quick Start

### 1. Build the JAR
Compile the plugin using Apache Maven:
```bash
mvn clean package
```
The compiled artifact will be generated at `target/dataverse-ark-1.0.0.jar`.

### 2. Configure Docker Compose
Mount the compiled JAR into your Dataverse container's SPI directory:

```yaml
services:
  dataverse:
    image: gdcc/dataverse:latest
    environment:
      # --- ARK PERSISTENT IDENTIFIER SPI CONFIGURATION ---
      - 'dataverse_spi_pidproviders_directory=/dv/spi-providers'
      - 'dataverse_pid_providers=ark'
      - 'dataverse_pid_default__provider=ark'
      - 'dataverse_pid_ark_type=ark'
      - 'dataverse_pid_ark_label=Institutional ARK Repository'
      - 'dataverse_pid_ark_authority=YOUR_NAAN_HERE'
      - 'dataverse_pid_ark_shoulder=YOUR_SHOULDER_HERE'
      - 'dataverse_pid_ark_base__url=https://n2t.net/ark:/'
      - 'dataverse_pid_ark_separator=/'
    volumes:
      - ./spi-providers:/dv/spi-providers
```

### 3. Start or Restart Dataverse
When Dataverse boots, Payara's SPI loader automatically detects and registers the provider:
```text
INFO: Initializing ArkPidProvider: id=ark, authority=YOUR_NAAN, shoulder=YOUR_SHOULDER, baseUrl=https://n2t.net/ark:/
```

---

## Configuration Reference

The provider reads parameters defined via container environment variables or Payara JVM system properties:

| Environment Variable | System Property | Description | Default Value |
| :--- | :--- | :--- | :--- |
| `dataverse_pid_providers` | `dataverse.pid.providers` | Active PID provider IDs (comma-separated) | `ark` |
| `dataverse_pid_default__provider` | `dataverse.pid.default-provider` | Default provider for new datasets | `ark` |
| `dataverse_pid_ark_type` | `dataverse.pid.ark.type` | SPI provider type selector | `ark` |
| `dataverse_pid_ark_label` | `dataverse.pid.ark.label` | Human-readable label displayed in UI | `ARK` |
| `dataverse_pid_ark_authority` | `dataverse.pid.ark.authority` | Your 5-digit institutional NAAN | *(Required)* |
| `dataverse_pid_ark_shoulder` | `dataverse.pid.ark.shoulder` | Reserved opaque shoulder prefix (e.g., `x3`) | `""` *(Optional)* |
| `dataverse_pid_ark_base__url` | `dataverse.pid.ark.base-url` | Global resolver prefix URL | `https://n2t.net/ark:/` |
| `dataverse_pid_ark_separator` | `dataverse.pid.ark.separator` | Delimiter between NAAN and identifier | `/` |

---

## Institutional Background

This provider was engineered as part of the **OpenScience.ph** sovereign research repository infrastructure, supporting persistent, zero-marginal-cost scientific discoverability for Philippine higher education institutions and regional academic commons.

---

## License

Licensed under the [Apache License, Version 2.0](LICENSE).
