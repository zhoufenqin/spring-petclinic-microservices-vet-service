# Spring PetClinic Vets Service - Architecture Diagram

## Overview

| Property | Value |
|----------|-------|
| Application Name | Spring PetClinic Vets Service |
| Application Type | Microservice - REST API |
| Framework | Spring Boot 3.4.1 |
| Java Version | 17 |
| Build Tool | Maven |
| Packaging | JAR |

## Application Architecture

### High-Level Architecture

```mermaid
graph TB
    Client[External Clients]
    ConfigServer[Config Server]
    EurekaServer[Eureka Discovery Server]
    
    VetsService[Vets Service<br/>Spring Boot 3.4.1]
    
    Cache[(Caffeine Cache<br/>vets)]
    DB[(Database<br/>MySQL/HSQLDB)]
    
    Client -->|HTTP REST API| VetsService
    VetsService -->|Service Discovery| EurekaServer
    VetsService -->|Configuration| ConfigServer
    VetsService -->|Cache Read/Write| Cache
    VetsService -->|JPA/JDBC| DB
    
    style VetsService fill:#4CAF50,stroke:#2E7D32,stroke-width:3px,color:#fff
    style Cache fill:#FF9800,stroke:#E65100,stroke-width:2px,color:#fff
    style DB fill:#2196F3,stroke:#0D47A1,stroke-width:2px,color:#fff
    style ConfigServer fill:#9C27B0,stroke:#4A148C,stroke-width:2px,color:#fff
    style EurekaServer fill:#9C27B0,stroke:#4A148C,stroke-width:2px,color:#fff
```

### Layered Architecture

```mermaid
graph TB
    subgraph "Presentation Layer"
        RestController[VetResource<br/>REST Controller]
    end
    
    subgraph "Business Layer"
        Cache[Caffeine Cache<br/>vets cache]
    end
    
    subgraph "Data Access Layer"
        Repository[VetRepository<br/>Spring Data JPA]
        Entity[Vet Entity<br/>Specialty Entity]
    end
    
    subgraph "Infrastructure"
        Config[Spring Cloud Config]
        Discovery[Eureka Client]
        Actuator[Spring Actuator]
        Monitoring[Prometheus Metrics]
    end
    
    subgraph "Database"
        MySQL[(MySQL Database)]
        HSQLDB[(HSQLDB<br/>Development)]
    end
    
    RestController -->|uses| Cache
    RestController -->|queries| Repository
    Repository -->|maps to| Entity
    Repository -->|JDBC| MySQL
    Repository -->|JDBC| HSQLDB
    
    Config -.->|configures| RestController
    Discovery -.->|registers| RestController
    Actuator -.->|monitors| RestController
    Monitoring -.->|metrics| RestController
    
    style RestController fill:#4CAF50,stroke:#2E7D32,stroke-width:2px,color:#fff
    style Cache fill:#FF9800,stroke:#E65100,stroke-width:2px,color:#fff
    style Repository fill:#2196F3,stroke:#0D47A1,stroke-width:2px,color:#fff
    style Entity fill:#2196F3,stroke:#0D47A1,stroke-width:2px,color:#fff
    style MySQL fill:#00BCD4,stroke:#006064,stroke-width:2px,color:#fff
    style HSQLDB fill:#00BCD4,stroke:#006064,stroke-width:2px,color:#fff
```

## Code Structure

### Components

| Component | Type | Purpose |
|-----------|------|---------|
| VetsServiceApplication | Main Class | Spring Boot application entry point |
| VetResource | REST Controller | Exposes GET /vets endpoint |
| VetRepository | Repository | Spring Data JPA repository for Vet entities |
| Vet | Entity | JPA entity representing veterinarian |
| Specialty | Entity | JPA entity representing vet specialty |
| CacheConfig | Configuration | Caffeine cache configuration |
| VetsProperties | Configuration | Application properties binding |

### Folder Structure

