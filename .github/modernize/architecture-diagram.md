# Architecture Diagram

The Spring PetClinic Vets Service is a Spring Boot microservice that exposes a REST API for managing veterinarian data, backed by a relational database and integrated with the Spring Cloud ecosystem.

## System Context

This diagram shows how the Vets Service interacts with external systems in the PetClinic microservices landscape.

```mermaid
graph TD
    Client["API Gateway / UI Client"]
    VetsService["Vets Service\n(Spring Boot :8081)"]
    ConfigServer["Config Server\n(Spring Cloud Config)"]
    Eureka["Service Registry\n(Netflix Eureka)"]
    MySQL["MySQL Database\n(petclinic)"]

    Client -->|"GET /vets"| VetsService
    VetsService -->|"fetch config on startup"| ConfigServer
    VetsService -->|"register and discover"| Eureka
    VetsService -->|"read vet and specialty data"| MySQL
```

## Container View

This diagram shows the major containers inside the Vets Service and how they interact with each other and with external dependencies.

```mermaid
graph TD
    subgraph VetsService["Vets Service Container"]
        REST["REST Layer\nVetResource"]
        Cache["In-Memory Cache\nCaffeine"]
        BL["Domain Model\nVet, Specialty"]
        DA["Data Access Layer\nVetRepository (JPA)"]
    end

    MySQL["MySQL Database"]
    HSQL["HSQLDB\n(embedded, dev/test)"]
    ConfigServer["Config Server"]
    Eureka["Eureka Service Registry"]
    Prometheus["Prometheus\n(metrics scrape)"]

    REST -->|"cached lookup"| Cache
    Cache -->|"cache miss"| DA
    DA -->|"JPA over JDBC"| BL
    DA -->|"production"| MySQL
    DA -->|"dev and test"| HSQL
    REST -->|"exposes metrics"| Prometheus
    VetsService -->|"loads config"| ConfigServer
    VetsService -->|"registers"| Eureka
```

## Component View

This diagram shows the internal components of the Vets Service and their relationships.

```mermaid
graph TD
    subgraph WebLayer["Web Layer"]
        VetResource["VetResource\nGET /vets"]
    end

    subgraph DomainLayer["Domain Layer"]
        Vet["Vet\n(Entity)"]
        Specialty["Specialty\n(Entity)"]
        VetRepository["VetRepository\n(JpaRepository)"]
    end

    subgraph SystemLayer["System Layer"]
        CacheConfig["CacheConfig\n(Caffeine cache)"]
        VetsProperties["VetsProperties\n(config props)"]
    end

    VetResource -->|"findAll"| VetRepository
    VetResource -->|"cacheable vets"| CacheConfig
    VetRepository -->|"manages"| Vet
    Vet -->|"has many"| Specialty
    VetsProperties -->|"configures"| CacheConfig
```
