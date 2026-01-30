---
name: agents-md
description: >-
  Create and optimize AGENTS.md files for AI coding agents. Use when
  setting up agent instructions for a new project, improving existing
  AGENTS.md, or diagnosing why agents aren't following project conventions.
  Relevant for project setup, onboarding, agent configuration, CLAUDE.md.
---

# AGENTS.md Authoring Skill

Guide the creation of effective AGENTS.md files -- configuration files
that provide persistent instructions to AI coding agents.

## What AGENTS.md Solves

1. **Memory persistence** -- Loaded at session start; survives context loss
2. **Team standardization** -- Shared via git; consistent AI behavior
3. **Reduced exploration** -- Agents immediately know commands, conventions
4. **Mistake prevention** -- Explicit rules prevent errors before they happen

## Key Principle

**AGENTS.md is compact context. Skills are the modularization.**

Keep AGENTS.md under 100 lines. Detailed procedures live in
`.claude/skills/`. AGENTS.md is the index; skills are the chapters.

---

## Phase 1: Assessment

Before writing, answer:

1. **Tech stack**: Languages, frameworks, build tools?
2. **Common commands**: What does the team run daily?
3. **Conventions**: Style rules not enforced by linters?
4. **Gotchas**: What trips up newcomers (human or AI)?
5. **Architecture**: Directory structure and data flow?

### Scope Determination

| Project Type | Structure |
|---|---|
| Single-language, simple | One root AGENTS.md (~50-80 lines) |
| Monorepo or multi-component | Root + component-level AGENTS.md |

---

## Phase 2: Structure

### File Hierarchy

```
~/.config/opencode/AGENTS.md    # User-level (all projects)
./AGENTS.md                      # Project-level (team-shared via git)
./AGENTS.local.md                # Personal overrides (gitignored)
./component/AGENTS.md            # Component-specific rules
```

### Recommended Sections

| Section | Include When | Content |
|---|---|---|
| Quick Reference | Always | Table of common commands |
| Architecture | Multi-component | Directory structure, data flow |
| Code Style | Beyond linter | Naming, patterns, preferences |
| Testing | Non-trivial setup | How to run, what to verify |
| Gotchas | Project has quirks | Things that cause mistakes |
| Skills | Has pipeline skills | Which skills to invoke and when |
| See Also | Has related docs | Links to other docs |

### Length Guidelines

| Guideline | Rationale |
|---|---|
| Root: 50-100 lines | Coverage without dilution |
| Component: 30-50 lines | Component-specific only |
| Total: Under 500 lines | Empirical effectiveness ceiling |

---

## Phase 3: Writing Effective Instructions

### What Works

**Imperative commands:**
```markdown
Run `npm test` before committing.
```

**Exact commands (copy-paste ready):**
```markdown
Run migrations: `cd server/database && uv run --directory .. alembic upgrade head`
```

**Always/Never rules:**
```markdown
- Always use `httpx` for HTTP calls, never `requests`
- Never commit directly to `main` branch
```

**Tables for quick reference:**
```markdown
| Task | Command |
|------|---------|
| Build | `npm run build` |
| Test | `npm test` |
```

### Anti-Patterns

| Anti-Pattern | Fix |
|---|---|
| Vague ("write clean code") | Specific rule ("2-space indent") |
| Contradictions | Remove conflict |
| Restating defaults | Agent already does this |
| Duplicating linters | Linter already enforces |
| Too verbose | Bullet points, tables |

---

## Phase 4: Gotchas Section

The most valuable section. Prevents AI mistakes proactively.

### What to Include

- **Naming collisions**: "API keys use `myapp_sk_` prefix"
- **Framework choices**: "Uses Riverpod, not Provider"
- **Library preferences**: "Use `httpx`, not `requests`"
- **Directory quirks**: "`database/` is inside `server/`"
- **Generated files**: "Files in `dist/` are generated -- never edit"

### Iterative Collection

When an AI makes a preventable mistake:
1. Note the mistake
2. Add a gotcha rule
3. Periodically prune rules that no longer trigger

---

## Phase 5: Skills Integration

AGENTS.md should reference pipeline skills, not replicate them:

```markdown
## Planning Pipeline

Start with `/vision-workshop` for new projects. The pipeline flows:
vision → features → journeys → beads-plan → beads-execute

## Available Skills

| Skill | When to Use |
|---|---|
| `/vision-workshop` | New project or feature vision |
| `/feature-discovery` | Break vision into features |
| `/journey-mapping` | Map user journeys with scenarios |
| `/beads-plan` | Decompose epic into tasks |
| `/beads-execute` | Execute a bead |
| `/commit` | Create conventional commit |
```

---

## Phase 6: Maintenance

| Trigger | Action |
|---|---|
| AI makes preventable mistake | Add gotcha rule |
| Rule never triggers | Consider removing |
| Major refactor | Update architecture, commands |
| Quarterly | Prune outdated rules |

Commit AGENTS.md changes with the code they relate to.

---

## Templates

### Minimal (~30 lines)

```markdown
# Project Name

## Quick Reference
| Task | Command |
|------|---------|
| Build | `<command>` |
| Test | `<command>` |
| Lint | `<command>` |

## Code Style
- <key rule>

## Gotchas
- <common mistake to avoid>
```

### Standard (~80 lines)

```markdown
# Project Name

Brief description.

## Quick Reference

| Task | Command |
|------|---------|
| Install | `<command>` |
| Dev | `<command>` |
| Test | `<command>` |
| Lint | `<command>` |

## Architecture

dir1/     Description
dir2/     Description

## Code Style

- <convention>

## Testing

- Run `<command>` before committing

## Gotchas

- <gotcha>

## Skills

| Skill | When to Use |
|---|---|
| `/skill-name` | <trigger> |

## See Also

- Related doc: path/to/doc
```

---

## Cross-Tool Compatibility

| Tool | Primary File | Also Reads |
|---|---|---|
| Claude Code | CLAUDE.md | -- |
| OpenCode | AGENTS.md | CLAUDE.md |
| Cursor | .cursor/rules/*.md | AGENTS.md |
| GitHub Copilot | .github/copilot-instructions.md | -- |

For maximum compatibility, use AGENTS.md (widest cross-tool support).

---

## Validation Checklist

- [ ] Commands tested and work
- [ ] No vague instructions
- [ ] No contradictory rules
- [ ] Gotchas cover known pitfalls
- [ ] Under 100 lines for root file
- [ ] AGENTS.local.md in .gitignore
- [ ] Skills referenced by slash command name
- [ ] At least one team member reviewed

## Related Skills

| Skill | Relationship |
|---|---|
| `writing-clearly-and-concisely` | Prose quality for instructions |
| `voice-coauthoring` | Optimize responses for voice workflow |
| `commit` | Commit AGENTS.md changes conventionally |

## Attribution

Imported from [pentaxis93/beadsmith](https://github.com/pentaxis93/beadsmith).
Adapted: compressed from 350+ lines; added skills integration phase;
added cross-tool compatibility; aligned with research finding that
AGENTS.md stays compact while `.claude/skills/` provides modularization.
