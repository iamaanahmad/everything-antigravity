---
description: Run comprehensive verification on current codebase state. Checks build, types, lint, tests, secrets, and git status. Produces a PR-readiness report.
---

# Verify Command

Run comprehensive verification on current codebase state in Antigravity.

## What This Command Does

Executes verification in this exact order:

1. **Build Check** - Run build command; stop if it fails
2. **Type Check** - Run TypeScript/type checker; report all errors
3. **Lint Check** - Run linter; report warnings and errors
4. **Test Suite** - Run all tests; report pass/fail and coverage %
5. **Console.log Audit** - Search for `console.log` in source files
6. **Git Status** - Show uncommitted changes and modified files

## Output

```
VERIFICATION: [PASS/FAIL]

Build:    [OK/FAIL]
Types:    [OK/X errors]
Lint:     [OK/X issues]
Tests:    [X/Y passed, Z% coverage]
Secrets:  [OK/X found]
Logs:     [OK/X console.logs]

Ready for PR: [YES/NO]
```

## When to Use

Use `/verify` when:
- Before creating a pull request
- After completing a feature
- After refactoring
- Before deploying to staging

## Arguments

- `quick` - Only build + types
- `full` - All checks (default)
- `pre-commit` - Checks relevant for commits
- `pre-pr` - Full checks plus security scan

## Integration with Other Commands

- Run `/verify quick` before `/checkpoint create`
- Run `/verify pre-pr` before submitting a PR
- Run `/verify full` after `/refactor-clean`
