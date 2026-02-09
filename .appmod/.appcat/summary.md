# App Modernization Assessment Summary

**Target Azure Services**: Azure Kubernetes Service, Azure App Service, Azure Container Apps

## Overall Statistics

**Total Applications**: 1

**Name: vets-service**
- Mandatory: 3 issues
- Potential: 1 issues
- Optional: 3 issues

> **Severity Levels Explained:**
> - **Mandatory**: The issue has to be resolved for the migration to be successful.
> - **Potential**: This issue may be blocking in some situations but not in others. These issues should be reviewed to determine whether a change is required or not.
> - **Optional**: The issue discovered is real issue fixing which could improve the app after migration, however it is not blocking.

## Applications Profile

### Name: vets-service
- **JDK Version**: 17
- **Frameworks**: Spring Boot, Spring Cloud, Spring
- **Languages**: Java
- **Build Tools**: Maven

**Key Findings**:
- **Mandatory Issues (6 locations)**:
  - <!--ruleid=dockerfile-00000-->No Dockerfile found (1 location found)
  - <!--ruleid=embedded-cache-15000-->Caching - Spring Boot Cache library (3 locations found)
  - <!--ruleid=unsecure-network-protocol-00000-->Use of unsecured network protocols or URI libraries (2 locations found)
- **Potential Issues (1 locations)**:
  - <!--ruleid=spring-boot-to-azure-eureka-02000-->Embedded framework - Eureka Client (1 location found)
- **Optional Issues (4 locations)**:
  - <!--ruleid=spring-boot-to-azure-config-server-01000-->Spring Cloud Config usage detected (1 location found)
  - <!--ruleid=jakarta-ee-version-01000-->Jakarta EE version not latest stable (1 location found)
  - <!--ruleid=hardcoded-urls-00001-->Avoid using hardcoded URLs (HTTP protocol) in source code (2 locations found)

## Next Steps

For comprehensive migration guidance and best practices, visit:
- [GitHub Copilot app modernization](https://aka.ms/ghcp-appmod)
