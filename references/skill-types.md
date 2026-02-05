# Skill Types Reference

## Type Classification Matrix

| Type | Max Lines | Structure | Use When |
|------|-----------|-----------|----------|
| **Simple** | 200 | SKILL.md only | Single-purpose, minimal logic |
| **Standard** | 400 | SKILL.md + references/ | Multi-step workflow, domain knowledge |
| **Complex** | 500 | SKILL.md + references/ + scripts/ | MCP, subagents, external tools |
| **Command** | 100 | Single .md file in commands/ | User-initiated /command |

## Simple Skills (< 200 lines)

**Characteristics:**
- Single-purpose, focused functionality
- Minimal branching logic
- No external dependencies
- Self-contained in SKILL.md

**Structure:**
```
skill-name/
└── SKILL.md
```

**Examples:**
- Format code with Prettier
- Generate Git commit message
- Create TODO list from comments

## Standard Skills (< 400 lines)

**Characteristics:**
- Multi-step workflows
- Domain-specific knowledge required
- Reference materials needed
- Progressive disclosure pattern

**Structure:**
```
skill-name/
├── SKILL.md (core logic, < 400 lines)
└── references/
    ├── patterns.md
    ├── examples.md
    └── checklist.md
```

**Examples:**
- API design validator
- Database migration generator
- Test suite creator

## Complex Skills (< 500 lines)

**Characteristics:**
- MCP server integration
- Multi-agent coordination
- External tool orchestration
- Script execution needed

**Structure:**
```
skill-name/
├── SKILL.md (orchestration logic, < 500 lines)
├── references/
│   ├── patterns.md
│   ├── api-reference.md
│   └── troubleshooting.md
└── scripts/
    ├── validate.sh
    └── setup.sh
```

**Examples:**
- Full-stack app scaffolder
- CI/CD pipeline builder
- Multi-language refactoring agent

## Command Skills (< 100 lines)

**Characteristics:**
- User-initiated via /command
- Immediate, single action
- No persistent state
- Imperative execution

**Structure:**
```
commands/
└── command-name.md (< 100 lines)
```

**Examples:**
- /format - Format current file
- /commit - Create Git commit
- /deploy - Deploy to production

## Type Selection Decision Tree

```
START
  │
  ├─ User-initiated command? → COMMAND (< 100 lines)
  │
  ├─ Needs MCP or subagents? → COMPLEX (< 500 lines)
  │
  ├─ Multi-step workflow? → STANDARD (< 400 lines)
  │
  └─ Single-purpose? → SIMPLE (< 200 lines)
```

## Anti-Patterns

| Don't | Do Instead |
|-------|------------|
| Mix command + skill in one file | Separate into commands/ and skills/ |
| Exceed line limits | Split into references/ |
| Duplicate content across types | Use inherits: in skill.yaml |
| Create Standard when Simple works | Start Simple, upgrade if needed |