| Path | Description |
|------|-------------|
| src/main/java/.../vets | Main application source code |
| src/main/java/.../vets/model | Domain entities (Vet, Specialty) |
| src/main/java/.../vets/web | REST controllers |
| src/main/java/.../vets/system | Configuration classes |
| src/main/resources | Application configuration and SQL scripts |
| src/main/resources/db/mysql | MySQL database schema and data |
| src/main/resources/db/hsqldb | HSQLDB database schema and data |
| src/test/java | Test classes |

## Technology Stack

### Core Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| Java | 17 | Programming language |
| Spring Boot | 3.4.1 | Application framework |
| Spring Cloud | 2024.0.0 | Microservices infrastructure |
| Maven | Default | Build and dependency management |

### Spring Framework Components

| Component | Purpose |
|-----------|---------|
| Spring Boot Web | REST API development |
| Spring Data JPA | Database access layer |
| Spring Boot Actuator | Health checks and metrics |
| Spring Cache | Caching abstraction |
| Spring Cloud Config | Centralized configuration |
| Spring Cloud Netflix Eureka | Service discovery |
| Spring Cloud Azure | Azure integration |

### Data & Persistence

| Technology | Version | Purpose |
|------------|---------|---------|
| Spring Data JPA | - | ORM framework |
| MySQL Connector | Runtime | MySQL database driver |
| HSQLDB | Runtime | In-memory database for development |
| Azure MySQL JDBC | - | Azure MySQL integration |

### Caching & Performance

| Technology | Version | Purpose |
|------------|---------|---------|
| Caffeine | - | High-performance caching library |
| javax.cache API | - | JSR-107 caching standard |

### Monitoring & Observability

| Technology | Version | Purpose |
|------------|---------|---------|
| Spring Boot Actuator | - | Application health and metrics |
| Micrometer Prometheus | - | Metrics export to Prometheus |
| Jolokia | 1.7.1 | JMX over HTTP |

### Development Tools

| Technology | Version | Purpose |
|------------|---------|---------|
| Lombok | - | Reduce boilerplate code |
| Chaos Monkey | 3.1.0 | Resilience testing |

### Testing

| Technology | Version | Purpose |
|------------|---------|---------|
| Spring Boot Test | - | Testing framework |
| JUnit Jupiter | - | Unit testing |

## Data Flow

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
        Cache-->>VetResource: Return cached vets
        VetResource-->>Client: Return vets list
    else Cache Miss
        VetResource->>VetRepository: findAll()
        VetRepository->>Database: SELECT * FROM vets
        Database-->>VetRepository: Vets data
        VetRepository-->>VetResource: List of Vets
        VetResource->>Cache: Store in cache
        VetResource-->>Client: Return vets list
    end
```

## External Dependencies

### Service Dependencies

| Service | Purpose | Configuration |
|---------|---------|---------------|
| Config Server | Centralized configuration management | Default: http://localhost:8888 |
| Eureka Server | Service discovery and registration | Spring Cloud Netflix Eureka |

### Cloud Integrations

| Integration | Purpose |
|-------------|---------|
| Azure Spring Cloud | Azure cloud platform integration |
| Azure MySQL | Managed MySQL database service |

## API Endpoints

| Endpoint | Method | Description | Cache |
|----------|--------|-------------|-------|
| /vets | GET | Get list of all veterinarians | Yes (vets) |

## Database Schema

### Tables

| Table | Description |
|-------|-------------|
| vets | Veterinarian information (id, first_name, last_name) |
| specialties | Specialty types |
| vet_specialties | Many-to-many relationship between vets and specialties |

## Configuration

### Application Properties

- **Application Name**: vets-service
- **Config Import**: Optional config server (http://localhost:8888/)
- **Cache Names**: vets
- **Active Profile**: production
- **Docker Profile**: Uses config-server:8888

### Ports

- **Exposed Port**: 8081 (from pom.xml docker configuration)

## Deployment

### Packaging

- **Type**: JAR
- **Docker Support**: Yes (maven docker plugin available)
- **Build Profile**: buildDocker

### Azure Readiness

Based on the assessment, this application can be deployed to:
- Azure Container Apps
- Azure Kubernetes Service (AKS)
- Azure App Service

---

*Generated from Spring PetClinic Vets Service assessment*
