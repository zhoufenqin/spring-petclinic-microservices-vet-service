# Architecture Diagram

Application architecture showing logical layers and data flow for the veterinarian management microservice.

## Application Architecture

This diagram shows the logical layers of the application and how requests flow through them.

```mermaid
graph TD
    A["Presentation Layer\nREST API Endpoint"] -->|"request"| B["Caching Layer\nIn-Memory Cache"]
    B -->|"cache miss"| C["Data Access Layer\nRepository"]
    C -->|"query"| D["Data Layer\nRelational Database"]
    E["Configuration Layer\nApp Configuration"] -->|"configures"| B
    E -->|"configures"| C
```
