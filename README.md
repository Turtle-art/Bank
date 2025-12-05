# BankOpenAPI — Spring Boot + OpenAPI Generator POC

This project demonstrates how to use **OpenAPI Generator** to automatically create **Spring Boot REST controller interfaces**, **DTOs**, and **HTTP clients** from OpenAPI specifications. It follows an **API-first approach**, where API contracts are defined in YAML files and backend and client code is generated consistently from a centralized configuration. This ensures type safety, reduces boilerplate, and keeps frontend and backend aligned.

---

## Getting Started

## How Code Generation Works

To generate code with OpenAPI Generator, you follow a simple workflow:

1. **Define Your API Specification**
   Start by creating an OpenAPI YAML or JSON file that describes your API.
   This file contains your endpoints, request/response models, parameters, and any schemas your service uses.
   It acts as the single source of truth for both backend and client code.

2. **Configure OpenAPI Generator in Your `pom.xml`**
   Add the OpenAPI Generator Maven plugin to your project.
   In this configuration, you specify:

   * The location of your spec file (`inputSpec`)
   * What type of code you want to generate (e.g., Spring interfaces, DTOs, or clients)
   * Output directories and generator settings
     This ensures the generator knows how to process your specification.

3. **Generate the Code**
   Run:

   ```bash
   mvn clean generate-sources
   ```

   The plugin reads your specification and automatically produces all the necessary classes—such as controller interfaces, models, and HTTP clients—inside the `target/generated-sources` directory.
   These generated artifacts can then be used directly or implemented in your application.

---

## Key Highlights

### HTTP Client Generation

For HTTP clients, the most important configuration is:

```json
"library": "spring-http-interface"
```

This ensures that the generated interfaces are annotated with `@HttpExchange` instead of `@RequestMapping`, allowing declarative HTTP calls using Spring’s HTTP Interface support.

## Using `inputSpecRootDirectory` for Multiple API Contracts

When your API is split across multiple YAML files, such as:

```
api-contracts/
 ├── accounts-api.yml
 ├── customers-api.yml
 └── transactions-api.yml
```

you can simplify code generation by using **`inputSpecRootDirectory`** instead of `inputSpec`.

### How to Configure in `pom.xml`

Replace a single file specification like:

```xml
<inputSpec>${project.basedir}/src/main/resources/openapi/banking-api.yml</inputSpec>
```

with a directory-based configuration:

```xml
<inputSpecRootDirectory>${project.basedir}/src/main/resources/openapi/api-contracts</inputSpecRootDirectory>
```

This allows OpenAPI Generator to automatically combine all YAML files in the directory and generate the corresponding server interfaces and DTOs in a single run.

### Multiple Executions for Clients

If you are also generating **HTTP clients**, the principle of `inputSpecRootDirectory` still applies. However, since client generation typically requires slightly different configuration (for example, `"library": "spring-http-interface"`), you should define a separate execution in your Maven plugin. This ensures clients are generated correctly while still benefiting from the directory-based approach.

### Centralized JSON Configuration

Maintaining generator settings in centralized JSON files simplifies maintenance. It allows consistent options to be applied across multiple server and client generation executions, reducing errors and ensuring uniformity.

---

## Benefits of Using OpenAPI Generator

* **API-First Development** — Backend and frontend teams can work in parallel based on a shared contract.
* **Automatic Code Generation** — Eliminates boilerplate code while keeping APIs consistent and maintainable.
* **Fewer Integration Bugs** — Request and response structures are validated at compile time.
* **Up-to-Date Documentation** — The OpenAPI spec serves as interactive API documentation via Swagger UI.
* **Cross-Language Reuse** — The same specification can generate SDKs in Java, TypeScript, Python, and other languages.

---

## Purpose

This POC validates a **contract-driven workflow** for:

* Building consistent and maintainable REST APIs
* Generating type-safe HTTP clients for external services
* Providing reliable and well-documented APIs based on industry-standard OpenAPI specifications
