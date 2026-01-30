# Error Recovery Patterns

## Claim Conflict

```bash
$ bd update bd-XXX --claim
Error: bead already claimed by agent-2
```

**Recovery:** Skip this bead, select next from `bd ready`.

## Test/Verification Failure

If acceptance criteria not met after implementation:

1. **If quickly fixable**: Fix it, verify again
2. **If complex issue**:
   ```bash
   bd update <task-id> --notes "Blocked: <issue description>. Needs: <what's required>"
   bd update <task-id> --status open  # Unclaim
   bd create "Fix: <issue>" -t bug -p 1 --parent <epic>
   bd dep add <task-id> <new-bug-id>  # Original blocked by fix
   bd sync
   ```
   Continue with other ready beads.

## Deadlock (No Ready Beads)

```bash
$ bd ready
(empty)
$ bd list --status open
bd-XXX  ...  (blocked)
bd-YYY  ...  (blocked)
```

**Diagnosis:**
```bash
bd blocked --json    # See what's blocked and why
bd dep cycles        # Check for cycles
```

**Recovery:**
- If cycle exists: Report cycle details, EXIT for human intervention
- If external blocker: Document and EXIT
- If all blocked on one bead: Attempt that bead or escalate

## Git Push Failure

```bash
$ git push
! [rejected] main -> main (fetch first)
```

**Recovery:**
```bash
git pull --rebase
# If conflict in .beads/issues.jsonl:
git checkout --theirs .beads/issues.jsonl
bd import -i .beads/issues.jsonl
bd sync
git push  # Retry
```
