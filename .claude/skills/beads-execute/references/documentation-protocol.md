# Project Documentation Protocol

Documentation is part of the work, not an afterthought. Follow these guidelines for when and how to document.

## When to Create/Update Documentation

**Create documentation when:**

1. **A bead explicitly requires it** - The acceptance criteria mentions docs, README, etc.
2. **You create a new public API** - Functions, endpoints, or interfaces others will use
3. **You add a new command/script** - CLI tools need usage documentation
4. **You set up a new service/component** - How to run, configure, deploy
5. **You implement non-obvious behavior** - Complex algorithms, business logic, edge cases
6. **The project has no README** - Create a minimal one with setup instructions

**Update documentation when:**

1. **You change existing documented behavior** - Keep docs in sync with code
2. **You deprecate or remove features** - Note what changed and migration path
3. **You discover the docs are wrong** - Fix them immediately

**Do NOT create documentation for:**

1. **Self-documenting code** - Well-named functions don't need paragraph explanations
2. **Internal implementation details** - Unless specifically requested
3. **Speculative features** - Only document what exists
4. **Personal notes** - Use bead notes, not project docs

## Documentation Locations

Follow project conventions. Common patterns:

```
README.md              # Project overview, quick start, basic usage
docs/                  # Detailed documentation
  architecture.md      # System design, component relationships
  api.md               # API reference
  deployment.md        # How to deploy
  development.md       # How to contribute/develop
CHANGELOG.md           # Version history (if project uses one)
<component>/README.md  # Component-specific docs
```

## Documentation Standards

**README.md minimum viable content:**

```markdown
# Project Name

Brief description of what this does.

## Quick Start

\`\`\`bash
# How to install/setup
# How to run
\`\`\`

## Usage

Basic usage examples.

## Configuration

Key configuration options (if any).
```

**API/Function documentation:**

```markdown
## function_name

Brief description.

### Parameters

- `param1` (type): Description
- `param2` (type, optional): Description. Default: `value`

### Returns

Description of return value.

### Example

\`\`\`language
example code
\`\`\`

### Errors

- `ErrorType`: When this occurs
```

**When documenting commands/scripts:**

```markdown
## command-name

Description of what it does.

### Usage

\`\`\`bash
command-name [options] <required-arg>
\`\`\`

### Options

- `--flag`: Description
- `--option <value>`: Description (default: X)

### Examples

\`\`\`bash
# Common use case
command-name --flag value

# Another use case  
command-name --other-option
\`\`\`
```

## Documentation as Beads

If a feature requires documentation, it should be part of the bead's acceptance criteria:

```
Acceptance criteria:
- Feature X works as specified
- README updated with usage instructions
- API documented in docs/api.md
```

If you complete a bead and realize docs are needed but weren't specified:

```bash
bd create "Docs: Document <feature>" -t chore -p 3 --parent <epic-id> \
  -d "Add documentation for <feature> implemented in <bead-id>.

Needed:
- Usage examples
- Configuration options
- API reference"
```

## Inline Code Documentation

**Do document:**
- Public API contracts (function signatures, types)
- Complex algorithms (brief explanation of approach)
- Non-obvious side effects
- TODO/FIXME with bead references: `// TODO(bd-XXX): description`

**Don't over-document:**
- Obvious code (`i++ // increment i`)
- Every private function
- Implementation details that may change

## Documentation Commits

Documentation changes follow the same commit pattern:

```bash
git commit -m "docs(<bead-id>): <what was documented>"
```

Examples:
```bash
git commit -m "docs(bd-XXX): add API reference for auth endpoints"
git commit -m "docs(bd-YYY): update README with new CLI options"
```
