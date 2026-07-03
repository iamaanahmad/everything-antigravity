---
description: Create or verify workflow checkpoints. Tracks progress with git state, test results, and coverage snapshots.
---

# Checkpoint Command

Create or verify a checkpoint in your Antigravity workflow.

## Usage

`/checkpoint [create|verify|list] [name]`

## Create Checkpoint

1. Run `/verify quick` to ensure current state is clean
2. Create a git stash or commit with checkpoint name
3. Log checkpoint to `.antigravity/checkpoints.log`:

```bash
echo "$(date +%Y-%m-%d-%H:%M) | $CHECKPOINT_NAME | $(git rev-parse --short HEAD)" >> .antigravity/checkpoints.log
```

4. Report checkpoint created

## Verify Checkpoint

1. Read checkpoint from log
2. Compare current state:
   - Files added/modified since checkpoint
   - Test pass rate now vs then
   - Coverage now vs then
3. Report:

```
CHECKPOINT COMPARISON: $NAME
============================
Files changed: X
Tests: +Y passed / -Z failed
Coverage: +X% / -Y%
Build: [PASS/FAIL]
```

## List Checkpoints

Show all checkpoints with name, timestamp, git SHA, and status.

## Workflow

```
[Start] --> /checkpoint create "feature-start"
[Implement] --> /checkpoint create "core-done"
[Test] --> /checkpoint verify "core-done"
[Refactor] --> /checkpoint create "refactor-done"
[PR] --> /checkpoint verify "feature-start"
```

## Arguments

- `create <name>` - Create named checkpoint
- `verify <name>` - Verify against named checkpoint
- `list` - Show all checkpoints
- `clear` - Remove old checkpoints (keeps last 5)
