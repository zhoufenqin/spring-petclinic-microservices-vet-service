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

3. **Generate Diagram**:
   - Create a Mermaid C4 or flowchart diagram
   - Focus on high-level architecture, not detailed components
   - Show key layers, dependencies, and data flow
   - Keep it simple and readable
   - **Important**: Follow GitHub-compatible Mermaid syntax:
     - Avoid special characters (@, #, $, etc.) in arrow labels
     - Use plain text descriptions instead
     - Test rendering compatibility with GitHub
   - **Important**: Only include the Mermaid diagram - do not add technology details, framework information, or other metadata outside the diagram
   - Save to `.github/modernize/appcat/architecture-diagram.md`

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
- Creates a Mermaid diagram showing:
  - Application layers (e.g., Presentation, Business, Data)
  - Data storage components
  - External service integrations
  - Component relationships and data flow
- Saves diagram to `.github/modernize/appcat/architecture-diagram.md`

## Diagram Output

The generated diagram will be saved to:

- `.github/modernize/appcat/architecture-diagram.md`

**IMPORTANT**: The output file must contain **ONLY** the Mermaid diagram code block:
```mermaid
[diagram content here]
```

Do NOT include:
- Text descriptions outside the diagram
- Technology lists or bullet points
- Separate sections for frameworks, dependencies, etc.
- Any content outside the Mermaid code block

All information should be represented within the diagram itself.

## Diagram Style

The diagram is intentionally simple and focused:

✅ **Include** (within the diagram):
- High-level application layers
- Data storage types
- Key external integrations
- Component relationships

❌ **Exclude**:
- Technology details outside the diagram (no separate sections for frameworks, libraries, etc.)
- Detailed class structures
- Individual components
- Migration directions (covered in separate planning)
- Low-level implementation details

## Success Criteria

Diagram generation is complete when:
- ✅ Project structure and files analyzed successfully
- ✅ Architecture analyzed and key components identified
- ✅ Mermaid diagram generated with clear, readable structure
- ✅ File contains only the Mermaid diagram (no additional metadata sections)
- ✅ Diagram saved to `.github/modernize/appcat/architecture-diagram.md`
- ✅ Diagram is viewable in markdown preview

## Error Handling

**Unsupported Project Type**:
- Only supports Java and .NET projects
- Provide clear message for unsupported project types

**Insufficient Architecture Information**:
- Generate a basic diagram from available data
- Indicate which information is missing or unclear

For any failure, provide clear error messages and next steps.
