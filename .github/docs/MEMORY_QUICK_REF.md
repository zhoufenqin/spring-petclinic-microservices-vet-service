# Repository Memory Quick Reference

## 🎯 Golden Rule

**Skill Documentation > Repository Memory**

Always prioritize skill instructions (`.github/skills/*/SKILL.md`) over repository memories.

## 🚫 How to Tell AI to Ignore Memory

### Quick Commands

```
"Ignore the repository memory about [topic]."

"Don't use repository memories for this task. Follow: [your instructions]"

"The memory is outdated. Use the skill documentation instead."

"Verify any memory against current documentation before using it."
```

## ✅ How to Correct Outdated Memory

1. **Point it out**: "The memory saying [X] is outdated"
2. **Provide correct info**: "The correct approach is [Y]"
3. **Ask for update**: "Please update the repository memory"

## 📋 Priority Order (Highest to Lowest)

1. Your explicit instructions
2. Skill documentation
3. Repository docs
4. Source code
5. Repository memories ⚠️

## 🔍 Spotting Bad Memories

❌ **Red flags**:
- Conflicts with skill documentation
- Multiple contradictory memories on same topic
- Citations to generated files, not documentation
- Based on old implementations

✅ **Good memories**:
- Cite documentation as source
- Verified build/test commands
- Consistent patterns across codebase
- Non-obvious facts

## 🛠️ Common Scenarios

| Problem | Solution |
|---------|----------|
| AI uses wrong memory | Say: "Ignore that memory, use [correct info]" |
| Conflicting memories | Point out which is correct + ask to update |
| Outdated memory | Provide new info + ask AI to store it |
| Too many memories | They auto-prune; only re-store important ones |

## 📖 Full Documentation

See `.github/docs/MEMORY_MANAGEMENT.md` for complete guide.

---

*Quick reference for managing AI repository memories*
