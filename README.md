# devkit - AI Agent Project Template

A [Copier](https://copier.readthedocs.io/) template for AI-powered software projects with structured planning-to-execution workflows.

## What This Template Provides

**23 reusable agent skills** implementing a 5-stage planning-to-execution pipeline:

### Planning Layer (human-co-creative, markdown artifacts)
1. **vision-workshop** → `planning/vision.md`
2. **feature-discovery** → `planning/features.md`
3. **journey-mapping** → `planning/journeys/<slug>.md`

### Execution Layer (agent-autonomous, beads database)
4. **beads-plan** → bead DAG (tasks + dependencies)
5. **beads-execute** → completed code/tests/docs

### Supporting Skills
- **Quality gates**: validate-pipeline, epic-validation, coherence-audit
- **BDD workflows**: scenario design, step implementation, red-green-refactor, evolution
- **Dev tooling**: commit discipline, bug finding, doc coauthoring, MCP building, web testing
- **Cross-cutting**: clear writing (Strunk's Elements of Style), skill creation

## Usage

### Create a New Project

```bash
# Install Copier
pipx install copier

# Create a new project from this template
copier copy gh:pentaxis93/devkit my-new-project

# Answer the prompts:
# - Project name: My New Project
# - Project slug: my-new-project
# - Description: AI-powered productivity application
```

### Update an Existing Project

Pull template updates (new skills, skill fixes) into an existing project:

```bash
cd my-existing-project
copier update
```

Copier will merge template changes while preserving your project-specific files (`planning/*`, `docs/decisions/*`, customized `.gitignore`).

## Template Structure

```
devkit/                            (this repo - template source)
├── copier.yml                     (template configuration)
├── README.md                      (this file)
├── docs/                          (template meta-documentation)
│   ├── decisions/                 (ADRs about template design)
│   └── postmortems/               (template quality audits)
│
└── template/                      (generated project structure)
    ├── .claude/skills/            (23 agent skills)
    ├── planning/                  (empty scaffold for planning artifacts)
    ├── docs/decisions/            (empty scaffold for project ADRs)
    └── README.md.jinja            (project README template)
```

**What gets copied**: Only `template/` contents go into generated projects.

**What stays here**: Template design decisions (ADR 0001-0006), coherence audit history, and this README.

## Agent Compatibility

Skills work with:
- **[Claude Code](https://docs.anthropic.com/en/docs/claude-code/skills)** (Anthropic)
- **[OpenCode](https://opencode.ai/docs/skills/)** (open source)

Both discover skills from `.claude/skills/<name>/SKILL.md`.

## Documentation

- **Skills catalog**: See `template/.claude/skills/README.md`
- **Template design decisions**: See `docs/decisions/`
- **Quality audits**: See `docs/postmortems/`

## Versioning

This template uses [semantic versioning](https://semver.org/). The version is tracked via git tags:

- **Major** (v2.0.0): Breaking changes to template structure or skill contracts
- **Minor** (v0.2.0): New skills, new template features (backward-compatible)
- **Patch** (v0.1.1): Skill fixes, documentation updates

`copier update` uses git tags to compute template diffs.

## Contributing

Improvements to skills, new pipeline stages, and template structure enhancements are welcome. See the coherence audit checklist in `template/.claude/skills/README.md` for quality standards.

## License

MIT License - see LICENSE file for details.

Skills imported from upstream sources retain their original licenses (see `template/.claude/skills/README.md` for attribution).
