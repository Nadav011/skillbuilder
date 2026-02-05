# Skill Template Reference

> Complete template structures for all skill types.

## Table of Contents

1. [Directory Structures](#directory-structures)
2. [skill.yaml Templates](#skillyaml-templates)
3. [SKILL.md Templates](#skillmd-templates)
4. [References Templates](#references-templates)

---

## Directory Structures

### Simple Skill (~200 lines)

```
simple-skill/
├── skill.yaml        # 30-50 lines
└── SKILL.md          # 100-200 lines
```

### Standard Skill (~400 lines)

```
standard-skill/
├── skill.yaml        # 50-100 lines
├── SKILL.md          # 200-300 lines
└── references/
    ├── patterns.md   # Domain patterns
    └── examples.md   # Usage examples
```

### Complex Skill (~500 lines)

```
complex-skill/
├── skill.yaml        # 80-150 lines
├── SKILL.md          # 250-400 lines
├── references/
│   ├── patterns.md
│   ├── examples.md
│   └── advanced.md
├── scripts/
│   └── helper.sh
└── assets/
    └── templates/
```

### Slash Command

```
commands/
└── command-name.md   # 50-100 lines
```

---

## skill.yaml Templates

### Minimal (Required Fields Only)

```yaml
name: skill-name
version: "1.0.0"
description: "WHAT it does. Triggers on: TRIGGERS. Use when: CONTEXT."
triggers:
  - "trigger phrase"
```

### Standard

```yaml
name: skill-name
version: "1.0.0"
description: |
  WHAT it does in one sentence. Triggers on: [trigger phrases].
  Supports: [features]. Use when: [specific contexts].

inherits:
  - ~/.claude/MASTER_COORDINATE.md

triggers:
  - "primary trigger"
  - "alternate trigger"
  - "another variation"

allowed_tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep

commands:
  - name: /skill-command
    description: What the command does
```

### Complex (Full Specification)

```yaml
name: complex-skill
version: "1.0.0"
description: |
  Comprehensive description of WHAT the skill does.
  Triggers on: [list of trigger phrases].
  Supports: [feature list]. Use when: [contexts].
  Integrates with: [MCP servers, external tools].

inherits:
  - ~/.claude/MASTER_COORDINATE.md
  - shared-protocols/rtl-verification.md

phase: IMPLEMENTATION
role: DOMAIN_EXPERT

triggers:
  - "primary trigger phrase"
  - "alternate trigger"
  - "third variation"
  - "domain-specific trigger"

allowed_tools:
  - Read
  - Write
  - Edit
  - Bash
  - Glob
  - Grep
  - WebFetch
  - WebSearch
  - Task

commands:
  - name: /main-command
    description: Primary action
  - name: /secondary-command
    description: Secondary action

mcp_servers:
  - supabase
  - playwright

security_template:
  deny:
    - ".env"
    - ".env.*"
    - "secrets/**"
    - "*.pem"
    - "*.key"

prime_directives:
  - READ MASTER_COORDINATE.md FIRST
  - Enforce RTL (Law #5)
  - Enforce Responsive (Law #6)
  - Verify 52/70 gates
```

---

## SKILL.md Templates

### Simple Template

```markdown
# Skill Name v1.0

> One-line purpose.

## PURPOSE

What this skill does (2 sentences).

## WORKFLOW

1. **Parse** input
2. **Execute** operation
3. **Output** result

## RULES

- Rule 1
- Rule 2
```

### Standard Template

```markdown
# Skill Name v1.0

> One-line purpose statement with key capability.

## PURPOSE

What this skill does and when to use it (2-3 sentences max).

## COMMANDS

| Command | Description | Example |
|---------|-------------|---------|
| /cmd | Action | `/cmd arg` |

## WORKFLOW

1. **Parse** - Extract and validate input
2. **Analyze** - Process requirements
3. **Execute** - Perform core operation
4. **Output** - Return formatted results

## RULES

- Use imperative form
- Tables over prose
- Progressive disclosure

## ERROR HANDLING

| Error | Response |
|-------|----------|
| Invalid input | Request clarification |
| Missing file | Report with path |

## REFERENCES

- [patterns.md]: Domain patterns
- [examples.md]: Usage examples
```

### Complex Template

```markdown
# Skill Name v1.0

> Purpose statement describing the GOD-TIER capability.

## PURPOSE

Comprehensive description of what this skill does,
when to use it, and key capabilities (3 sentences max).

## COMMANDS

| Command | Description | Example |
|---------|-------------|---------|
| /main | Primary action | `/main "input"` |
| /alt | Alternative action | `/alt --flag value` |
| /config | Configuration | `/config setting=value` |

## WORKFLOW

### Phase 1: INITIALIZATION

| Step | Action | Tool |
|------|--------|------|
| 1.1 | Parse input | Read |
| 1.2 | Validate requirements | Grep |
| 1.3 | Load configuration | Read |

### Phase 2: EXECUTION

| Step | Action | Tool |
|------|--------|------|
| 2.1 | Process data | Edit |
| 2.2 | Apply transformations | Bash |
| 2.3 | Generate output | Write |

### Phase 3: VERIFICATION

| Step | Action | Tool |
|------|--------|------|
| 3.1 | Validate output | Read |
| 3.2 | Run tests | Bash |
| 3.3 | Report results | - |

## DECISION TREE

```
Input Type?
├── Type A → Process with method A
├── Type B → Process with method B
└── Unknown → Request clarification
```

## RULES

| Rule | Implementation |
|------|----------------|
| Validation | Zod schema for all inputs |
| Error handling | Result<T,E> pattern |
| Performance | Under 500 lines |

## ERROR HANDLING

| Error | Cause | Response |
|-------|-------|----------|
| INVALID_INPUT | Malformed data | Return validation errors |
| NOT_FOUND | Missing resource | Report with suggestions |
| PERMISSION | Access denied | Escalate to user |

## REFERENCES

- [patterns.md]: Domain patterns and best practices
- [examples.md]: Complete usage examples
- [advanced.md]: Advanced configuration
- [troubleshooting.md]: Common issues

## VERIFICATION

```
[ ] Input validated
[ ] Operation completed
[ ] Output verified
[ ] Tests passed
```

---

<!-- SKILL_TEMPLATE v23.0.0 | Updated: 2026-01-27 -->
```

### Slash Command Template

```markdown
---
description: "ACTION TARGET. Usage: /command [args]. Supports: [features]."
allowed-tools: Read, Write, Edit
---

# Command Name

1. **Parse** $ARGUMENTS for parameters
2. **Validate** input requirements
3. **Execute** operation
4. **Output** result

## Parameters

| Param | Required | Default | Description |
|-------|----------|---------|-------------|
| arg1 | Yes | - | First argument |
| arg2 | No | "default" | Optional second |

## Examples

```bash
/command value1
/command value1 --flag value2
```
```

---

## References Templates

### patterns.md

```markdown
# Domain Patterns

## Table of Contents

1. [Pattern Category A](#pattern-category-a)
2. [Pattern Category B](#pattern-category-b)

---

## Pattern Category A

### Pattern A1: Name

**Use When:** Context description

**Implementation:**
```code
example
```

**Avoid:** Anti-pattern description

### Pattern A2: Name

...

---

## Pattern Category B

...
```

### examples.md

```markdown
# Usage Examples

## Basic Examples

### Example 1: Simple Use Case

**Input:**
```
user input
```

**Output:**
```
expected output
```

### Example 2: With Options

...

## Advanced Examples

### Example 3: Complex Workflow

...
```

### advanced.md

```markdown
# Advanced Features

## Configuration

### Option A

| Setting | Type | Default | Description |
|---------|------|---------|-------------|
| opt1 | string | "value" | Description |

## Integration

### MCP Server X

Usage instructions...

## Troubleshooting

| Issue | Cause | Solution |
|-------|-------|----------|
| Error X | Cause | Fix |
```

---

## Field Specifications

### name

- Max 64 characters
- Lowercase letters, numbers, hyphens only
- kebab-case format
- Must be unique across all skills

### description

- Max 1024 characters
- Format: WHAT + WHEN + TRIGGERS
- Third person, imperative mood
- Include activation triggers

### triggers

- Natural language phrases
- Specific and unique
- Cover common variations
- Avoid overlapping with other skills

### allowed_tools

Standard tools:
- Read, Write, Edit (file operations)
- Bash (shell commands)
- Glob, Grep (search)
- WebFetch, WebSearch (web)
- Task (subagents)

---

<!-- SKILL_TEMPLATE v23.0.0 | Updated: 2026-01-27 -->
