# BankOpenAPI — Spring Boot + OpenAPI Generator POC

This project demonstrates how to use **OpenAPI Generator** to automatically create **Spring Boot REST controller interfaces**, **DTOs**, and **HTTP clients** from OpenAPI specifications. It follows an **API-first approach**, where API contracts are defined in YAML files and backend and client code is generated consistently from a centralized configuration. This ensures type safety, reduces boilerplate, and keeps frontend and backend aligned.

---

## Getting Started

1. **Generate Code**

   ```bash
   mvn clean generate-sources
   ```

   This command reads OpenAPI specifications and generates:

   * Server interfaces and DTOs
   * Declarative HTTP client interfaces

   Generated code is placed under `target/generated-sources/openapi/src/main/java`.

2. **Run the Application**

   ```bash
   mvn spring-boot:run
   ```

3. **Access Endpoints**
   Example endpoints include: `GET /api/accounts`, `POST /api/accounts`
   Interactive API documentation is available at `http://localhost:8080/swagger-ui.html`.

---

## Key Highlights

### Multiple Server YAML Files

If your API is organized into multiple files (e.g., `accounts.yml`, `transactions.yml`, `user.yml`), use **`inputSpecRootDirectory`** instead of `inputSpec` in the Maven plugin. This instructs OpenAPI Generator to combine all files under the directory, generating a unified set of interfaces and DTOs. This approach avoids duplication and ensures consistent API structure across modules.

### HTTP Client Generation

For HTTP clients, the most important configuration is:

```json
"library": "spring-http-interface"
```

This ensures that the generated interfaces are annotated with `@HttpExchange` instead of `@RequestMapping`, allowing declarative HTTP calls using Spring’s HTTP Interface support.

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

I can create that **visual summary** next, showing server vs client generation and multi-file combination, if you want. Do you want me to make it?
