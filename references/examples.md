# SKILL EXAMPLES

## Table of Contents

1. [Slash Command Examples](#slash-command-examples)
2. [Simple Skill Examples](#simple-skill-examples)
3. [Standard Skill Examples](#standard-skill-examples)
4. [Complex Skill Examples](#complex-skill-examples)

---

## Slash Command Examples

### Example 1: Simple Utility

```markdown
---
description: "Format JSON with 2-space indentation. Use with /format-json"
---

# Format JSON

1. Read input file or clipboard content
2. Parse as JSON (error if invalid)
3. Format with 2-space indentation
4. Output to same file or display
```

### Example 2: Parameterized Command

```markdown
---
description: "Create component with name. Usage: /component ButtonPrimary"
allowed-tools: Write, Read
---

# Create Component

Parse $ARGUMENTS as ComponentName.

1. Create `src/components/$ARGUMENTS/$ARGUMENTS.tsx`
2. Create `src/components/$ARGUMENTS/index.ts`
3. Use project conventions from AI_CONTEXT.md
```

### Example 3: Git Workflow

```markdown
---
description: "Smart commit with conventional format. Usage: /commit fix: button alignment"
allowed-tools: Bash
---

# Smart Commit

1. Run `git status` to verify changes
2. Parse $ARGUMENTS for type and message
3. Validate conventional commit format
4. Execute `git add -A && git commit -m "$ARGUMENTS"`
```

---

## Simple Skill Examples

### Example 1: Code Review

```markdown
---
name: code-reviewer
description: "Review code for quality issues. Triggers on: 'review code', 'check code', 'audit code'. Analyzes structure, patterns, and potential bugs."
---

# Code Reviewer

## Workflow

1. Read specified files or recent changes
2. Analyze against quality patterns
3. Check for common anti-patterns
4. Output findings with severity levels

## Output Format

- 🔴 Critical: Security/correctness issues
- 🟡 Warning: Performance/maintainability
- 🔵 Info: Style/conventions
```

### Example 2: Documentation Generator

```markdown
---
name: doc-generator
description: "Generate documentation from code. Triggers on: 'document function', 'add jsdoc', 'generate docs'. Creates JSDoc/TSDoc comments."
---

# Documentation Generator

## Workflow

1. Parse function/class signature
2. Infer parameter types and purpose
3. Generate appropriate doc format
4. Insert above target code
```

---

## Standard Skill Examples

### Example 1: API Client Generator

```
api-generator/
├── SKILL.md
└── references/
    └── patterns.md
```

**SKILL.md:**

```markdown
---
name: api-generator
description: "Generate TypeScript API clients from OpenAPI specs. Triggers on: 'generate api', 'create client', 'openapi to typescript'."
allowed-tools: Read, Write, WebFetch
---

# API Client Generator

## Workflow

1. Fetch or read OpenAPI spec
2. Parse endpoints and schemas
3. Generate typed client functions
4. Output to specified directory

## Patterns

See [references/patterns.md] for output patterns.
```

### Example 2: Test Generator

```
test-generator/
├── SKILL.md
└── references/
    ├── jest-patterns.md
    └── vitest-patterns.md
```

**SKILL.md:**

```markdown
---
name: test-generator
description: "Generate unit tests for functions. Triggers on: 'generate tests', 'write tests', 'add tests'. Supports Jest and Vitest."
---

# Test Generator

## Workflow

1. Read target function
2. Detect test framework (Jest/Vitest)
3. Generate test cases
4. Output to `.test.ts` file

## Framework Patterns

- Jest: See [references/jest-patterns.md]
- Vitest: See [references/vitest-patterns.md]
```

---

## Complex Skill Examples

### Example 1: Full-Stack Feature Generator

```
feature-generator/
├── SKILL.md
├── references/
│   ├── nextjs-patterns.md
│   ├── api-patterns.md
│   └── db-patterns.md
├── scripts/
│   └── scaffold.py
└── assets/
    └── templates/
        ├── page.tsx.template
        ├── api-route.ts.template
        └── schema.prisma.template
```

**SKILL.md:**

```markdown
---
name: feature-generator
description: "Generate full-stack features with UI, API, and DB. Triggers on: 'create feature', 'new feature', 'scaffold feature'. Creates Next.js pages, API routes, and Prisma schemas."
allowed-tools: Read, Write, Bash
---

# Feature Generator

## Workflow

1. Parse feature requirements
2. Generate Prisma schema changes
3. Create API route with validation
4. Build React component
5. Add to navigation

## Reference Navigation

- Next.js: `grep "^## Page" references/nextjs-patterns.md`
- API: `grep "^## Route" references/api-patterns.md`
- DB: `grep "^## Schema" references/db-patterns.md`

## Scripts

Run `python scripts/scaffold.py --name $FEATURE` for boilerplate.
```

### Example 2: Migration Assistant

```
migration-assistant/
├── SKILL.md
├── references/
│   ├── breaking-changes.md
│   ├── codemods.md
│   └── testing-checklist.md
├── scripts/
│   ├── analyze.py
│   └── transform.py
└── assets/
    └── migration-report.html.template
```

**SKILL.md:**

```markdown
---
name: migration-assistant
description: "Assist with framework/library migrations. Triggers on: 'migrate to', 'upgrade from', 'migration help'. Analyzes codebase and applies codemods."
allowed-tools: Read, Write, Bash, Glob, Grep
---

# Migration Assistant

## Workflow

1. Identify current versions
2. Analyze breaking changes impact
3. Apply automated codemods
4. Flag manual migration points
5. Generate migration report

## Analysis

Run `python scripts/analyze.py` for impact assessment.

## Codemods

See [references/codemods.md] for transformation patterns.

## Checklist

See [references/testing-checklist.md] for validation steps.
```

---

## Anti-Pattern Examples (NEVER DO THIS)

### ❌ Too Verbose

```markdown
---
name: helper
description: "Helps with stuff" # Too vague!
---

# Helper Skill

This skill is designed to help you with various tasks.
It will analyze your request and try to provide assistance.

## How to Use

To use this skill, simply describe what you need help with...
```

### ❌ Missing Structure

```markdown
---
description: "Does things" # Missing name for skill!
---

Do the thing the user asked for.
```

### ❌ Too Large (>500 lines)

```markdown
# Giant Skill

[600 lines of content that should be split into references/]
```

---

## Quick Templates

### Slash Command Template

```markdown
---
description: "[ACTION] [TARGET]. Usage: /[name] [args]"
allowed-tools: [TOOLS]
---

# [Name]

1. [Step 1]
2. [Step 2]
3. [Step 3]
```

### Simple Skill Template

```markdown
---
name: [kebab-case]
description: "[WHAT]. Triggers on: [TRIGGERS]. [CAPABILITIES]."
---

# [Name]

## Workflow

1. [Step]
2. [Step]
3. [Step]
```

### Standard Skill Template

```markdown
---
name: [kebab-case]
description: "[WHAT]. Triggers on: [TRIGGERS]. [CAPABILITIES]."
allowed-tools: [TOOLS]
---

# [Name]

## Workflow

[Core steps only]

## References

- [Topic]: See [references/topic.md]
```

---

**Verification:** 📋 EXAMPLES v16.2.0
