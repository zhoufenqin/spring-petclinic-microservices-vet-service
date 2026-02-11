# Spring PetClinic Vets Service - Architecture Diagram

## Overview

| Property | Value |
|----------|-------|
| Application Name | vets-service |
| Application Type | Microservice (REST API) |
| Framework | Spring Boot 3.4.1 |
| Java Version | 17 |
| Build Tool | Maven |

## Application Architecture

### High-Level Architecture

```mermaid
graph TB
    Client[External Clients]
    ConfigServer[Config Server]
    EurekaServer[Eureka Discovery Server]
    
    VetsService[Vets Service<br/>Spring Boot 3.4.1]
    
    Cache[Caffeine Cache<br/>vets cache]
    Database[(Database<br/>HSQLDB/MySQL)]
    
    Client -->|REST API /vets| VetsService
    VetsService -->|Service Registration| EurekaServer
    VetsService -->|Configuration| ConfigServer
    VetsService -->|Cache Read/Write| Cache
    VetsService -->|JPA/JDBC| Database
    
    style VetsService fill:#4CAF50,stroke:#2E7D32,color:#fff
    style Cache fill:#FFC107,stroke:#F57C00,color:#000
    style Database fill:#2196F3,stroke:#1565C0,color:#fff
    style ConfigServer fill:#9E9E9E,stroke:#424242,color:#fff
    style EurekaServer fill:#9E9E9E,stroke:#424242,color:#fff
```

### Layered Architecture

```mermaid
graph TB
    subgraph "Presentation Layer"
        REST[REST Controller<br/>VetResource]
    end
    
    subgraph "Business Layer"
        Cache[Cache Management<br/>Caffeine]
        Config[Configuration<br/>VetsProperties]
    end
    
    subgraph "Data Access Layer"
        Repository[VetRepository<br/>JPA Repository]
        Model[Domain Models<br/>Vet, Specialty]
    end
    
    subgraph "Infrastructure"
        DB[(Database<br/>HSQLDB/MySQL)]
        Discovery[Service Discovery<br/>Eureka Client]
        ConfigSrv[Config Client<br/>Spring Cloud Config]
    end
    
    REST --> Cache
    Cache --> Repository
    Repository --> Model
    Model --> DB
    REST --> Discovery
    REST --> ConfigSrv
    
    style REST fill:#4CAF50,stroke:#2E7D32,color:#fff
    style Cache fill:#FFC107,stroke:#F57C00,color:#000
    style Repository fill:#2196F3,stroke:#1565C0,color:#fff
    style DB fill:#1976D2,stroke:#0D47A1,color:#fff
```

## Code Structure

### Component Overview

| Component | Type | Purpose |
|-----------|------|---------|
| VetResource | REST Controller | Exposes /vets endpoint for retrieving veterinarians |
| VetRepository | JPA Repository | Data access layer for Vet entities |
| Vet | Entity | Domain model representing a veterinarian |
| Specialty | Entity | Domain model representing veterinarian specialties |
| CacheConfig | Configuration | Configures Caffeine cache for vets data |
| VetsProperties | Configuration | Application-specific configuration properties |
| VetsServiceApplication | Main Class | Spring Boot application entry point |

### Folder Structure

```
src/main/java/org/springframework/samples/petclinic/vets/
├── model/              # Domain models and repositories
│   ├── Vet.java
│   ├── Specialty.java
│   └── VetRepository.java
├── web/                # REST controllers
│   └── VetResource.java
├── system/             # System configuration
│   ├── CacheConfig.java
│   └── VetsProperties.java
└── VetsServiceApplication.java
```

## Technology Stack

### Core Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| Java | 17 | Runtime platform |
| Spring Boot | 3.4.1 | Application framework |
| Spring Cloud | 2024.0.0 | Microservices infrastructure |
| Maven | - | Build and dependency management |

### Key Dependencies

| Dependency | Version/Type | Purpose |
|------------|--------------|---------|
| Spring Boot Starter Web | 3.4.1 | REST API development |
| Spring Boot Starter Data JPA | 3.4.1 | Data persistence with JPA |
| Spring Boot Starter Cache | 3.4.1 | Caching abstraction |
| Spring Boot Starter Actuator | 3.4.1 | Production monitoring and management |
| Spring Cloud Config | 2024.0.0 | Centralized configuration management |
| Spring Cloud Netflix Eureka | 2024.0.0 | Service discovery and registration |
| Azure Spring Cloud JDBC MySQL | 5.20.1 | Azure MySQL connectivity with managed identity |
| Caffeine | - | High-performance in-memory cache |
| HSQLDB | runtime | In-memory database for development |
| MySQL Connector | runtime | MySQL database connectivity |
| Lombok | provided | Reduce boilerplate code |
| Micrometer Prometheus | - | Metrics collection and export |
| Chaos Monkey | 3.1.0 | Chaos engineering for resilience testing |

### Data Storage

| Type | Technology | Usage |
|------|------------|-------|
| Primary Database | HSQLDB/MySQL | Veterinarian and specialty data persistence |
| Cache | Caffeine | In-memory caching of vets list |

### External Integrations

| Service | Purpose | Protocol |
|---------|---------|----------|
| Config Server | Centralized configuration management | HTTP/REST |
| Eureka Server | Service discovery and registration | HTTP/REST |
| Azure MySQL | Managed database service | JDBC |

## Data Flow

### Typical Request Flow

1. **Client Request**: External client sends GET request to `/vets`
2. **Cache Check**: VetResource checks Caffeine cache for cached vets list
3. **Cache Hit**: If cached, return immediately from cache
4. **Cache Miss**: If not cached, query VetRepository
5. **Database Query**: VetRepository executes JPA query against database
6. **Cache Update**: Result is cached in Caffeine for subsequent requests
7. **Response**: JSON response returned to client

### Service Registration Flow

1. **Startup**: VetsServiceApplication starts
2. **Configuration**: Fetch configuration from Config Server
3. **Registration**: Register service instance with Eureka Server
4. **Health Checks**: Periodic health checks sent to Eureka
5. **Discovery**: Other services can discover this service via Eureka

## Migration Considerations

Based on the assessment report, the following areas require attention for Azure migration:

### Identified Issues (7 issues, 11 incidents, 35 effort points)

**By Severity:**
- Mandatory: 6 incidents
- Optional: 4 incidents
- Potential: 1 incident

**By Category:**
- Remote Communication: 4 incidents
- Embedded Cache Management: 3 incidents
- Spring Migration: 2 incidents
- Framework Upgrade: 1 incident
- Containerization: 1 incident

### Key Migration Areas

1. **Config Server Migration**: Replace Spring Cloud Config with Azure App Configuration
2. **Service Discovery**: Replace Eureka with Azure-native service discovery or Azure Service Bus
3. **Cache Management**: Consider Azure Cache for Redis for distributed caching
4. **Database**: Migrate to Azure Database for MySQL with managed identity
5. **Monitoring**: Integrate with Azure Application Insights
6. **Containerization**: Prepare for deployment on AKS, App Service, or Container Apps

---

*Generated from assessment results on 2026-02-11*
