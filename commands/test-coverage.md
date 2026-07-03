---
description: Analyze test coverage and generate missing tests. Identifies files below 80% threshold and generates unit, integration, and E2E tests to close the gaps.
---

# Test Coverage Command

This command invokes the **tdd-guide** agent to analyze coverage and generate missing tests.

## What This Command Does

1. **Run Coverage** - `npm test --coverage` or `pnpm test --coverage`
2. **Analyze Report** - Parse `coverage/coverage-summary.json`
3. **Identify Gaps** - Files below 80% coverage threshold
4. **Generate Tests** - Unit, integration, and E2E tests for uncovered paths
5. **Verify Tests Pass** - Run new tests and confirm they pass
6. **Show Metrics** - Before/after coverage comparison

## When to Use

Use `/test-coverage` when:
- Coverage drops below 80%
- After adding new features without tests
- Before a release
- During code review preparation

## Coverage Targets

| Type | Minimum |
|------|---------|
| Branches | 80% |
| Functions | 80% |
| Lines | 80% |
| Statements | 80% |

**100% required for:** financial calculations, authentication, security-critical code.

## Focus Areas

- Happy path scenarios
- Error handling paths
- Edge cases (null, undefined, empty)
- Boundary conditions

## Commands

```bash
npm run test:coverage
open coverage/lcov-report/index.html
```

## Related Agents

This command invokes the `tdd-guide` agent located at:
`.antigravity/agents/tdd-guide.md`
