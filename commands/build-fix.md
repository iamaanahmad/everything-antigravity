---
description: Incrementally fix TypeScript and build errors one at a time. Invokes build-error-resolver agent to parse, explain, and fix errors safely.
---

# Build Fix Command

This command invokes the **build-error-resolver** agent to incrementally fix TypeScript and build errors.

## What This Command Does

1. **Run Build** - Execute `npm run build` or `pnpm build`
2. **Parse Errors** - Group by file, sort by severity
3. **Fix Incrementally** - One error at a time with context
4. **Verify Each Fix** - Re-run build after each change
5. **Report Summary** - Errors fixed, remaining, and any new ones introduced

## When to Use

Use `/build-fix` when:
- Build fails with TypeScript errors
- Type errors appear after refactoring
- Import/export errors occur
- After upgrading dependencies

## How It Works

For each error:
- Show error context (5 lines before/after)
- Explain the issue clearly
- Propose and apply minimal fix
- Re-run build to verify resolution
- Stop if fix introduces new errors or same error persists after 3 attempts

## Safety Rules

- Fix one error at a time
- Never refactor unrelated code
- Never change architecture
- Rollback if new errors are introduced

## Related Agents

This command invokes the `build-error-resolver` agent located at:
`.antigravity/agents/build-error-resolver.md`
