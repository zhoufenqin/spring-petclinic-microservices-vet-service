# Architecture Diagram

Application architecture showing logical layers and data flow.

## Application Architecture

The following diagram shows the logical layers and their relationships within the application.

```mermaid
graph TD
    Client["Client / API Consumer"]

    subgraph Presentation["Presentation Layer"]
        REST["REST Controller"]
    end

    subgraph BusinessLogic["Business Logic Layer"]
        Cache["Cache Management"]
        Config["Configuration and Properties"]
    end

    subgraph DataAccess["Data Access Layer"]
        Repository["Repository"]
        Entities["Domain Entities"]
    end

    subgraph DataStore["Data Store"]
        DB["Relational Database"]
    end

    Client -->|"HTTP request"| REST
    REST -->|"check cache"| Cache
    Cache -->|"cache miss, fetch data"| Repository
    Repository -->|"query"| Entities
    Entities -->|"persist and retrieve"| DB
    Cache -->|"cached response"| REST
    Config -->|"configure"| Cache
```
