# Select and Adapt Baseline Dev Tooling Skills

## Context and Problem Statement

The project needs a set of general-purpose agent skills for everyday
development work (commits, code review, documentation, testing, MCP
development). Should we write these from scratch, or import established
skills from the open-source ecosystem?

## Considered Options

* Write all skills from scratch
* Import from a single source (Anthropic's official skills)
* Import from multiple sources, adapting where needed

## Decision Outcome

Chosen option: "Import from multiple sources, adapting where needed."

We imported from two sources:

**Anthropic (unmodified):** `doc-coauthoring`, `mcp-builder`,
`webapp-testing` — these are vendor-maintained, well-tested, and
directly useful without changes.

**Sentry (adapted for SourceHut):** `commit`, `find-bugs` — Sentry's
skills assume GitHub workflows. We adapted them:
- `commit`: SourceHut `todo.sr.ht` issue trailers instead of GitHub refs
- `find-bugs`: `git` branch detection instead of `gh` CLI (no GitHub CLI
  on SourceHut-hosted projects)

### Consequences

* Good, because we get battle-tested skills immediately
* Good, because Anthropic's skills track their own platform changes
* Good, because Sentry's commit discipline is well-designed and proven
* Neutral, because adapted skills require checking diffs before upstream updates
* Bad, because we now depend on external repos for updates (mitigated by
  vendoring copies with attribution tracking)
