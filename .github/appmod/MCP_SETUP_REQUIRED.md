# MCP Assessment Tools Setup Required

## Status
❌ **MCP Assessment tools are not currently available**

## Required MCP Tools
The following MCP server tools are required to run application assessment:

1. `appmod-precheck-assessment` - Validates that the project is ready for assessment
2. `appmod-run-assessment` - Executes the AppCAT assessment on the project

## Current Situation
- MCP is enabled in the environment (COPILOT_MCP_ENABLED=true)
- MCP server temp directory exists at: /home/runner/work/_temp/mcp-server/
- However, the required assessment MCP tools are not available in the current agent toolset

## Project Details
- **Project Type**: Java Spring Boot Microservice (Vets Service)
- **Java Version**: 17
- **Spring Boot Version**: 3.4.1
- **Build Tool**: Maven
- **Project Path**: /home/runner/work/spring-petclinic-microservices-vet-service/spring-petclinic-microservices-vet-service

## Next Steps
To complete the assessment, the following actions are needed:

### Option 1: Configure Assessment MCP Server
Ensure that an Assessment MCP server is configured and running with the required tools:
- `appmod-precheck-assessment`
- `appmod-run-assessment`

### Option 2: Manual AppCAT Assessment
As an alternative, AppCAT can be run manually:

1. Install AppCAT CLI tool
2. Run assessment on the project
3. Generate report.json
4. Place report in `.github/appmod/report.json`

### Option 3: Use Alternative Assessment Tools
Other Java application assessment tools could be used to analyze:
- Cloud migration readiness
- Framework compatibility
- Dependency issues
- Code modernization opportunities

## Expected Output
Once assessment tools are available, the assessment should generate:
- `.github/appmod/appcat/result/report.json` (or similar path)
- Consolidated report at `.github/appmod/report.json`

The consolidated report should contain:
- Application profile (detected language, frameworks, dependencies)
- Issues categorized by severity
- Specific code incidents with locations
- Migration recommendations
