# Architecture Diagram

This diagram shows the high-level architecture of the Spring PetClinic Vets Service, a Spring Boot microservice that manages veterinarian data.

## Application Architecture

```mermaid
flowchart TD
    Client["REST Client / API Consumer"]

    subgraph VetsService["Vets Service (Spring Boot 3.4.1 / Java 17)"]
        WebLayer["Web Layer\nVetResource REST Controller\nGET /vets"]
        CacheLayer["Cache Layer\nCaffeine Cache\ncache-names: vets"]
        BusinessLayer["Business Layer\nVetRepository\nSpring Data JPA"]
        ConfigLayer["Config\nVetsProperties\nCacheConfig"]
    end

    subgraph Infrastructure["Infrastructure / Cloud Services"]
        ConfigServer["Spring Cloud Config Server\nport 8888"]
        EurekaServer["Eureka Service Registry\nSpring Cloud Netflix"]
    end

    subgraph DataStore["Data Storage"]
        MySQL["MySQL\nProduction Database"]
        HSQLDB["HSQLDB\nIn-Memory for Development"]
    end

    subgraph AzureIntegration["Azure Integration"]
        AzureJDBC["Azure Spring Cloud\nJDBC MySQL Starter\nManaged Identity Auth"]
    end

    Client -->|HTTP GET /vets| WebLayer
    WebLayer -->|Cacheable lookup| CacheLayer
    CacheLayer -->|Cache miss - query| BusinessLayer
    BusinessLayer -->|JPA queries| AzureJDBC
    AzureJDBC -->|JDBC connection| MySQL
    BusinessLayer -.->|Dev/Test fallback| HSQLDB
    VetsService -->|Fetch config on startup| ConfigServer
    VetsService -->|Register and discover| EurekaServer
    ConfigLayer -->|Configures| CacheLayer
```
