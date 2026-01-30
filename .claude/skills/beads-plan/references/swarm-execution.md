# Swarm Execution Guidance

> This documents how the beads-harness and execution agents work.
> It is reference material, not actions for the planning agent to perform.

When multiple agents work on the same beads project, coordination is essential.

## Agent Workflow

1. **Find ready work**:
   ```bash
   bd ready --json
   ```

2. **Claim a bead** (atomic operation):
   ```bash
   bd update bd-XXXX --claim
   ```
   This atomically sets `status=in_progress` and `assignee=<agent>`. Fails if already claimed.

3. **Work on the bead**: Implement the task.

4. **Complete the bead**:
   ```bash
   bd close bd-XXXX -r "Completed: brief summary"
   ```

5. **Sync changes**:
   ```bash
   bd sync
   ```

6. **Check for newly unblocked work**:
   ```bash
   bd ready --json
   ```

## Status Transitions

```
open -> in_progress (via --claim or --status in_progress)
in_progress -> closed (via bd close)
closed -> open (via bd reopen, if needed)
```

## Avoiding Conflicts

- **Always claim before working**: Never work on unclaimed beads
- **Sync frequently**: Run `bd sync` before and after work sessions
- **Use JSON output**: `--json` flag for programmatic parsing
- **Check assignee**: `bd show bd-XXXX --json | jq '.assignee'`

## Multi-Agent Patterns

**Round-Robin**:
Each agent queries `bd ready`, claims the first unclaimed bead.

**Specialized Agents**:
Filter by label: `bd ready --label backend` vs `bd ready --label frontend`

**Priority-Based**:
Sort by priority: `bd ready --sort priority`
