---
name: assessment
description: Run application assessment for a single repository
---

# Application Assessment

This skill performs application assessment for a single repository using AppCAT tools.

## When to Use This Skill

Use this skill when you need to:

• Assess a Java or .NET application for cloud readiness and migration issues
• Generate detailed assessment reports with issue analysis and recommendations
• Understand application dependencies, frameworks, and potential migration blockers

## What This Skill Does

This skill performs a simplified assessment workflow:

1. **Check Project Type and Prerequisites**:
   - **For Java projects**: Verify that MCP tools are available ('appmod-precheck-assessment' and 'appmod-run-assessment')
     - If these MCP tools are not configured, return immediately with setup instructions
   - **For .NET projects**: Check if .NET SDK is available
     - No MCP tools required for .NET assessment

2. **Run Assessment**:
   - **For Java projects**: Trigger AppCAT analysis via Assessment MCP server
     - Uses 'appmod-precheck-assessment' and 'appmod-run-assessment' MCP tools
     - Auto-detects project configuration
   - **For .NET projects**: Install and run AppCAT directly
     - Install: `dotnet tool update dotnet-appcat`
     - Run: `dotnet-appcat analyze --source {workspace-path} --target Any --serializer APPMODJSON --code --privacyMode Protected --non-interactive --report {workspace-path}\.github\appmod\report.json`
   - Analyzes code for cloud migration issues
   - Generates structured assessment data
   - Report is stored under `.github/appmod/` directory

3. **Consolidate Report** (Java projects only):
   - Search for `report.json` under `.github/appmod/` subdirectories
   - Common locations: `.github/appmod/appcat/result/report.json`
   - Copy the latest report to `.github/appmod/report.json`
   - For .NET projects, the report is already generated at `.github/appmod/report.json`
   - This consolidated report should be included in the pull request

## Input Parameters

- `workspace-path` (required): Path to the project to assess

## How to Use

### Prerequisites

**For Java projects**:
- MCP tools must be available: 'appmod-precheck-assessment' and 'appmod-run-assessment'
- If tools are not configured, the skill will return instructions for setup

**For .NET projects**:
- .NET SDK must be installed
- No MCP tools required - appcat will be installed and run directly via .NET CLI
- The assessment will automatically install `dotnet-appcat` tool if not already present

### Triggering Assessment

Simply express the intent to assess the application. Example prompts:

• "Assess the application"
• "Run AppCAT assessment for this project"
• "Analyze this application for Azure migration"

The assessment process automatically:
• Detects project language and framework
• **For Java**: Uses MCP tools to install and run AppCAT
• **For .NET**: Installs dotnet-appcat tool and runs analysis directly
• Executes comprehensive analysis
• Generates report at `.github/appmod/report.json` (or subdirectories for Java)

### Report Consolidation

**For Java projects**:
1. Search for `report.json` files under `.github/appmod/` subdirectories
2. If multiple reports exist, identify the most recently modified one
3. Copy the latest report to `.github/appmod/report.json`
4. Include this consolidated report in the pull request

**For .NET projects**:
1. Report is directly generated at `.github/appmod/report.json`
2. Include this report in the pull request

## Assessment Output

**Output file**: `.github/appmod/report.json`

This file contains:
- Detected issues and their severity
- Application dependencies and frameworks
- Recommendations for cloud migration
- Compatibility analysis

**Include this file in the pull request.**

## Success Criteria

Assessment is complete when:
• ✅ **For Java**: MCP tools are available and executed successfully
• ✅ **For .NET**: .NET SDK is available and dotnet-appcat tool executed successfully
• ✅ AppCAT analysis completes without errors
• ✅ Report generated at `.github/appmod/report.json`
• ✅ Report included in pull request

## Error Handling

**Prerequisites Not Met**:
• **For Java**: Verify MCP tools are available ('appmod-precheck-assessment' and 'appmod-run-assessment')
  - Return immediately with setup instructions if tools are not available
  - Do not attempt to run assessment without MCP
• **For .NET**: Verify .NET SDK is installed
  - Check with `dotnet --version` command
  - Provide installation instructions if .NET SDK is missing

