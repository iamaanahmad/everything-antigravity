---
description: Safely identify and remove dead code with test verification. Invokes refactor-cleaner agent to run analysis tools, categorize findings, and clean up with safety checks.
---

# Refactor Clean Command

This command invokes the **refactor-cleaner** agent to safely identify and remove dead code.

## What This Command Does

1. **Run Analysis Tools** - `knip`, `depcheck`, `ts-prune`
2. **Generate Report** - Dead code analysis in `.reports/dead-code-analysis.md`
3. **Categorize Findings** by risk:
   - SAFE: Test files, unused utilities
   - CAUTION: API routes, components
   - DANGER: Config files, main entry points
4. **Propose Safe Deletions Only**
5. **Verify with Tests** - Run full test suite before and after each deletion
6. **Show Summary** - Files deleted, dependencies removed, lines reduced

## When to Use

Use `/refactor-clean` when:
- Codebase has accumulated unused code
- Dependencies need pruning
- After a major refactor
- Before a release to reduce bundle size

## Safety Rules

- Never delete code without running tests first
- Start with SAFE items only
- Remove one category at a time
- Rollback if tests fail
- Document all deletions in `docs/DELETION_LOG.md`

## Detection Tools Used

```bash
npx knip          # Unused exports, files, dependencies
npx depcheck      # Unused npm dependencies
npx ts-prune      # Unused TypeScript exports
```

## Related Agents

This command invokes the `refactor-cleaner` agent located at:
`.antigravity/agents/refactor-cleaner.md`
