# Spring PetClinic Vets Service

This is the Vets microservice for the Spring PetClinic application, built using Spring Boot and Spring Cloud. It provides veterinarian information and their specialties.

## Overview

The Vets Service is a microservice that manages veterinarian data within the PetClinic ecosystem. It provides REST endpoints to retrieve veterinarian information, including their names and specialties.

## Technology Stack

- **Java 17** - Programming language
- **Spring Boot 3.4.1** - Application framework
- **Spring Cloud 2024.0.0** - Microservices infrastructure
- **Spring Data JPA** - Data access layer
- **HSQLDB** - In-memory database (development)
- **MySQL** - Production database
- **Caffeine** - Caching library
- **Lombok** - Code generation library
- **Azure Spring Cloud** - Cloud deployment support

## Features

- RESTful API for veterinarian data
- Caching support for improved performance
- Spring Cloud Config integration
- Eureka service discovery
- MySQL and HSQLDB database support
- Actuator endpoints for monitoring
- Prometheus metrics integration
- Chaos Monkey for resilience testing

## API Endpoints

### Get All Vets
```
GET /vets
```

Returns a list of all veterinarians with their specialties.

**Response Example:**
```json
[
  {
    "id": 1,
    "firstName": "James",
    "lastName": "Carter",
    "specialties": []
  },
  {
    "id": 2,
    "firstName": "Helen",
    "lastName": "Leary",
    "specialties": [
      {
        "id": 1,
        "name": "radiology"
      }
    ]
  }
]
```

## Configuration

### Application Properties

The service can be configured through `application.yml`:

```yaml
spring:
  application:
    name: vets-service
  config:
    import: optional:configserver:${CONFIG_SERVER_URL:http://localhost:8888/}
  cache:
    cache-names: vets
  profiles:
    active: production
```

### Custom Configuration Properties

The service supports custom configuration through the `vets` prefix:

```yaml
vets:
  cache:
    ttl: 60        # Cache time-to-live in seconds
    heapSize: 100  # Maximum cache size
```

## Building and Running

### Prerequisites

- Java 17 or higher
- Maven 3.6 or higher

### Build

```bash
mvn clean install
```

### Run Locally

```bash
mvn spring-boot:run
```

The service will start on port 8081 (default).

### Run Tests

```bash
mvn test
```

## Database

### HSQLDB (Development)

By default, the service uses HSQLDB for development. The schema and initial data are loaded from:
- `src/main/resources/db/hsqldb/schema.sql`
- `src/main/resources/db/hsqldb/data.sql`

### MySQL (Production)

For production, configure MySQL connection through Spring Cloud Config or environment variables.

## Docker

The service can be containerized using the Docker Maven plugin:

```bash
mvn clean install -PbuildDocker
```

## Monitoring

### Actuator Endpoints

The service exposes various actuator endpoints:
- `/actuator/health` - Health check
- `/actuator/info` - Application information
- `/actuator/metrics` - Metrics
- `/actuator/prometheus` - Prometheus metrics

### Chaos Monkey

Chaos Monkey for Spring Boot is integrated to test system resilience. It can be enabled through configuration.

## Service Discovery

The service registers with Eureka service registry for discovery by other microservices in the PetClinic ecosystem.

## Caching

The `/vets` endpoint is cached using Caffeine cache to improve performance. The cache is enabled in the `production` profile.

## Architecture

### Model Classes

- **Vet** - Represents a veterinarian with first name, last name, and specialties
- **Specialty** - Represents a veterinarian specialty (e.g., radiology, surgery, dentistry)

### Repository

- **VetRepository** - JPA repository for Vet entity operations

### REST Controller

- **VetResource** - REST controller exposing the `/vets` endpoint

## Dependencies

Key dependencies and their versions:
- Spring Boot: 3.4.1
- Spring Cloud: 2024.0.0
- Chaos Monkey: 3.2.2
- Jolokia: 1.7.2
- Azure Spring Cloud: 5.20.1

## License

Copyright 2002-2021 the original author or authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
