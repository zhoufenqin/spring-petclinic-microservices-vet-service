# Architecture Diagram - Spring PetClinic Vets Service

## Overview

| Property | Value |
|----------|-------|
| **Application Name** | Spring PetClinic Vets Service |
| **Application Type** | Microservice / REST API |
| **Framework** | Spring Boot 3.4.1 |
| **Java Version** | 17 |
| **Build Tool** | Maven |
| **Packaging** | JAR |

## Application Architecture

### High-Level Architecture

```mermaid
graph TB
    subgraph "Vets Service Microservice"
        API[REST API Layer<br/>VetResource]
        BIZ[Business Logic Layer<br/>Service Components]
        DATA[Data Access Layer<br/>VetRepository]
        CACHE[Cache Layer<br/>Caffeine Cache]
    end
    
    subgraph "External Dependencies"
        EUREKA[Service Discovery<br/>Eureka Client]
        CONFIG[Configuration Server<br/>Spring Cloud Config]
        DB[(Database<br/>MySQL / HSQLDB)]
    end
    
    CLIENT[API Clients] --> API
    API --> CACHE
    CACHE --> BIZ
    BIZ --> DATA
    DATA --> DB
    
    API --> EUREKA
    API --> CONFIG
    
    style API fill:#e1f5ff
    style BIZ fill:#fff4e1
    style DATA fill:#e8f5e9
    style CACHE fill:#f3e5f5
    style DB fill:#ffebee
    style EUREKA fill:#fff9c4
    style CONFIG fill:#fff9c4
```

### Layered Architecture

```mermaid
graph LR
    subgraph "Presentation Layer"
        REST[REST Controllers<br/>VetResource]
    end
    
    subgraph "Business Layer"
        MODEL[Domain Models<br/>Vet, Specialty]
    end
    
    subgraph "Data Layer"
        REPO[JPA Repositories<br/>VetRepository]
        ENTITY[JPA Entities]
    end
    
    subgraph "Infrastructure"
        CACHE2[Caching<br/>Spring Cache + Caffeine]
        CONFIG2[Configuration<br/>Spring Cloud Config]
        DISC[Service Discovery<br/>Eureka Client]
        MON[Monitoring<br/>Actuator + Prometheus]
    end
    
    REST --> MODEL
    MODEL --> REPO
    REPO --> ENTITY
    REST --> CACHE2
    REST --> CONFIG2
    REST --> DISC
    REST --> MON
    
    style REST fill:#e1f5ff
    style MODEL fill:#fff4e1
    style REPO fill:#e8f5e9
    style ENTITY fill:#e8f5e9
    style CACHE2 fill:#f3e5f5
    style CONFIG2 fill:#fff9c4
    style DISC fill:#fff9c4
    style MON fill:#ffe0b2
```

## Code Structure

### Component Overview

| Component | Package | Purpose |
|-----------|---------|---------|
| **VetsServiceApplication** | `vets` | Main application entry point |
| **VetResource** | `vets.web` | REST controller for vet endpoints |
| **VetRepository** | `vets.model` | JPA repository for data access |
| **Vet** | `vets.model` | Domain model for veterinarian |
| **Specialty** | `vets.model` | Domain model for vet specialties |
| **CacheConfig** | `vets.system` | Cache configuration |
| **VetsProperties** | `vets.system` | Application properties |

### Folder Structure

```
src/main/java/org/springframework/samples/petclinic/vets/
├── VetsServiceApplication.java         # Main application class
├── model/                               # Domain models and repositories
│   ├── Vet.java
│   ├── Specialty.java
│   └── VetRepository.java
├── web/                                 # REST controllers
│   └── VetResource.java
└── system/                              # System configuration
    ├── CacheConfig.java
    └── VetsProperties.java

src/main/resources/
├── application.yml                      # Application configuration
├── db/                                  # Database schemas and data
│   ├── hsqldb/
│   └── mysql/
└── logback-spring.xml                   # Logging configuration
```

## Technology Stack

### Core Technologies

| Technology | Version | Purpose |
|------------|---------|---------|
| **Spring Boot** | 3.4.1 | Application framework |
| **Java** | 17 | Programming language |
| **Maven** | - | Build tool |
| **Spring Cloud** | 2024.0.0 | Microservices support |

### Spring Ecosystem

