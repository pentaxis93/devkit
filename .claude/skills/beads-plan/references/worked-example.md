# Worked Example: CLI Tool with 3 Subcommands

## Specification

> Build a CLI tool called `mytool` with three subcommands: `init`, `run`, and `status`. 
> It should be written in Go, have a config file, and include tests.

## Phase 1: Understanding

**Scope**: CLI tool with 3 commands, Go, config support, tests.
**Success**: `mytool init`, `mytool run`, `mytool status` all work, tests pass.
**Constraints**: Go 1.21+, Cobra for CLI framework.
**Boundaries**: 
- Core library (shared code)
- CLI layer (Cobra commands)
- Config system
- Each subcommand

## Phase 2: Decomposition

```
Epic: mytool CLI
  Task: Project scaffolding (go mod, directory structure)
  Task: Implement config loading
  Task: Implement `init` subcommand
  Task: Implement `run` subcommand  
  Task: Implement `status` subcommand
  Task: Add unit tests
  Task: Add integration tests
  Task: Write README documentation
```

## Phase 3: Dependency Mapping

```
scaffolding <- config <- init
scaffolding <- config <- run
scaffolding <- config <- status
init, run, status <- unit tests
unit tests <- integration tests
integration tests <- documentation (docs describe tested behavior)
```

## Phase 4: Instantiation

```bash
# Create epic
bd create "mytool CLI" -t epic -p 1 \
  -d "CLI tool with init, run, and status subcommands" \
  --acceptance "All commands work, tests pass, README complete"
# Returns: bd-m7k2

# Create tasks with hierarchical IDs
bd create "Project scaffolding" -t task -p 1 --parent bd-m7k2 \
  -d "Set up go.mod, directory structure, Cobra root command" \
  --acceptance "go build succeeds, mytool --help works"
# Returns: bd-m7k2.1

bd create "Implement config loading" -t task -p 1 --parent bd-m7k2 \
  -d "Config file parsing with Viper, support YAML and env vars" \
  --acceptance "Config loads from file and environment"
# Returns: bd-m7k2.2

bd create "Implement init subcommand" -t task -p 2 --parent bd-m7k2 \
  -d "mytool init creates default config file" \
  --acceptance "Running mytool init creates .mytool.yaml"
# Returns: bd-m7k2.3

bd create "Implement run subcommand" -t task -p 2 --parent bd-m7k2 \
  -d "mytool run executes the main workflow" \
  --acceptance "Running mytool run produces expected output"
# Returns: bd-m7k2.4

bd create "Implement status subcommand" -t task -p 2 --parent bd-m7k2 \
  -d "mytool status shows current state" \
  --acceptance "Running mytool status displays state information"
# Returns: bd-m7k2.5

bd create "Add unit tests" -t task -p 2 --parent bd-m7k2 \
  -d "Unit tests for config, init, run, status packages" \
  --acceptance "go test ./... passes with >70% coverage"
# Returns: bd-m7k2.6

bd create "Add integration tests" -t task -p 3 --parent bd-m7k2 \
  -d "End-to-end tests running actual CLI commands" \
  --acceptance "Integration test suite passes"
# Returns: bd-m7k2.7

bd create "Write README documentation" -t task -p 3 --parent bd-m7k2 \
  -d "README with installation, usage, and examples" \
  --acceptance "README covers all commands with examples"
# Returns: bd-m7k2.8

# Add dependencies
bd dep add bd-m7k2.2 bd-m7k2.1  # config depends on scaffolding
bd dep add bd-m7k2.3 bd-m7k2.2  # init depends on config
bd dep add bd-m7k2.4 bd-m7k2.2  # run depends on config
bd dep add bd-m7k2.5 bd-m7k2.2  # status depends on config
bd dep add bd-m7k2.6 bd-m7k2.3  # unit tests depend on init
bd dep add bd-m7k2.6 bd-m7k2.4  # unit tests depend on run
bd dep add bd-m7k2.6 bd-m7k2.5  # unit tests depend on status
bd dep add bd-m7k2.7 bd-m7k2.6  # integration tests depend on unit tests
bd dep add bd-m7k2.8 bd-m7k2.7  # docs depend on integration tests
```

## Phase 5: Validation

```bash
$ bd ready
bd-m7k2.1  P1  task  Project scaffolding

$ bd dep cycles
# (empty - no cycles)

$ bd graph bd-m7k2
┌─────────────────────────────────────────────────────────────┐
│ Layer 0                                                     │
│ ○ bd-m7k2.1 Project scaffolding                             │
├─────────────────────────────────────────────────────────────┤
│ Layer 1                                                     │
│ ○ bd-m7k2.2 Implement config loading                        │
├─────────────────────────────────────────────────────────────┤
│ Layer 2                                                     │
│ ○ bd-m7k2.3 Implement init subcommand                       │
│ ○ bd-m7k2.4 Implement run subcommand                        │
│ ○ bd-m7k2.5 Implement status subcommand                     │
├─────────────────────────────────────────────────────────────┤
│ Layer 3                                                     │
│ ○ bd-m7k2.6 Add unit tests                                  │
├─────────────────────────────────────────────────────────────┤
│ Layer 4                                                     │
│ ○ bd-m7k2.7 Add integration tests                           │
├─────────────────────────────────────────────────────────────┤
│ Layer 5                                                     │
│ ○ bd-m7k2.8 Write README documentation                      │
└─────────────────────────────────────────────────────────────┘

$ bd epic status bd-m7k2
Epic: mytool CLI (bd-m7k2)
Progress: 0/8 complete (0%)
Ready: 1, Blocked: 7, In Progress: 0
```

**Result**: Valid dependency graph with one ready starting point, no cycles, clear execution layers.
