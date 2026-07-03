---
description: Analyze codebase structure and update architecture documentation. Invokes doc-updater agent to generate token-lean codemaps with freshness timestamps.
---

# Update Codemaps Command

This command invokes the **doc-updater** agent to analyze the codebase and update architecture documentation.

## What This Command Does

1. **Scan Source Files** - Analyze imports, exports, and dependencies
2. **Generate Codemaps** - Create structured architecture docs:
   - `docs/CODEMAPS/architecture.md` - Overall architecture
   - `docs/CODEMAPS/backend.md` - Backend structure
   - `docs/CODEMAPS/frontend.md` - Frontend structure
   - `docs/CODEMAPS/database.md` - Data models and schemas
3. **Calculate Diff** - Compare against previous version
4. **Request Approval** - If changes > 30%, ask before updating
5. **Add Timestamps** - Freshness date on each codemap
6. **Save Diff Report** - `.reports/codemap-diff.txt`

## When to Use

Use `/update-codemaps` when:
- After significant architectural changes
- After adding new modules or services
- Before onboarding new team members
- As part of release preparation

## Codemap Format

```markdown
# [Area] Codemap

**Last Updated:** YYYY-MM-DD
**Entry Points:** list of main files

## Architecture
[ASCII diagram of component relationships]

## Key Modules
| Module | Purpose | Exports | Dependencies |

## Data Flow
[Description of how data flows]
```

## Notes

- Keep codemaps under 500 lines for token efficiency
- Focus on high-level structure, not implementation details
- Generate from actual code, not assumptions

## Related Agents

This command invokes the `doc-updater` agent located at:
`.antigravity/agents/doc-updater.md`
