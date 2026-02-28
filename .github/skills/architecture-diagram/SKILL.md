---
name: architecture-diagram
description: Generate architecture diagram from assessment results
---

# Architecture Diagram

This skill generates a simple, clear architecture diagram based on the application assessment results.

## When to Use This Skill

Use this skill when you need to:

- Visualize application architecture after running assessment
- Understand the high-level structure of the assessed application
- Get a quick overview of application layers, dependencies, and data flow
- Share architecture understanding with stakeholders

## What This Skill Does

This skill creates a visual architecture diagram by analyzing the codebase:

1. **Analyze Project Structure**:
   - Examine project files and directory structure
   - Analyze build files:
     - Java: pom.xml, build.gradle, settings.gradle
     - .NET: *.csproj, *.sln
   - Review configuration files:
     - application.properties, appsettings.json
     - database configs, API configs
   - Scan key source files to understand patterns

2. **Analyze Architecture**:
   - Identify application layers (UI, Business Logic, Data Access)
   - Detect data storage technologies (databases, caches, file systems)
   - Map external service dependencies
   - Understand component relationships

3. **Generate Diagrams**:
   - Create 2-3 Mermaid diagrams showing different architectural perspectives
   - **Required Diagrams**:
     1. **System Context Diagram**: Show the application and its interactions with external systems (databases, APIs, services)
     2. **Container Diagram**: Show major containers within the application (web app, API, database, cache, etc.) and how they communicate
   - **Optional Diagram** (only for complex applications):
     3. **Component Diagram**: Show internal structure of the main container (controllers, services, data access layer)
   - Focus on high-level architecture, not detailed components
   - Keep it simple and readable
   - **Important**: Follow GitHub-compatible Mermaid syntax:
     - Avoid special characters (@, #, $, etc.) in arrow labels
     - Use plain text descriptions instead
     - Test rendering compatibility with GitHub
   - Save to `.github/modernize/architecture-diagram.md`

## Input Parameters

- `workspace-path` (optional): Path to the project to analyze (defaults to current directory)

## How to Use

### Prerequisites

- No special prerequisites required
- Works with Java and .NET projects only
- Best used after running `assessment` skill to understand the context

### Triggering Diagram Generation

Simply express the intent to visualize the architecture. Example prompts:

- "Generate architecture diagram from assessment"
- "Show me the application architecture"
- "Create a visual diagram of the assessed application"
- "Visualize the architecture based on assessment results"

The diagram generation process automatically:
- Analyzes project files (build files, configs, source structure)
- Identifies application structure and dependencies
- Creates 2-3 Mermaid diagrams showing different perspectives:
  - **System Context**: Application and external systems (APIs, databases, services)
  - **Container View**: Internal containers (web app, API, database, cache) and their communication
  - **Component View** (optional): Internal structure of main container (only for complex apps)
- Saves all diagrams to `.github/modernize/architecture-diagram.md`

## Diagram Output

The generated diagram will be saved to:

- `.github/modernize/architecture-diagram.md`

**File Structure**: The output file should contain multiple diagrams with clear sections:

```markdown
# Architecture Diagram

Brief introduction (1-2 sentences)

## System Context

Brief description of what this diagram shows (1 sentence)

```mermaid
[System context diagram here]
```

## Container View

Brief description of what this diagram shows (1 sentence)

```mermaid
[Container diagram here]
```

## Component View (Optional)

Brief description of what this diagram shows (1 sentence)

```mermaid
[Component diagram here - only if application is complex]
```
```

**IMPORTANT - Do NOT include**:
- Detailed technology stack tables
- Assessment summary tables
- Migration considerations or recommendations
- Lists of issues or findings
- Any detailed analysis outside the diagrams

Each diagram should be self-explanatory and show the architecture visually.

## Diagram Style

Each diagram is intentionally simple and focused:

**System Context Diagram**:
- ✅ Show: Application boundary, external systems (databases, APIs, services), data flow
- ❌ Exclude: Internal structure, detailed components

**Container Diagram**:
- ✅ Show: Major containers (web, API, database, cache), communication protocols, dependencies
- ❌ Exclude: Internal classes, detailed implementation

**Component Diagram** (if included):
- ✅ Show: Main components (controllers, services, repositories), their relationships
- ❌ Exclude: Individual classes, methods, low-level details

**All diagrams should exclude**:
- Technology details outside the diagrams (no separate sections for frameworks, libraries, etc.)
- Detailed class structures
- Migration directions (covered in separate planning)
- Low-level implementation details

## Success Criteria

Diagram generation is complete when:
- ✅ Project structure and files analyzed successfully
- ✅ Architecture analyzed and key components identified
- ✅ 2-3 Mermaid diagrams generated (System Context + Container View + optional Component View)
- ✅ Each diagram has clear, readable structure with brief descriptions
- ✅ File contains only diagrams and minimal descriptions (no detailed tables or analysis)
- ✅ All diagrams saved to `.github/modernize/architecture-diagram.md`
- ✅ All diagrams are viewable in markdown preview

## Error Handling

**Unsupported Project Type**:
- Only supports Java and .NET projects
- Provide clear message for unsupported project types

**Insufficient Architecture Information**:
- Generate a basic diagram from available data
- Indicate which information is missing or unclear

For any failure, provide clear error messages and next steps.
