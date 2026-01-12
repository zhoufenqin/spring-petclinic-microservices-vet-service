# Spring PetClinic Vets Service - Architecture Documentation

## Overview
This is the Vets Service microservice from the Spring PetClinic application. It manages veterinarian information and their specialties.

## Technology Stack
- **Framework**: Spring Boot 3.4.1
- **Language**: Java 17
- **Build Tool**: Maven
- **Database**: HSQLDB (default), MySQL (runtime)
- **Cloud Integration**: Spring Cloud, Netflix Eureka, Azure Spring Cloud
- **Cache**: Caffeine
- **Monitoring**: Micrometer (Prometheus), Actuator, Jolokia

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                    Spring PetClinic Vets Service                     │
└─────────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────────┐
│                           Application Layer                          │
├─────────────────────────────────────────────────────────────────────┤
│  VetsServiceApplication                                              │
│  - @SpringBootApplication                                            │
│  - @EnableDiscoveryClient (Eureka)                                   │
│  - Main entry point                                                  │
└─────────────────────────────────────────────────────────────────────┘
                                    │
        ┌───────────────────────────┼───────────────────────────┐
        │                           │                           │
        ▼                           ▼                           ▼
┌──────────────┐          ┌──────────────────┐       ┌─────────────────┐
│   Web Layer  │          │  System Config   │       │  Model Layer    │
├──────────────┤          ├──────────────────┤       ├─────────────────┤
│ VetResource  │          │ CacheConfig      │       │ Vet             │
│ - REST API   │          │ VetsProperties   │       │ Specialty       │
│ - @GetMapping│          │                  │       │ VetRepository   │
│   /vets      │          │                  │       │                 │
│ - Cacheable  │          │                  │       │                 │
└──────────────┘          └──────────────────┘       └─────────────────┘
        │                                                      │
        └──────────────────────┬───────────────────────────────┘
                               ▼
                    ┌──────────────────────┐
                    │   Data Access Layer  │
                    ├──────────────────────┤
                    │ Spring Data JPA      │
                    │ VetRepository        │
                    │ (JpaRepository)      │
                    └──────────────────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │     Database         │
                    ├──────────────────────┤
                    │ HSQLDB (dev/test)    │
                    │ MySQL (production)   │
                    │                      │
                    │ Tables:              │
                    │ - vets               │
                    │ - specialties        │
                    │ - vet_specialties    │
                    └──────────────────────┘
```

## Component Details

### 1. Web Layer
**Package**: `org.springframework.samples.petclinic.vets.web`

- **VetResource**: REST controller exposing veterinarian data
  - Endpoint: `GET /vets`
  - Returns: List of all veterinarians with their specialties
  - Caching: Uses `@Cacheable("vets")` annotation

### 2. Model Layer
**Package**: `org.springframework.samples.petclinic.vets.model`

- **Vet**: Entity representing a veterinarian
  - Fields: id, firstName, lastName
  - Relationships: Many-to-Many with Specialty
  
- **Specialty**: Entity representing a veterinary specialty
  - Fields: id, name
  - Examples: dentistry, surgery, radiology

- **VetRepository**: JPA repository interface for Vet entities
  - Extends: JpaRepository<Vet, Integer>
  - Provides: CRUD operations

### 3. System Configuration
**Package**: `org.springframework.samples.petclinic.vets.system`

- **CacheConfig**: Enables caching for production profile
  - `@EnableCaching`
  - `@Profile("production")`

- **VetsProperties**: Configuration properties for the service
  - Prefix: `vets`
  - Properties: cache TTL and heap size

### 4. Data Model Relationships

```
┌────────────┐                ┌──────────────────┐                ┌─────────────┐
│    Vet     │                │ vet_specialties  │                │  Specialty  │
├────────────┤                │  (join table)    │                ├─────────────┤
│ id (PK)    │◄───────────────┤ vet_id (FK)      │                │ id (PK)     │
│ first_name │                │ specialty_id (FK)├───────────────►│ name        │
│ last_name  │                └──────────────────┘                └─────────────┘
└────────────┘
    1                                                                     *
    │                                                                     │
    └─────────────────── Many-to-Many ───────────────────────────────────┘
