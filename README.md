# Spring PetClinic Vets Service

A microservice for managing veterinarians and their specialties in the Spring PetClinic application.

## Overview

The Vets Service is a Spring Boot microservice that provides REST API endpoints for managing veterinarian information, including their personal details and medical specialties. This service is part of the Spring PetClinic microservices architecture.

## Features

- RESTful API for veterinarian management
- Specialty tracking for each veterinarian (radiology, surgery, dentistry)
- Caching support for improved performance
- Service discovery with Eureka
- Configuration management with Spring Cloud Config
- Support for MySQL and HSQLDB databases
- Azure cloud integration ready
- Actuator endpoints for monitoring
- Prometheus metrics support

## Technology Stack

- **Java**: 17
- **Spring Boot**: 3.4.1
- **Spring Cloud**: 2024.0.0
- **Database**: MySQL (production) / HSQLDB (development)
- **Caching**: Caffeine
- **Build Tool**: Maven
- **Cloud Platform**: Azure (with Spring Cloud Azure support)

## Prerequisites

- Java 17 or higher
- Maven 3.6+
- MySQL 8.0+ (for production profile)

## Building the Application

```bash
mvn clean install
```

## Running the Application

### With HSQLDB (Development)

```bash
mvn spring-boot:run
```

### With MySQL (Production)

1. Ensure MySQL is running and accessible
2. Configure the database connection in your application properties or through Spring Cloud Config Server
3. Run the application:

```bash
mvn spring-boot:run -Dspring-boot.run.profiles=production
```

### With Docker Profile

```bash
mvn spring-boot:run -Dspring-boot.run.profiles=docker
```

## API Endpoints

### Get All Veterinarians

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

The service uses Spring Cloud Config Server for external configuration. By default, it connects to:

- **Config Server URL**: `http://localhost:8888/` (can be overridden with `CONFIG_SERVER_URL` environment variable)
- **Docker Profile**: Uses `http://config-server:8888`

### Key Configuration Properties

- `spring.application.name`: vets-service
- `spring.cache.cache-names`: vets
- `spring.profiles.active`: production (default)

## Database Schema

The service manages three main tables:

- **vets**: Veterinarian information (id, first_name, last_name)
- **specialties**: Medical specialties (id, name)
- **vet_specialties**: Many-to-many relationship between vets and specialties

## Monitoring and Actuator

The service includes Spring Boot Actuator with support for:

- Health checks
- Prometheus metrics
- Jolokia JMX-HTTP bridge
- Application info

## Azure Integration

The service includes Azure-specific dependencies:

- Spring Cloud Azure JDBC MySQL starter
- Support for Azure-managed database connections

## Testing

Run the test suite:

```bash
mvn test
```

## Docker Support

The project includes Docker build support via Maven plugin. To build a Docker image:

```bash
mvn clean install -PbuildDocker
```

## Service Discovery

This service is configured to register with Eureka service registry (`@EnableDiscoveryClient`). Ensure your Eureka server is running before starting this service in a microservices environment.

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

## Authors

- Maciej Szarlinski
- Juergen Hoeller
- Mark Fisher
- Ken Krebs
- Arjen Poutsma
- Ramazan Sakin
