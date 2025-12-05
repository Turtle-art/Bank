# 🏦 BankOpenAPI — Spring Boot + OpenAPI Generator POC

This project demonstrates how to use **OpenAPI Generator** to automatically create **Spring Boot REST controller interfaces**, **DTO classes**, and **HTTP clients** from **OpenAPI (Swagger)** specifications.

It follows an **API-first** approach — defining API contracts in YAML files, then generating consistent backend and client code with **centralized JSON configuration**.

---

## 🚀 Getting Started

### 1. Generate Code
```bash
mvn clean generate-sources
```

This reads the OpenAPI specs and generates:
- **Server interfaces + DTOs** from `banking-api.yml`
- **HTTP client interfaces** from `payment-gateway.yml`

All generated code appears under `target/generated-sources/openapi/src/main/java`.

### 2. Run the Application
```bash
mvn spring-boot:run
```

### 3. Access Endpoints
- `GET /api/accounts`
- `GET /api/accounts/{id}`
- `POST /api/accounts`
- Interactive API docs: `http://localhost:8080/swagger-ui.html`

---

## 🌟 Top 5 Benefits of Using OpenAPI Generator

1. **API-First Development**  
   Define your contract once — backend and frontend teams work in parallel from a shared spec.

2. **Automatic Code Generation**  
   Instantly create REST interfaces, DTOs, and HTTP clients — eliminating boilerplate and keeping code consistent.

3. **Fewer Integration Bugs**  
   The generator enforces matching request/response structures between backend and clients.

4. **Always-Updated Documentation**  
   The OpenAPI spec doubles as interactive API docs (via Swagger UI).

5. **Cross-Language Reuse**  
   Use the same spec to generate client SDKs in Java, TypeScript, Python, etc.

---

## 🧩 Tech Stack

- **Spring Boot 3.5.7**
- **Java 21**
- **OpenAPI Generator 7.4.0**
- **Maven**
- **Spring HTTP Interface** (for declarative HTTP clients)
- **SpringDoc OpenAPI** (for Swagger UI)

---

## 📂 Project Structure

```
src/
 ├── main/
 │   ├── java/com/thulasizwe/bank/
 │   │   ├── controllers/              → Implements generated API interfaces
 │   │   ├── config/                   → HTTP client configuration
 │   │   └── BankApplication.java      → Spring Boot entry point
 │   └── resources/
 │       ├── openapi/
 │       │   ├── banking-api.yml       → Server API specification
 │       │   ├── config/
 │       │   │   ├── server-config.json → Server generation config
 │       │   │   └── client-config.json → Client generation config
 │       │   ├── common/
 │       │   │   └── schemas.yaml      → Shared DTOs
 │       │   └── external/
 │       │       └── payment-gateway.yml → External service spec
 │       └── application.properties
 └── test/                             → Tests

target/
 └── generated-sources/openapi/
     └── src/main/java/
         ├── com/thulasizwe/bank/api/          → Generated API interfaces
         └── com/thulasizwe/bank/dto/          → Generated DTOs
```

---

## ⚙️ Configuration Architecture

### Centralized JSON Configuration

Instead of duplicating settings in `pom.xml`, common configuration is maintained in JSON files:

**`config/server-config.json`** — Server API generation settings:
```json
{
  "generatorName": "spring",
  "generateApis": true,
  "generateModels": true,
  "generateApiTests": false,
  "configOptions": {
    "useSpringBoot3": "true",
    "interfaceOnly": "true",
    "skipDefaultInterface": "true",
    "useTags": "true",
    "useJakartaEe": "true",
    "useBeanValidation": "true",
    "documentationProvider": "springdoc"
  }
}
```

**`config/client-config.json`** — HTTP client generation settings:
```json
{
  "generatorName": "spring",
  "library": "spring-http-interface",
  "generateApis": true,
  "generateModels": true,
  "configOptions": {
    "useSpringBoot3": "true",
    "interfaceOnly": "true",
    "library": "spring-http-interface"
  }
}
```

### Maven Plugin Configuration

The `pom.xml` references these config files and only specifies what varies per execution:

```xml
<plugin>
    <groupId>org.openapitools</groupId>
    <artifactId>openapi-generator-maven-plugin</artifactId>
    <version>7.4.0</version>
    <executions>
        <!-- Server API -->
        <execution>
            <id>generate-api</id>
            <configuration>
                <inputSpec>${project.basedir}/src/main/resources/openapi/banking-api.yml</inputSpec>
                <configurationFile>${project.basedir}/src/main/resources/openapi/config/server-config.json</configurationFile>
                <apiPackage>com.thulasizwe.bank.api</apiPackage>
                <modelPackage>com.thulasizwe.bank.dto</modelPackage>
            </configuration>
        </execution>
        
        <!-- Payment Gateway Client -->
        <execution>
            <id>generate-payment-client</id>
            <configuration>
                <inputSpec>${project.basedir}/src/main/resources/openapi/external/payment-gateway.yml</inputSpec>
                <configurationFile>${project.basedir}/src/main/resources/openapi/config/client-config.json</configurationFile>
                <apiPackage>com.thulasizwe.bank.client.payment.api</apiPackage>
                <modelPackage>com.thulasizwe.bank.client.payment.model</modelPackage>
            </configuration>
        </execution>
    </executions>
</plugin>
```

---

## 🔌 HTTP Client Usage

### Generated Client Interface

From `payment-gateway.yml`, the generator creates:

```java
@HttpExchange("/api/payments")
public interface PaymentApi {
    @PostExchange("/process")
    PaymentResponse processPayment(@RequestBody PaymentRequest request);
}
```

### Client Configuration

Wire up the HTTP client in Spring:

```java
@Configuration
public class ClientConfig {
    
    @Bean
    public PaymentClientApi paymentClient() {
        RestClient transferRestClient = RestClient.builder()
                .baseUrl("http://localhost:8080/v1/api/payments")
                .build();

        HttpServiceProxyFactory factory = HttpServiceProxyFactory
                .builderFor(RestClientAdapter.create(transferRestClient))
                .build();

        return factory.createClient(PaymentClientApi.class);
    }
}
```

### Using the Client

```java
@Service
public class PaymentService {
    
    private final PaymentClientApi paymentClient;
    
    public PaymentService(PaymentApi paymentClient) {
        this.paymentClient = paymentClient;
    }
    
    public PaymentResponse process(PaymentRequest request) {
        return paymentClient.processPayment(request);
    }
}
```

---

## 🛠️ Key Features

✅ **Server Interface Generation** — Generate Spring REST controller interfaces with validation  
✅ **HTTP Client Generation** — Generate declarative HTTP clients using Spring's `@HttpExchange`  
✅ **Shared Schemas** — Reuse common DTOs across multiple API specs  
✅ **Centralized Configuration** — Maintain generator settings in JSON files  
✅ **Type Safety** — Compile-time validation for requests/responses  
✅ **Swagger UI Integration** — Auto-generated interactive API documentation

---

## 🧠 Purpose

This POC validates a **contract-driven workflow** for building:
- Consistent, maintainable REST APIs
- Type-safe HTTP clients for external services
- Well-documented APIs using industry-standard OpenAPI specifications

Perfect for banking, financial services, or any domain requiring strict API contracts.