```

## Directory Structure

```
spring-petclinic-microservices-vet-service/
├── pom.xml                                  # Maven build configuration
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── org/springframework/samples/petclinic/vets/
│   │   │       ├── VetsServiceApplication.java        # Main application class
│   │   │       ├── model/                             # Domain model
│   │   │       │   ├── Vet.java
│   │   │       │   ├── Specialty.java
│   │   │       │   └── VetRepository.java
│   │   │       ├── web/                               # REST controllers
│   │   │       │   └── VetResource.java
│   │   │       └── system/                            # Configuration
│   │   │           ├── CacheConfig.java
│   │   │           └── VetsProperties.java
│   │   └── resources/
│   │       ├── application.yml                        # Application config
│   │       ├── logback-spring.xml                     # Logging config
│   │       └── db/                                    # Database scripts
│   │           ├── hsqldb/
│   │           │   ├── schema.sql
│   │           │   └── data.sql
│   │           └── mysql/
│   │               ├── schema.sql
│   │               └── data.sql
│   └── test/
│       ├── java/
│       │   └── org/springframework/samples/petclinic/vets/web/
│       │       └── VetResourceTest.java               # REST API tests
│       └── resources/
└── .github/                                           # GitHub workflows & agents
```

## Key Features

### Service Discovery
- Integrates with Netflix Eureka for service registration and discovery
- Enabled via `@EnableDiscoveryClient` annotation

### Configuration Management
- Uses Spring Cloud Config Server
- Default URL: http://localhost:8888/
- Docker profile uses: http://config-server:8888

### Caching
- Uses Caffeine cache implementation
- Configurable TTL and heap size
- Enabled in production profile

### Database Support
- **Development/Test**: HSQLDB (in-memory)
- **Production**: MySQL with Azure JDBC support
- JPA/Hibernate for ORM

### Monitoring & Observability
- Spring Boot Actuator endpoints
- Prometheus metrics via Micrometer
- Jolokia for JMX over HTTP
- Chaos Monkey for resilience testing

## API Endpoints

| Method | Endpoint | Description                    | Response       |
|--------|----------|--------------------------------|----------------|
| GET    | /vets    | Get all veterinarians          | List<Vet>      |

## Database Schema

### vets
| Column     | Type         | Constraints    |
|------------|--------------|----------------|
| id         | INTEGER      | PRIMARY KEY    |
| first_name | VARCHAR(30)  | NOT NULL       |
| last_name  | VARCHAR(30)  | NOT NULL       |

### specialties
| Column | Type         | Constraints    |
|--------|--------------|----------------|
| id     | INTEGER      | PRIMARY KEY    |
| name   | VARCHAR(80)  | NOT NULL       |

### vet_specialties
| Column        | Type    | Constraints                  |
|---------------|---------|------------------------------|
| vet_id        | INTEGER | FOREIGN KEY (vets.id)        |
| specialty_id  | INTEGER | FOREIGN KEY (specialties.id) |

## Configuration Properties

### Application Configuration (application.yml)
```yaml
spring:
  application:
    name: vets-service
  config:
    import: optional:configserver:${CONFIG_SERVER_URL}
  cache:
    cache-names: vets
  profiles:
    active: production
```

## Build & Run

### Build
```bash
mvn clean package
```

### Run
```bash
mvn spring-boot:run
```

### Docker Build
```bash
mvn clean package -PbuildDocker
```

## Dependencies Overview

### Spring Boot Starters
- spring-boot-starter-web
- spring-boot-starter-actuator
- spring-boot-starter-cache
- spring-boot-starter-data-jpa
- spring-boot-starter-test

### Spring Cloud
- spring-cloud-starter-config
- spring-cloud-starter-netflix-eureka-client

### Database
- HSQLDB (runtime)
- MySQL Connector (runtime)
- Azure JDBC Starter (MySQL)

### Caching & Performance
- Caffeine
- javax.cache API

### Monitoring
- Micrometer (Prometheus registry)
- Jolokia
- Chaos Monkey for Spring Boot

### Utilities
- Lombok (code generation)
- Jakarta XML Bind
