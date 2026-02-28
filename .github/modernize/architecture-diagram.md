# Architecture Diagram

Application architecture showing logical layers and data flow for the veterinarian microservice.

## Application Architecture

The following diagram shows the logical layers and their relationships within the application.

```mermaid
graph TD
    Client["Client / API Consumer"]

    subgraph Presentation["Presentation Layer"]
        REST["REST Controller\n/vets endpoint"]
    end

    subgraph BusinessLogic["Business Logic Layer"]
        Cache["Cache Manager"]
        Config["Configuration\nand Properties"]
    end

    subgraph DataAccess["Data Access Layer"]
        Repository["Vet Repository"]
        Entities["Domain Entities\nVet, Specialty"]
    end

    subgraph DataStore["Data Store"]
        DB["Relational Database"]
    end

    Client -->|"HTTP GET request"| REST
    REST -->|"check cache"| Cache
    Cache -->|"cache miss - fetch data"| Repository
    Repository -->|"query entities"| Entities
    Entities -->|"persist and retrieve"| DB
    Cache -->|"return cached response"| REST
    Config -->|"configure"| Cache
```
