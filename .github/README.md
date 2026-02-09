# .github Directory

This directory contains GitHub-specific configuration and documentation for the Spring PetClinic Vets Service.

## Contents

### 📁 `/docs`
Documentation for working with this repository:
- **[MEMORY_MANAGEMENT.md](docs/MEMORY_MANAGEMENT.md)** - Complete guide for managing AI repository memories
- **[MEMORY_QUICK_REF.md](docs/MEMORY_QUICK_REF.md)** - Quick reference card for memory management

### 📁 `/skills`
AI skills for automated tasks:
- `assessment` - Application assessment using AppCAT
- `assessment-diagram` - Architecture diagram generation
- `create-modernization-plan` - Modernization planning
- `execute-modernization-plan` - Plan execution
- `generate-appcat-aggregate-report` - Aggregate report generation

### 📁 `/modernize`
Assessment and modernization outputs:
- `report.json` - AppCAT assessment report
- `assessment-diagram.md` - Architecture diagram

## Documentation

### Memory Management

If you're working with AI agents and notice:
- Conflicting or outdated repository memories
- AI using incorrect information from past sessions
- Questions about how memory works

👉 **See [docs/MEMORY_MANAGEMENT.md](docs/MEMORY_MANAGEMENT.md)** for:
- How repository memory works
- How to tell AI to ignore outdated memories
- How to correct and update memories
- Best practices and troubleshooting

👉 **See [docs/MEMORY_QUICK_REF.md](docs/MEMORY_QUICK_REF.md)** for:
- Quick commands and reference
- Common scenarios and solutions

## Priority of Information

When AI agents work on this repository, information is prioritized in this order:

1. **Explicit user instructions** (highest priority)
2. **Skill documentation** (`.github/skills/*/SKILL.md`)
3. **Repository documentation** (docs/)
4. **Source code** (actual implementation)
5. **Repository memories** (lowest priority)

Always verify memories against current documentation before relying on them.
