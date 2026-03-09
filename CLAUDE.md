# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Maven project that **auto-generates** a Java REST API client for OpenNMS using Swagger Codegen v3. The source of truth is the OpenAPI spec at `src/build/resources/openapi-opennms-v1.json`. All Java sources under `target/generated-sources/` are generated — do not edit them directly.

## Commands

```bash
# Build and generate sources
mvn clean install

# Generate sources only (without running tests)
mvn generate-sources

# Compile only
mvn compile

# Run tests
mvn test

# Run a single test
mvn test -Dtest=TestClassName
```

## Architecture

The project uses `io.swagger.codegen.v3` Maven plugin to generate a Java client from the OpenAPI spec during the `generate-sources` phase. The generated code uses the `okhttp4-gson` library.

**Generated package structure:**
- `it.arsinfo.myplugin.clients.opennms.v1.api` — API interfaces
- `it.arsinfo.myplugin.clients.opennms.v1.model` — Request/response models
- `it.arsinfo.myplugin.clients.opennms.v1.handler` — Invoker/client infrastructure (ApiClient, auth, etc.)

**Key details:**
- Packaged as an OSGi bundle (Felix bundle plugin); exports `it.arsinfo.myplugin.clients.opennms.v1.*`
- Java source/target compatibility: Java 7
- To change the generated API, modify `src/build/resources/openapi-opennms-v1.json` and rebuild
- Codegen is configured with `interfaceOnly=true` and `dateLibrary=java17`
- Tests and documentation generation are disabled in codegen (`generateApiTests=false`, etc.)