# Spring PetClinic Vets Service - Architecture Diagram

## Overview

| Property | Value |
|----------|-------|
| **Application Name** | Spring PetClinic Vets Service |
| **Application Type** | Microservice (REST API) |
| **Framework** | Spring Boot 3.4.1 |
| **Java Version** | 17 |
| **Build Tool** | Maven |
| **Package** | org.springframework.samples.petclinic.vets |

## Application Architecture

### High-Level Architecture

```mermaid
graph TB
    Client[External Clients]
    ConfigServer[Config Server]
    EurekaServer[Eureka Discovery Server]
    
    Client -->|HTTP REST API| VetsService[Vets Service]
    VetsService -->|Service Discovery| EurekaServer
    VetsService -->|Configuration| ConfigServer
    VetsService -->|JPA/JDBC| Database[(MySQL/HSQLDB)]
    VetsService -->|Cache| CaffeinCache[Caffeine Cache]
    
    style VetsService fill:#4A90E2,stroke:#2E5C8A,color:#fff
    style Database fill:#E8B931,stroke:#B8890B,color:#fff
    style CaffeinCache fill:#7ED321,stroke:#5FA319,color:#fff
    style ConfigServer fill:#F5A623,stroke:#C5861C,color:#fff
    style EurekaServer fill:#F5A623,stroke:#C5861C,color:#fff
```

### Layered Architecture

```mermaid
graph TD
    subgraph "Presentation Layer"
        REST[REST Controller - VetResource]
    end
    
    subgraph "Business Layer"
        Cache[Cache Management - Caffeine]
        Config[Configuration - VetsProperties]
    end
    
    subgraph "Data Access Layer"
        Repo[VetRepository - JPA]
        Model[Domain Models - Vet, Specialty]
    end
    
    subgraph "Infrastructure Layer"
        DB[(MySQL/HSQLDB Database)]
        Discovery[Eureka Client]
        ConfigClient[Config Client]
    end
    
    REST --> Cache
    Cache --> Repo
    Repo --> Model
    Model --> DB
    REST --> Discovery
    REST --> ConfigClient
    
    style REST fill:#4A90E2,stroke:#2E5C8A,color:#fff
    style Cache fill:#7ED321,stroke:#5FA319,color:#fff
    style Repo fill:#BD10E0,stroke:#8B0AA8,color:#fff
    style DB fill:#E8B931,stroke:#B8890B,color:#fff
```

## Code Structure

### Component Organization

| Layer | Package | Components | Description |
|-------|---------|------------|-------------|
| **Application** | `org.springframework.samples.petclinic.vets` | `VetsServiceApplication` | Main Spring Boot application entry point with service discovery |
| **Web/REST** | `org.springframework.samples.petclinic.vets.web` | `VetResource` | REST controller exposing veterinarian endpoints |
| **Domain Model** | `org.springframework.samples.petclinic.vets.model` | `Vet`, `Specialty`, `VetRepository` | JPA entities and repository interfaces |
| **Configuration** | `org.springframework.samples.petclinic.vets.system` | `VetsProperties`, `CacheConfig` | Application configuration and cache setup |

### Folder Structure

| Path | Purpose |
|------|---------|
| `src/main/java/...vets/` | Main application source code |
| `src/main/java/...vets/model/` | Domain models and repositories |
| `src/main/java/...vets/web/` | REST API controllers |
| `src/main/java/...vets/system/` | System configuration classes |
| `src/main/resources/` | Configuration files and resources |
| `src/main/resources/db/mysql/` | MySQL database schema and data scripts |
| `src/main/resources/db/hsqldb/` | HSQLDB database schema and data scripts |
| `src/test/java/` | Unit and integration tests |

## Technology Stack

### Core Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| **Spring Boot** | 3.4.1 | Application framework |
| **Spring Cloud** | 2024.0.0 | Microservices infrastructure |
| **Java** | 17 | Programming language |
| **Maven** | - | Build and dependency management |

### Spring Ecosystem

