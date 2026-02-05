# Validation Rules & Checklist

## File Size Limits (MANDATORY)

| File | Max Lines | Max Chars | Format |
|------|-----------|-----------|--------|
| skill.yaml | 150 | 10,000 | YAML |
| SKILL.md | 500 | 100,000 | Markdown |
| references/*.md | 1,000 | 200,000 | Markdown |
| scripts/*.sh | No limit | No limit | Bash |

**Enforcement:** CI/CD checks fail if limits exceeded.

---

## skill.yaml Validation

### Metadata Requirements

**Name:**
- Format: kebab-case
- Max length: 64 characters
- Pattern: `^[a-z0-9]+(-[a-z0-9]+)*$`
- No uppercase, no underscores

**Examples:**
- ✅ `api-validator`
- ✅ `database-migration-pro`
- ❌ `API_Validator` (uppercase)
- ❌ `database_migration` (underscore)

**Version:**
- Format: Semantic versioning (X.Y.Z)
- Pattern: `^\d+\.\d+\.\d+$`
- Must match MASTER_COORDINATE version (18.x.x)

**Examples:**
- ✅ `18.0.0`
- ✅ `18.1.5`
- ❌ `v18.0` (missing patch)
- ❌ `1.0` (missing MASTER version)

**Description:**
- Max length: 1024 characters
- Format: WHAT + WHEN + TRIGGERS
- Must be descriptive, not vague
- Use imperative form

**Template:**
```
[SKILL NAME] v[VERSION] - [PRIMARY PURPOSE].
Triggers on: [TRIGGER PHRASES].
Supports: [KEY CAPABILITIES].
[ONE-LINE SUMMARY OF VALUE].
```

**Example:**
```yaml
description: |
  API Validator v21.0.0-SLIM - Validates REST/GraphQL APIs against OpenAPI/GraphQL schemas.
  Triggers on: "validate api", "check api", "api compliance".
  Supports: OpenAPI 3.x, GraphQL introspection, contract testing.
  Ensures API responses match schema, catches breaking changes.
```

---

## SKILL.md Structure Validation

### Required Sections (in order)

1. **Role** (What is this skill?)
2. **Capabilities** (What can it do?)
3. **Triggers** (When to activate?)
4. **Structured Chain-of-Thought (SCoT)** (How to think?)
5. **Commands** (If applicable)
6. **Tools Available** (Which tools can be used?)
7. **Protocol** (Execution workflow)
8. **Verification** (How to validate success?)

---

## Form & Style Validation

### Imperative Form (MANDATORY)

**Bad (passive voice):**
- "The skill should be triggered when..."
- "Files can be read by using..."
- "Validation is performed by..."

**Good (imperative):**
- "Trigger when..."
- "Read files using..."
- "Validate by..."

---

## Anti-Patterns (FORBIDDEN)

### 1. README.md Files

**Don't:**
```
skill-name/
├── README.md  ❌
└── SKILL.md
```

**Why:** Redundant. Use SKILL.md for all documentation.

### 2. CHANGELOG.md Files

**Don't:**
```
skill-name/
├── CHANGELOG.md  ❌
└── SKILL.md
```

**Why:** Use Git history for version tracking.

### 3. Prose Paragraphs

**Don't:**
```markdown
This skill helps you create database migrations. It analyzes your schema changes
and generates migration files. You can use it to create both up and down migrations.
The skill supports PostgreSQL, MySQL, and SQLite databases.
```

**Do:**
```markdown
| Feature | Support |
|---------|---------|
| Purpose | Generate database migrations |
| Direction | Up & down migrations |
| Databases | PostgreSQL, MySQL, SQLite |
```

---

## Verification Gates v21.0.0

### Research Verification Gates

| Gate | Name | Validation | Pass Criteria |
|------|------|------------|---------------|
| G-R1 | RESEARCH_TRIGGERED | Research phase executed | Phase 0 completed |
| G-R2 | CONTEXT7_QUERIED | Context7 docs checked | At least 1 query |
| G-R3 | MEMORY_CHECKED | Memory MCP searched | Patterns retrieved |
| G-R4 | SOURCES_CITED | External sources documented | >= 1 source |

### Security Verification Gates

| Gate | Name | Validation | Pass Criteria |
|------|------|------------|---------------|
| G-S1 | INJECTION_SAFE | No prompt injection patterns | 0 detections |
| G-S2 | OUTPUT_SANITIZED | Output validation enabled | Sanitizer active |
| G-S3 | PII_REDACTED | No PII in outputs | 0 exposures |
| G-S4 | ENCODING_SAFE | No unicode attacks | 0 detections |

### Test Verification Gates

| Gate | Name | Validation | Pass Criteria |
|------|------|------------|---------------|
| G-T1 | GOLDEN_SET_EXISTS | Test cases created | >= 5 per category |
| G-T2 | COVERAGE_MET | Test coverage threshold | > 80% |
| G-T3 | SECURITY_TESTS | Security test cases | >= 3 |
| G-T4 | BENCHMARK_PASSED | Industry comparison | Score > 0.85 |

### Self-Improvement Gates

| Gate | Name | Validation | Pass Criteria |
|------|------|------------|---------------|
| G-I1 | UNCERTAINTY_CHECKED | Confidence evaluated | Score calculated |
| G-I2 | REFLEXION_LOGGED | Learning captured | Memory updated |
| G-I3 | EVOLUTION_SAFE | Self-mod within bounds | No forbidden changes |

### Gate Summary v21.0.0

| Category | Gates | Count |
|----------|-------|-------|
| Core APEX Gates | G-01 to G-52 | 52 |
| Research Gates | G-R1 to G-R4 | 4 |
| Security Gates | G-S1 to G-S4 | 4 |
| Test Gates | G-T1 to G-T4 | 4 |
| Self-Improvement Gates | G-I1 to G-I3 | 3 |
| **Total** | **All Gates** | **67** |

---

## Pre-Commit Checklist

Before finalizing any skill:

### File Validation
- [ ] skill.yaml < 150 lines
- [ ] SKILL.md < 500 lines
- [ ] Valid YAML syntax
- [ ] No README.md or CHANGELOG.md

### Content Validation
- [ ] Description < 1024 chars
- [ ] Imperative form used throughout
- [ ] Tables used instead of prose
- [ ] No duplicate content
- [ ] Examples in references/

### Structure Validation
- [ ] All required sections present
- [ ] Sections in correct order
- [ ] Headers follow hierarchy (H2 → H3 → H4)
- [ ] No more than 3 heading levels

### Gate Validation
- [ ] All 67 gates pass (52 Core + 15 Extended)
- [ ] All 6 APEX Laws compliant
- [ ] Research gates validated (G-R1 to G-R4)
- [ ] Security gates validated (G-S1 to G-S4)
- [ ] Test gates validated (G-T1 to G-T4)
- [ ] Self-improvement gates validated (G-I1 to G-I3)
- [ ] Security deny patterns configured
- [ ] Token efficiency score > 0.5

**Golden Rule:** If any validation fails, fix before proceeding. Never skip validation.
