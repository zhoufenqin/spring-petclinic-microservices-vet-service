# Architecture Diagram

The Spring PetClinic Vets Service is a Spring Boot microservice responsible for managing veterinarian data, exposing a REST API for vet listings with in-memory caching, backed by a relational database.

## Application Architecture

```mermaid
flowchart TD
    Client["HTTP Client\n(Other Microservices / API Gateway)"]

    subgraph ExternalInfra["External Infrastructure"]
        ConfigServer["Spring Cloud Config Server\nport 8888"]
        Eureka["Netflix Eureka\nService Registry"]
        Prometheus["Prometheus\nMetrics Scraper"]
    end

    subgraph VetsService["Vets Service  Java 17  Spring Boot 3.4.1"]
        subgraph WebLayer["Web Layer"]
            VetResource["VetResource\nREST Controller\nGET /vets"]
        end

        subgraph BusinessLayer["Business / Config Layer"]
            CacheConfig["CacheConfig\nCaffeine Cache"]
            VetsProperties["VetsProperties\nApp Configuration"]
            Actuator["Spring Boot Actuator\nHealth  Metrics  Jolokia"]
            ChaosMonkey["Chaos Monkey\nResilience Testing"]
        end

        subgraph DataLayer["Data Access Layer"]
            VetRepository["VetRepository\nSpring Data JPA"]
            VetEntity["Vet Entity\nSpecialty Entity"]
        end
    end

    subgraph DataStorage["Data Storage"]
        MySQL["MySQL\nProduction Database\nAzure JDBC Starter"]
        HSQLDB["HSQLDB\nEmbedded  Test only"]
    end

    Client -->|"REST GET /vets"| VetResource
    VetResource -->|"cache lookup"| CacheConfig
    VetResource -->|"findAll"| VetRepository
    VetRepository -->|"JPA queries"| VetEntity
    VetEntity -->|"reads/writes"| MySQL
    VetEntity -.->|"reads/writes test"| HSQLDB

    VetsService -->|"fetch config"| ConfigServer
    VetsService -->|"register and discover"| Eureka
    Actuator -->|"expose metrics endpoint"| Prometheus
```
