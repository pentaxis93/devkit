# Template Roadmap

Evolution plan for the sendhelp project template.

## v0.1.0 (Released 2026-01-30)

- Copier template structure with `_subdirectory` isolation
- 23 agent skills (pipeline, BDD, dev tooling, cross-cutting)
- Template variables: project_name, project_slug, project_description
- Root README documenting template usage
- Generated project README via Jinja template

## v0.2.0 (In Progress)

### Phase 1: Core Dev Environment

- [ ] **AGENTS.md template** - Compact project-level agent instructions
      with skill references (< 200 lines)
- [ ] **agents-md skill** - Imported from Beadsmith, adapted to current
      best practices for writing/maintaining AGENTS.md
- [ ] **voice-coauthoring skill** - Optimize agent responses for users
      reading text and responding via voice
- [ ] **OpenCode slash commands** - `.opencode/commands/` for 8 pipeline
      skills: vision-workshop, feature-discovery, journey-mapping,
      beads-plan, beads-execute, epic-validation, coherence-audit, commit
- [ ] **Research tool config** - `.env.example` with Tavily API key,
      MCP server configuration for AI-powered search
- [ ] **LICENSE** - MIT license file

### Phase 2: Project Hygiene

- [ ] **Git hooks** - `.githooks/` with pre-commit secret detection
- [ ] **CONTRIBUTING.md** - How to contribute to projects using this template
- [ ] **.editorconfig** - Consistent coding styles across editors
- [ ] **CHANGELOG.md** - Empty scaffold for tracking project changes

### Phase 3: CI/CD & Automation

- [ ] **GitHub Actions workflows** - Test runner, linter, dependency updates
- [ ] **GitLab CI alternative** (optional, via template question)
- [ ] **Pre-commit hook framework** - Expandable hook system beyond secrets

## Future Considerations

### Language-Specific Scaffolding

Template questions could conditionally include:
- Python: `pyproject.toml`, `src/` layout, pytest config
- TypeScript: `package.json`, `tsconfig.json`, `src/` layout
- Rust: `Cargo.toml`, `src/` layout

### Platform-Specific Adaptation

Template questions could adapt skills for:
- GitHub vs SourceHut (issue trailers, branch detection)
- GitHub Actions vs GitLab CI vs Forgejo Actions

### MCP Server Configuration

- Template for `.claude/mcp.json` or equivalent
- Pre-configured research tools (Tavily, web search)
- Project-specific MCP servers

### Skill Updates

- Upstream tracking for Anthropic and Sentry skills
- Automated diff checking for adapted skills
- Skill versioning and compatibility matrix
