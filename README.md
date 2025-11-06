# 🏦 BankOpenAPI — Spring Boot + OpenAPI Generator POC

This project demonstrates how to use **OpenAPI Generator** to automatically create **Spring Boot REST controller interfaces** and **DTO classes** from an **OpenAPI (Swagger)** specification.
It follows an **API-first** approach — defining the API contract in `banking-api.yml`, then generating consistent backend code.

---

## 🚀 Getting Started

### 1. Generate Code

```bash
mvn clean compile
```

This reads the OpenAPI spec from
`src/main/resources/openapi/banking-api.yml`
and generates interfaces + DTOs under
`target/generated-sources/openapi`.

### 2. Run the Application

```bash
mvn spring-boot:run
```

### 3. Access Endpoints

* `GET /api/accounts`
* `GET /api/accounts/{id}`
* `GET /api/users`
* `POST /api/users`

---

## 🌟 Top 5 Benefits of Using OpenAPI Generator

1. **API-First Development**
   Define your contract once — backend and frontend teams work in parallel from a shared spec.

2. **Automatic Code Generation**
   Instantly create REST interfaces and DTOs — eliminating boilerplate and keeping code consistent.

3. **Fewer Integration Bugs**
   The generator enforces matching request/response structures between backend and clients.

4. **Always-Updated Documentation**
   The OpenAPI spec doubles as interactive API docs (via Swagger UI or ReDoc).

5. **Cross-Language Reuse**
   Use the same spec to generate client SDKs in Java, TypeScript, Python, etc.

---

## 🧩 Tech Stack

* **Spring Boot 3.5.7**
* **Java 21**
* **OpenAPI Generator 7.4.0**
* **Maven**

---

## 📂 Project Structure

```
src/
 ├── main/
 │   ├── java/com/thulasizwe/bank/      → Spring Boot app + controllers
 │   └── resources/openapi/             → OpenAPI spec (banking-api.yml)
 └── test/                              → Tests
target/
 └── generated-sources/openapi/         → Generated interfaces & DTOs
```

---

## 🧠 Purpose

This POC validates a **contract-driven workflow** for building consistent, maintainable, and well-documented REST APIs for banking or financial services.

