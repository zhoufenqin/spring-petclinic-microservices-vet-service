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
   - Understand component relationships and data flow between layers

3. **Generate Diagram**:
   - Create a single Mermaid diagram showing the Application Architecture
   - Show logical layers and their relationships
   - Focus on high-level architecture layers, not detailed components
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
- Identifies application layer structure
- Creates a single Mermaid diagram showing the Application Architecture
- Saves diagram to `.github/modernize/architecture-diagram.md`

## Diagram Output

The generated diagram will be saved to:

- `.github/modernize/architecture-diagram.md`

**File Structure**: The output file should be simple and focused:

```markdown
# Architecture Diagram

A brief, generic introduction (1-2 sentences) - avoid specific technology names or implementation details.

## Application Architecture

Brief description of what this diagram shows (1 sentence)

```mermaid
[Application architecture diagram showing logical layers and their relationships]
```
```

**IMPORTANT - Introduction Guidelines**:
- Keep introduction generic and high-level
- ✅ Good: "Application architecture showing logical layers and data flow"
- ❌ Bad: "Contoso University is an ASP.NET Core 6.0 Razor Pages web application..."
- ❌ Do NOT mention specific frameworks, versions, or technology stack

**IMPORTANT - Do NOT include**:
- Detailed technology stack tables or descriptions
- Specific framework or library names outside the diagram
- Assessment summary tables
- Migration considerations or recommendations
- Lists of issues or findings
- External dependencies analysis
- Data storage technology details
- Any detailed analysis outside the diagram

The diagram should be self-explanatory and show the architecture visually.

## Diagram Style

The diagram is intentionally simple and focused on logical architecture:

**Application Architecture Diagram**:
- ✅ Show: Logical layers (presentation, business logic, data access) and their relationships
- ✅ Show: Data flow between layers (simplified arrows and descriptions)
- ✅ Keep it high-level and easy to understand
- ❌ Exclude: Specific technology names, frameworks, or libraries
- ❌ Exclude: External systems, databases, APIs (focus on internal structure only)
- ❌ Exclude: Detailed class structures, individual components, or methods
- ❌ Exclude: Migration directions or implementation details

## Success Criteria

Diagram generation is complete when:
- ✅ Project structure and files analyzed successfully
- ✅ Logical architecture layers identified
- ✅ Single Mermaid diagram generated showing Application Architecture
- ✅ Introduction is generic and does not mention specific technologies
- ✅ Diagram has clear, readable structure with brief description
- ✅ File contains only the diagram and minimal generic description
- ✅ Diagram saved to `.github/modernize/architecture-diagram.md`
- ✅ Diagram is viewable in markdown preview

## Error Handling

**Unsupported Project Type**:
- Only supports Java and .NET projects
- Provide clear message for unsupported project types

**Insufficient Architecture Information**:
- Generate a basic diagram from available data
- Indicate which information is missing or unclear

For any failure, provide clear error messages and next steps.