| Component | Purpose |
|-----------|---------|
| **Spring Web** | REST API development |
| **Spring Data JPA** | Database access and ORM |
| **Spring Cache** | Caching abstraction |
| **Spring Actuator** | Monitoring and metrics |
| **Spring Cloud Config** | Centralized configuration management |
| **Spring Cloud Netflix Eureka** | Service discovery and registration |

### Data & Storage

| Technology | Purpose |
|------------|---------|
| **MySQL** | Production database (with Azure MySQL JDBC support) |
| **HSQLDB** | Embedded database for development/testing |
| **Caffeine** | High-performance in-memory cache |
| **Hibernate/JPA** | ORM framework |

### Observability & Monitoring

| Technology | Purpose |
|------------|---------|
| **Micrometer Prometheus** | Metrics collection and export |
| **Spring Actuator** | Health checks and application endpoints |
| **Jolokia** | JMX over HTTP monitoring |
| **Chaos Monkey for Spring Boot** | Resilience testing |

### Azure Integration

| Component | Version | Purpose |
|-----------|---------|---------|
| **Spring Cloud Azure JDBC MySQL** | 5.20.1 | Azure MySQL database integration with managed identity support |

### Additional Libraries

| Library | Purpose |
|---------|---------|
| **Lombok** | Reduce boilerplate code |
| **Jakarta XML Bind API** | XML processing |
| **JUnit Jupiter** | Testing framework |

## Data Flow

### Veterinarian Data Retrieval Flow

```mermaid
sequenceDiagram
    participant Client
    participant VetResource
    participant Cache
    participant VetRepository
    participant Database
    
    Client->>VetResource: GET /vets
    VetResource->>Cache: Check cache for vets
    alt Cache Hit
        Cache-->>VetResource: Return cached data
    else Cache Miss
        VetResource->>VetRepository: findAll()
        VetRepository->>Database: SELECT query
        Database-->>VetRepository: Vet data
        VetRepository-->>VetResource: List of Vets
        VetResource->>Cache: Store in cache
    end
    VetResource-->>Client: JSON response with vets
```

## Assessment Summary

### AppCAT Analysis Results

| Metric | Value |
|--------|-------|
| **Total Issues** | 7 |
| **Total Incidents** | 11 |
| **Total Effort** | 35 story points |
| **Analysis Date** | 2026-02-10 |

### Issues by Severity

| Severity | Count |
|----------|-------|
| **Mandatory** | 6 |
| **Optional** | 4 |
| **Potential** | 1 |
| **Information** | 0 |

### Issues by Category

| Category | Count | Description |
|----------|-------|-------------|
| **Remote Communication** | 4 | Service discovery and remote API patterns |
| **Embedded Cache Management** | 3 | Local caching implementations |
| **Spring Migration** | 2 | Spring framework migration considerations |
| **Containerization** | 1 | Container deployment preparation |
| **Framework Upgrade** | 1 | Framework version compatibility |

### Target Azure Services

This application has been assessed for migration to:

1. **Azure Kubernetes Service (AKS)** - Container orchestration platform
2. **Azure App Service** - Managed application hosting
3. **Azure Container Apps** - Serverless container platform

### Key Findings

**Strengths:**
- Modern Spring Boot 3.4.1 framework
- Already uses Azure MySQL JDBC driver
- RESTful API design
- JPA-based data access
- Built-in caching with Caffeine
- Actuator endpoints for monitoring

**Migration Considerations:**
- Service discovery (Eureka) needs Azure-native alternative
- Config Server integration requires adaptation
- Cache management may need distributed solution for scaling
- Remote communication patterns need review for cloud deployment

## Architecture Patterns

### Design Patterns Used

- **Repository Pattern**: Data access abstraction via `VetRepository`
- **RESTful API**: Resource-based HTTP endpoints
- **Caching Pattern**: Method-level caching with `@Cacheable`
- **Dependency Injection**: Spring-managed beans and autowiring
- **Service Discovery**: Eureka client registration
- **Externalized Configuration**: Spring Cloud Config integration

### Microservices Patterns

- **Service Registration**: Eureka client for discovery
- **Centralized Configuration**: Config server integration
- **Health Monitoring**: Actuator endpoints
- **API Gateway Ready**: REST endpoints designed for gateway routing

---

*Generated from AppCAT assessment on 2026-02-10*
