# Architecture Diagram

This diagram illustrates the high-level architecture of the Spring PetClinic Vets Service, a Spring Boot 3.4.1 microservice that manages veterinarian data within the PetClinic microservices ecosystem.

## Application Architecture

```mermaid
flowchart TD
    Client["REST Client\n(API Consumer)"]

    subgraph VetsService["Vets Service (Spring Boot 3.4.1 / Java 17)"]
        subgraph WebLayer["Web Layer"]
            VetResource["VetResource\nREST Controller\nGET /vets"]
        end

        subgraph CacheLayer["Cache Layer"]
            CaffeineCache["Caffeine Cache\n(vets cache)"]
        end

        subgraph DataLayer["Data Access Layer"]
            VetRepository["VetRepository\nSpring Data JPA"]
            Vet["Vet Entity"]
            Specialty["Specialty Entity"]
        end

        subgraph SystemLayer["System Configuration"]
            CacheConfig["CacheConfig"]
            VetsProperties["VetsProperties"]
        end
    end

    subgraph ExternalServices["External Services"]
        ConfigServer["Spring Cloud Config Server\nhttp://config-server:8888"]
        EurekaServer["Netflix Eureka Server\nService Registry"]
    end

    subgraph DataStore["Data Storage"]
        MySQL["MySQL Database\n(Azure MySQL via spring-cloud-azure-starter-jdbc-mysql)"]
        HSQLDB["HSQLDB\n(In-Memory, Dev/Test)"]
    end

    subgraph Observability["Observability"]
        Actuator["Spring Boot Actuator\nHealth and Metrics"]
        Prometheus["Micrometer Prometheus\nMetrics Endpoint"]
        Jolokia["Jolokia\nJMX via HTTP"]
    end

    Client -->|"HTTP GET /vets"| VetResource
    VetResource -->|"check cache"| CaffeineCache
    CaffeineCache -->|"cache miss: query"| VetRepository
    VetRepository -->|"JPA queries"| Vet
    VetRepository -->|"JPA queries"| Specialty
    VetRepository -->|"JDBC"| MySQL
    VetRepository -->|"JDBC (dev/test)"| HSQLDB
    CacheConfig -->|"configures"| CaffeineCache
    VetsService -->|"fetch config on startup"| ConfigServer
    VetsService -->|"register and discover"| EurekaServer
    VetsService -->|"exposes"| Actuator
    Actuator -->|"metrics"| Prometheus
    Actuator -->|"JMX"| Jolokia
```
