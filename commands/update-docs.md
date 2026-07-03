---
description: Sync documentation from source-of-truth. Generates CONTRIB.md and RUNBOOK.md from package.json and .env.example, and flags obsolete docs.
---

# Update Docs Command

This command invokes the **doc-updater** agent to sync documentation from source-of-truth files.

## What This Command Does

1. **Read `package.json` scripts** - Generate scripts reference table
2. **Read `.env.example`** - Document all environment variables
3. **Generate `docs/CONTRIB.md`** with:
   - Development workflow
   - Available scripts
   - Environment setup
   - Testing procedures
4. **Generate `docs/RUNBOOK.md`** with:
   - Deployment procedures
   - Monitoring and alerts
   - Common issues and fixes
   - Rollback procedures
5. **Flag Obsolete Docs** - Find docs not modified in 90+ days
6. **Show Diff Summary** - What changed

## When to Use

Use `/update-docs` when:
- After adding new npm scripts
- After adding new environment variables
- Before a release
- When onboarding new developers
- When docs feel out of date

## Single Source of Truth

- `package.json` → scripts documentation
- `.env.example` → environment variable documentation
- Source code → API documentation

## Notes

- Never manually write what can be generated
- Freshness timestamps on all generated docs
- Verify all file paths and links after generation

## Related Agents

This command invokes the `doc-updater` agent located at:
`.antigravity/agents/doc-updater.md`