| Component | Purpose |
|-----------|---------|
| **Spring Boot Starter Web** | REST API support |
| **Spring Boot Starter Data JPA** | Data access layer |
| **Spring Boot Starter Cache** | Caching abstraction |
| **Spring Boot Starter Actuator** | Health checks and metrics |
| **Spring Cloud Config** | Centralized configuration |
| **Spring Cloud Netflix Eureka Client** | Service discovery |

### Data & Caching

| Technology | Version | Purpose |
|------------|---------|---------|
| **MySQL Connector** | Runtime | MySQL database driver |
| **HSQLDB** | Runtime | In-memory database for testing |
| **Azure JDBC Starter for MySQL** | 5.20.1 | Azure MySQL integration |
| **Caffeine** | - | High-performance caching library |
| **javax.cache API** | - | JSR-107 caching specification |

### Monitoring & Observability

| Technology | Purpose |
|------------|---------|
| **Micrometer Prometheus** | Metrics export to Prometheus |
| **Spring Boot Actuator** | Health endpoints and metrics |
| **Jolokia** | JMX-HTTP bridge |
| **Chaos Monkey** | Resilience testing |

### Additional Libraries

| Library | Version | Purpose |
|---------|---------|---------|
| **Lombok** | - | Code generation (reduce boilerplate) |
| **Jakarta XML Bind API** | - | XML binding |
| **JUnit Jupiter** | - | Unit testing framework |

## Data Flow

```mermaid
sequenceDiagram
    participant Client
    participant REST as VetResource
    participant Cache as Cache Layer
    participant Repo as VetRepository
    participant DB as Database
    
    Client->>REST: GET /vets
    REST->>Cache: Check cache
    alt Cache Hit
        Cache-->>REST: Return cached data
    else Cache Miss
        REST->>Repo: findAll()
        Repo->>DB: SELECT * FROM vets
        DB-->>Repo: Vet records
        Repo-->>REST: List of Vets
        REST->>Cache: Store in cache
    end
    REST-->>Client: JSON response
```

## External Integrations

### Service Dependencies

```mermaid
graph TB
    VETS[Vets Service]
    
    VETS -->|Registers with| EUREKA[Eureka Server<br/>Service Registry]
    VETS -->|Fetches config from| CONFIG[Config Server<br/>Configuration Management]
    VETS -->|Persists data to| MYSQL[(MySQL Database<br/>or HSQLDB)]
    VETS -->|Exports metrics to| PROM[Prometheus<br/>Monitoring]
    
    style VETS fill:#e1f5ff
    style EUREKA fill:#fff9c4
    style CONFIG fill:#fff9c4
    style MYSQL fill:#ffebee
    style PROM fill:#ffe0b2
```

### Integration Points

| Service | Protocol | Purpose |
|---------|----------|---------|
| **Eureka Server** | HTTP | Service registration and discovery |
| **Config Server** | HTTP | Centralized configuration management |
| **Database** | JDBC | Data persistence (MySQL or HSQLDB) |
| **Prometheus** | HTTP | Metrics collection and monitoring |

## Deployment Considerations

### Port Configuration

- **Application Port**: 8081 (Docker exposed port)
- **Actuator Endpoints**: Available via Spring Boot Actuator

### Database Support

The application supports multiple database configurations:

1. **HSQLDB** (Development/Testing)
   - In-memory database
   - Schema and data scripts in `db/hsqldb/`

2. **MySQL** (Production)
   - Azure MySQL integration via Azure JDBC Starter
   - Schema and data scripts in `db/mysql/`

### Profiles

- **Default Profile**: `production`
- **Docker Profile**: Uses Config Server at `http://config-server:8888`

### Spring Cloud Integration

- **Service Discovery**: Registers with Eureka for service discovery
- **Configuration**: Fetches configuration from Config Server
- **Config Server URL**: Configurable via `CONFIG_SERVER_URL` environment variable (default: `http://localhost:8888/`)

## Assessment Findings Summary

Based on the AppCAT assessment, the following key findings were identified:

### Mandatory Issues (6 locations)
- **Caching**: Spring Boot Cache library usage (3 locations)
- **Network Protocols**: Use of unsecured network protocols (2 locations)
- **Containerization**: No Dockerfile found (1 location)

### Potential Issues (1 location)
- **Eureka Client**: Embedded framework dependency (1 location)

### Optional Issues (4 locations)
- **Spring Cloud Config**: Usage detected (1 location)
- **Jakarta EE**: Version not latest stable (1 location)
- **Hardcoded URLs**: HTTP protocol URLs in source code (2 locations)

For detailed assessment results, refer to the assessment report at `.github/modernize/report.json`.