**Assessment Failures**:
• Unsupported project type (only Java and .NET supported)
• **For Java**: MCP server communication errors
• **For .NET**: 
  - dotnet-appcat tool installation failure
  - dotnet-appcat command execution errors
• Invalid project structure or build configuration

**Report Generation Issues**:
• **For Java**: No report.json found under `.github/appmod/` subdirectories after MCP execution
• **For .NET**: Report not generated at `.github/appmod/report.json`
• Report file is corrupted or invalid JSON

For any failure, provide clear error messages and troubleshooting steps.

---

# Architecture Diagram Generation

After completing the assessment, you can optionally generate an architecture diagram to visualize the application structure.

## When to Use

• After successful assessment completion
• When you need visual documentation of the application architecture
• To share architecture overview with stakeholders
• User can explicitly request: "Generate architecture diagram"

## What It Does

Generates a **simple, focused** architecture diagram by:

1. **Analyzing Project Structure**:
   - Scan `*.csproj` or `pom.xml`/`build.gradle` files for project types and dependencies
   - Identify project references and build structure

2. **Analyzing Configuration**:
   - Read `appsettings.json` or `application.properties` for service configurations
   - Detect external dependencies (databases, caching, messaging, etc.)

3. **Analyzing Code**:
   - Identify entry points (Program.cs, Startup.cs, Main classes)
   - Find controllers/endpoints for API structure
   - Detect DbContext classes and key service classes

4. **Generating Diagram**:
   - Create `.github/appmod/application-diagram.md`
   - Include 1-2 simple architecture diagrams (max 10 nodes each)
   - Use tables for code structure and technology stack details

## Diagram Output

**Output file**: `.github/appmod/application-diagram.md`

**Content structure** (exactly 4 sections):
1. **Overview**: Simple property table (type, framework, database, etc.)
2. **Application Architecture**: 1-2 mermaid diagrams showing high-level architecture
3. **Code Structure**: Tables listing key components and folder organization  
4. **Technology Stack**: Table of frameworks, libraries, and tools

**Critical rules**:
- Maximum 1-2 mermaid diagrams (architecture focus only)
- Each diagram under 10 nodes
- Use tables for details, not complex diagrams
- NO migration considerations, security recommendations, or flow descriptions
- Use `<!-- DIAGRAM:architecture -->` marker before mermaid blocks

## Example Output

```markdown
# ContosoUniversity

| Property | Value |
|----------|-------|
| Type | ASP.NET Core Web App |
| Framework | .NET 6.0 |
| Database | SQL Server |
| ORM | Entity Framework Core |

## Application Architecture

<!-- DIAGRAM:architecture -->
\`\`\`mermaid
graph TB
    subgraph "Web Application"
        Pages[Razor Pages]
        Data[Data Access]
    end
    Pages --> Data --> DB[(SQL Server)]
\`\`\`

## Code Structure

| Component | Location | Responsibility |
|-----------|----------|----------------|
| SchoolContext | Data/SchoolContext.cs | Database context |
| Student | Models/Student.cs | Student entity |

| Folder | Purpose |
|--------|---------|
| Pages/ | Razor Pages UI |
| Models/ | Domain entities |
| Data/ | EF Core context |

## Technology Stack

| Category | Technologies |
|----------|--------------|
| Runtime | .NET 6.0 |
| Web Framework | ASP.NET Core Razor Pages |
| ORM | Entity Framework Core 6.0 |
| Database | SQL Server |
```

## Success Criteria

Diagram generation is complete when:
• ✅ `.github/appmod/application-diagram.md` is created
• ✅ Contains 1-2 architecture diagrams (max 10 nodes each)
• ✅ Has exactly 4 sections: Overview, Architecture, Code Structure, Tech Stack
• ✅ Uses actual project/class names from the codebase
• ✅ Does NOT include migration, security, or flow sections

## Triggering Diagram Generation

- Recommended after successful assessment
- User says: "Generate architecture diagram"
- User can skip by saying: "Skip the diagram"
