# BD Command Reference

## Initialization

```bash
bd init                    # Interactive initialization
bd init --quiet            # Non-interactive (for agents)
bd init --prefix myproj    # Custom ID prefix
```

## Creating Issues

```bash
bd create "Title" [flags]

Flags:
  -t, --type string        # epic|feature|task|bug|chore (default: task)
  -p, --priority string    # 0-4 or P0-P4 (default: 2)
  -d, --description string # Issue description
  --parent string          # Parent issue ID for hierarchical child
  --acceptance string      # Acceptance criteria
  -l, --labels strings     # Labels (comma-separated)
  -a, --assignee string    # Assignee
  --deps strings           # Dependencies on create (type:id format)
  --json                   # JSON output
```

## Dependency Management

```bash
bd dep add <issue> <depends-on> [-t type]  # Add dependency
bd dep remove <issue> <depends-on>          # Remove dependency
bd dep tree <issue>                         # Show dependency tree
bd dep tree <issue> --direction=up          # Show what this blocks
bd dep cycles                               # Detect cycles
bd dep list <issue>                         # List dependencies
```

Dependency types: `blocks` (default), `related`, `parent-child`, `discovered-from`, `tracks`

## Querying Issues

```bash
bd ready                   # Show ready work (no blockers)
bd ready --json            # JSON output for parsing
bd ready --limit 5         # Limit results
bd ready --assignee name   # Filter by assignee
bd ready --label backend   # Filter by label

bd list                    # List all open issues
bd list --all              # Include closed
bd list --parent bd-XXX    # Children of specific parent
bd list --status open      # Filter by status

bd show <id>               # Show issue details
bd show <id> --json        # JSON output

bd blocked                 # Show blocked issues
bd graph <id>              # ASCII dependency visualization
bd graph --all             # All open issues
```

## Updating Issues

```bash
bd update <id> [flags]

Flags:
  --status string          # open|in_progress|closed
  --claim                  # Atomically claim (set in_progress + assignee)
  --title string           # New title
  -d, --description string # New description
  -p, --priority string    # New priority
  --acceptance string      # Acceptance criteria
  --add-label strings      # Add labels
  --remove-label strings   # Remove labels
```

## Closing Issues

```bash
bd close <id>              # Close an issue
bd close <id> -r "reason"  # Close with reason
bd close <id> --suggest-next  # Show newly unblocked after close
```

## Epic Management

```bash
bd epic status <id>        # Show epic completion status
bd epic close-eligible     # Close epics where all children complete
```

## Swarm Operations

```bash
bd swarm validate <epic>   # Validate epic structure for swarming
bd swarm create <epic>     # Create swarm molecule from epic
bd swarm status            # Show swarm status
```

## Sync and Data

```bash
bd sync                    # Sync with git (export + commit + pull + push)
bd export -o file.jsonl    # Export to JSONL
bd import -i file.jsonl    # Import from JSONL
```
