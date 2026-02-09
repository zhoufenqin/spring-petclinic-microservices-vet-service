# Repository Memory Management Guide

## What is Repository Memory?

Repository memory is a system that allows AI agents to store and retrieve facts about your codebase across different sessions. These memories help maintain consistency and avoid repeating work, but they can become outdated or incorrect over time.

## How Repository Memory Works

### Memory Display

At the start of each session, you'll see a `<repository_memories>` section that contains:

```
**memory subject**
- Fact: [description of what was learned]
- Citations: [where this information came from]
```

### Memory Lifecycle

1. **Storage**: AI agents store memories using the `store_memory` tool during sessions
2. **Retrieval**: Memories are shown at the start of subsequent sessions
3. **Retention**: Only recent memories are retained; older ones are automatically pruned
4. **Refresh**: Re-storing a memory updates its timestamp and keeps it longer

## The Problem: Outdated or Conflicting Memories

### Example from This Repository

In our recent session, there were **conflicting memories**:

❌ **Outdated Memory** (stored multiple times from old sessions):
```
Fact: Assessment results should be saved to both .github/appmod/ 
      and .github/modernize/ directories
```

✅ **Correct Memory** (current reality):
```
Fact: Assessment results should ONLY be saved to .github/modernize/ directory
```

### Why This Happens

1. **Context Changes**: Requirements or implementations change over time
2. **Multiple Sessions**: Different sessions may store conflicting information
3. **Memory Persistence**: Outdated memories can persist if frequently re-stored
4. **No Automatic Correction**: The system doesn't automatically detect conflicts

## How to Manage Repository Memories

### 1. Review Memories at Session Start

Always review the `<repository_memories>` section to:
- ✅ Identify outdated or incorrect information
- ✅ Spot conflicts between memories
- ✅ Verify citations are still accurate

### 2. Explicit Instructions to Ignore Memory

You can explicitly tell the AI agent:

```
"Ignore the repository memory about [specific topic]. 
The correct approach is [your guidance]."
```

**Example from our session:**
```
"Ignore the memory saying to save to both directories. 
Files should ONLY go to .github/modernize/ as per the skill documentation."
```

### 3. Ask for Memory Correction

Instruct the agent to correct outdated memories:

```
"Please update the repository memory to reflect that [correct fact]."
```

The agent will use `store_memory` to store the corrected information.

### 4. Priority: Skill Instructions > Repository Memory

**Important Rule**: 
- **Skill documentation** and **explicit instructions** are the source of truth
- **Repository memories** are helpful hints but can be outdated
- Always prioritize current documentation over old memories

### 5. Request Memory Cleanup

You can ask:
```
"Please review and update all outdated repository memories 
related to [topic]."
```

## Best Practices for AI Agents

### When to Store Memories

✅ **DO store memories for**:
- Verified build/test commands
- Important architectural patterns
- Consistent coding conventions
- Non-obvious project structure facts

❌ **DON'T store memories for**:
- Temporary implementation details
- Information already in documentation
- Context-specific decisions
- Frequently changing configurations

### How to Store Good Memories

```javascript
store_memory({
  category: "general",
  subject: "build commands",
  fact: "Build with 'mvn clean install', test with 'mvn test'",
  citations: "pom.xml, verified in session on 2026-02-09",
  reason: "Critical for future development tasks. Build commands 
          are stable and frequently needed."
})
```

**Key Elements**:
1. **Specific subject**: Clear categorization
2. **Concise fact**: Short, actionable statement
3. **Accurate citations**: Where information came from
4. **Clear reason**: Why this matters for future tasks

### Correcting Outdated Memories

When you identify an outdated memory, store the corrected version:

```javascript
store_memory({
  category: "general",
  subject: "assessment workflow",
  fact: "Assessment results should ONLY be saved to .github/modernize/ directory",
  citations: ".github/skills/assessment/SKILL.md (skill documentation)",
  reason: "This CORRECTS previous outdated memories that mentioned 
          .github/appmod/. The skill documentation is the source 
          of truth."
})
```

## How to Tell AI Not to Use Memory

### Method 1: Explicit Override
```
"Don't use the repository memory for this task. 
Instead, follow these instructions: [your guidance]"
```

### Method 2: Point to Source of Truth
```
"The repository memory is outdated. Refer to the skill 
documentation in .github/skills/[skill-name]/SKILL.md instead."
```

### Method 3: Request Verification
```
"Before using any repository memory about [topic], 
please verify it against the current codebase/documentation."
```

### Method 4: Clear Guidance
```
"Repository memories may be from different contexts. 
For this task, use only: [specific instructions]"
```

## Troubleshooting

### Problem: AI Uses Outdated Memory

**Solution**:
1. Point out the specific memory that's wrong
2. Provide the correct information
3. Ask AI to update the memory
4. Verify the correction in the session summary

### Problem: Conflicting Memories

**Solution**:
1. Identify which memory is correct (check citations)
2. Tell AI: "Memory X is outdated, Memory Y is correct"
3. Ask AI to store the correct version again
4. Outdated version will eventually be pruned

### Problem: Too Many Irrelevant Memories

**Solution**:
1. Memories are automatically pruned over time
2. Don't re-store irrelevant memories
3. Focus on storing only critical, long-term facts

## Examples from This Repository

### ❌ Bad Memory (What Happened)
```
Subject: assessment workflow
Fact: Assessment results should be saved to both .github/appmod/ 
      and .github/modernize/ directories
Citations: .github/modernize/report.json, .github/appmod/report.json
Reason: Assessment requires both locations
```

**Why it's bad**: Based on implementation, not documentation. Created confusion.

### ✅ Good Memory (Correction)
```
Subject: assessment workflow  
Fact: Assessment results should ONLY be saved to .github/modernize/ 
      directory with report.json and assessment-diagram.md files
Citations: .github/skills/assessment/SKILL.md, 
           .github/skills/assessment-diagram/SKILL.md
Reason: The skill documentation specifies .github/modernize/ as the 
        sole output directory. Future assessment tasks must follow 
        the skill instructions, not outdated repository memories.
```

**Why it's good**: Based on documentation (source of truth), explicitly corrects the mistake.

## Quick Reference

| Scenario | Action |
|----------|--------|
| Memory is outdated | Tell AI to ignore it and provide correct info |
| Memory conflicts exist | Identify which is correct, ask AI to update |
| Memory from different context | Ask AI to verify before using |
| Need to prevent memory use | Give explicit instructions to ignore it |
| Want to correct memory | Ask AI to store updated version |
| Skill docs vs memory conflict | Always prioritize skill documentation |

## Documentation Hierarchy (Priority Order)

1. **Explicit user instructions** (highest priority)
2. **Skill documentation** (`.github/skills/*/SKILL.md`)
3. **Repository documentation** (README, docs/)
4. **Source code** (actual implementation)
5. **Repository memories** (lowest priority - use with caution)

## Summary

- ✅ Repository memories are **helpful hints**, not absolute truth
- ✅ **Always verify** memories against current documentation
- ✅ **Prioritize** skill instructions over memories
- ✅ **Correct** outdated memories by storing updated versions
- ✅ **Be explicit** when you want AI to ignore a memory
- ✅ **Store selectively** - only critical, long-term facts

---

*This guide was created to help users understand and manage repository memories effectively. Last updated: 2026-02-09*
