# FORGE DESIGN PATTERNS

## Table of Contents

1. [Skill Type Selection](#skill-type-selection)
2. [Degrees of Freedom](#degrees-of-freedom)
3. [Progressive Disclosure](#progressive-disclosure)
4. [YAML Frontmatter](#yaml-frontmatter)
5. [Instruction Patterns](#instruction-patterns)
6. [Structure Patterns](#structure-patterns)
7. [Anti-Patterns](#anti-patterns)

---

## Skill Type Selection

### Decision Matrix

| Trigger               | Complexity | Type          | Location              |
| --------------------- | ---------- | ------------- | --------------------- |
| User types `/command` | Simple     | Slash Command | `~/.claude/commands/` |
| User types `/command` | Complex    | Slash Command | `~/.claude/commands/` |
| Natural language      | Any        | Agent Skill   | `~/.claude/skills/`   |
| Auto-detect context   | Any        | Agent Skill   | `~/.claude/skills/`   |

### When to Use Slash Commands

- User-initiated actions
- Simple, single-purpose tasks
- Quick utilities
- No context detection needed

### When to Use Agent Skills

- Context-triggered capabilities
- Multi-step workflows
- Domain expertise
- Complex file structures needed

---

## Degrees of Freedom

### The Bridge Metaphor

```
HIGH FREEDOM (Open Field)
├── Multiple valid approaches
├── Context-dependent decisions
└── Heuristic guidance

MEDIUM FREEDOM (Wide Path)
├── Preferred pattern exists
├── Some variation acceptable
└── Pseudocode guidance

LOW FREEDOM (Narrow Bridge)
├── Fragile operations
├── Consistency critical
└── Exact scripts required
```

### Application

| Freedom | Use When          | Instruction Style           |
| ------- | ----------------- | --------------------------- |
| High    | Creative tasks    | "Consider approaches..."    |
| Medium  | Standard patterns | "Prefer X, alternatively Y" |
| Low     | Deterministic ops | "Execute script exactly"    |

### Examples

**High Freedom (Creative)**

```markdown
## Image Selection

Consider composition, color harmony, and subject relevance.
Balance aesthetic appeal with message clarity.
```

**Medium Freedom (Pattern)**

```markdown
## API Integration

Prefer REST endpoints. Use GraphQL if real-time needed.
Always implement retry logic with exponential backoff.
```

**Low Freedom (Deterministic)**

```markdown
## PDF Rotation

Execute: `python scripts/rotate_pdf.py --input $FILE --degrees 90`
Do not modify parameters.
```

---

## Progressive Disclosure

### Three-Level Loading

```
Level 1: Metadata (Always loaded)
├── name: ~20 tokens
└── description: ~100 tokens

Level 2: SKILL.md (On trigger)
└── Body: <500 lines, <5000 words

Level 3: Bundled Resources (On demand)
├── references/ (loaded when needed)
├── scripts/ (executed, not loaded)
└── assets/ (used in output)
```

### Split Triggers

| Condition             | Action             |
| --------------------- | ------------------ |
| SKILL.md > 300 lines  | Start splitting    |
| SKILL.md > 500 lines  | Must split         |
| Reference > 100 lines | Add TOC            |
| Reference > 500 lines | Consider sub-split |
| Repeated code block   | Move to scripts/   |
| Template file         | Move to assets/    |

### Cross-Reference Pattern

```markdown
## Core Workflow

1. Parse input
2. Validate schema (see [references/schemas.md])
3. Execute transformation
4. Output results

## Advanced Options

- **Custom schemas**: See [references/schemas.md#custom]
- **Error handling**: See [references/errors.md]
```

### Domain Organization

```
bigquery-skill/
├── SKILL.md (overview + routing)
└── references/
    ├── finance.md (revenue queries)
    ├── sales.md (pipeline queries)
    └── product.md (usage queries)
```

Claude loads only the relevant domain file.

---

## YAML Frontmatter

### Required Fields

```yaml
---
name: skill-name # Required: kebab-case, max 64 chars
description: "..." # Required: max 1024 chars
---
```

### Optional Fields

```yaml
---
name: skill-name
description: "..."
allowed-tools: Read, Write, Bash # Restrict tool access
---
```

### Description Best Practices

**Structure:** WHAT + WHEN + TRIGGERS

```yaml
# BAD - Vague
description: "Helps with PDFs"

# GOOD - Complete
description: "Process PDF documents including rotation, text extraction, and merging. Use when working with .pdf files for: (1) rotating pages, (2) extracting text, (3) combining documents, or (4) splitting pages."
```

### Name Conventions

- Use gerund form: `document-processing`, `code-reviewing`
- Max 64 characters
- Lowercase letters, numbers, hyphens only
- No XML tags, no reserved words

---

## Instruction Patterns

### Imperative Form (ALWAYS)

```markdown
# BAD

This skill will help you process documents.

# GOOD

Process documents using the following workflow.
```

### Step-by-Step

```markdown
## Workflow

1. **Parse** input to extract file paths
2. **Validate** all files exist
3. **Execute** transformation
4. **Output** results with summary
```

### Conditional Logic

```markdown
## Processing

IF input is single file:
Process directly
ELSE IF input is directory:
Iterate files with glob pattern
ELSE:
Request clarification
```

### Decision Trees

```markdown
## API Selection
```

Is real-time needed?
├── Yes → Use WebSocket
└── No → Is batch processing?
├── Yes → Use Queue API
└── No → Use REST API

```

```

---

## Structure Patterns

### Minimal Skill (Slash Command)

```markdown
---
description: "Rotate images 90 degrees clockwise."
---

# Rotate Image

Execute rotation on provided image file:

1. Validate file is image format
2. Apply 90-degree clockwise rotation
3. Save with "\_rotated" suffix
```

### Standard Skill

```markdown
---
name: document-processor
description: "Process documents for format conversion and text extraction."
---

# Document Processor

## Purpose

Convert and extract content from various document formats.

## Supported Formats

- PDF, DOCX, TXT, MD

## Workflow

1. Detect input format
2. Apply appropriate parser
3. Execute requested operation
4. Output in target format

## Advanced

- See [references/formats.md] for format details
```

### Complex Skill

```
complex-skill/
├── SKILL.md (routing + core workflow)
├── references/
│   ├── api.md (API documentation)
│   ├── schemas.md (data schemas)
│   └── examples.md (usage examples)
├── scripts/
│   ├── transform.py (deterministic transform)
│   └── validate.sh (validation script)
└── assets/
    └── templates/
        └── output.html (output template)
```

---

## Anti-Patterns

### NEVER Include

| File            | Reason                 |
| --------------- | ---------------------- |
| README.md       | Skill IS the readme    |
| CHANGELOG.md    | Version in frontmatter |
| INSTALLATION.md | Not needed for skills  |
| LICENSE.md      | Inherited from parent  |
| .gitignore      | Not relevant           |

### Content Anti-Patterns

```markdown
# BAD - Obvious statement

This skill is designed to help you with...

# BAD - Meta-commentary

The following instructions will guide you...

# BAD - Redundant explanation

In order to process the file, you need to first read it.

# BAD - Passive voice

The file should be processed by...
```

### Structure Anti-Patterns

- Nesting deeper than 3 levels
- SKILL.md over 500 lines
- Duplicating content between SKILL.md and references
- Scripts that could be inline code
- Empty directories

---

**Verification:** 📐 PATTERNS v16.2.0
