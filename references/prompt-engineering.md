# Prompt Engineering Patterns - SINGULARITY FORGE Reference

## Overview

Advanced prompt engineering patterns for creating world-class AI skills. These patterns optimize for Claude's cognitive architecture.

---

## 1. CORE PRINCIPLES

### The 5 Pillars of Skill Design

| Pillar | Principle | Implementation |
|--------|-----------|----------------|
| **Clarity** | Unambiguous instructions | Imperative form, no passive voice |
| **Efficiency** | Maximum value per token | Tables, progressive disclosure |
| **Reliability** | Consistent outputs | Structured workflows, templates |
| **Safety** | Prevent harmful outputs | Deny patterns, validation |
| **Adaptability** | Handle edge cases | Fallbacks, error handling |

---

## 2. INSTRUCTION HIERARCHY

### Authority Escalation Ladder

```
Level 1: MASTER_COORDINATE.md (Supreme Law - IMMUTABLE)
    ↓
Level 2: Project AI_CONTEXT.md (Project-specific rules)
    ↓
Level 3: Skill SKILL.md (Domain-specific instructions)
    ↓
Level 4: User prompt (Immediate context)
    ↓
Level 5: Claude's base knowledge (Fallback only)
```

### Conflict Resolution

| Conflict | Resolution |
|----------|------------|
| User vs MASTER | MASTER wins (immutable laws) |
| User vs Skill | User wins (explicit override) |
| Skill vs Skill | Most specific wins |
| Ambiguous | ASK user for clarification |

---

## 3. COGNITIVE LOAD OPTIMIZATION

### Progressive Disclosure (3-Level)

```
L1: skill.yaml (~100 tokens)
    └── Load: Always (registry scan)
    └── Contains: Name, triggers, brief description

L2: SKILL.md (200-500 lines)
    └── Load: On skill activation
    └── Contains: Commands, workflow, rules

L3: references/*.md (unlimited)
    └── Load: On-demand only
    └── Contains: Deep documentation, examples
```

### Token Budget Guidelines

| Content Type | Max Tokens | Strategy |
|--------------|------------|----------|
| Skill.yaml | 100 | Minimal metadata |
| SKILL.md | 5,000 | Summary + workflow |
| Single reference | 10,000 | Deep dive on topic |
| Total loaded | 20,000 | Progressive loading |

---

## 4. INSTRUCTION PATTERNS

### Pattern 1: Imperative Commands

```markdown
❌ BAD (Passive/Verbose):
"The user's input should be validated before processing"

✅ GOOD (Imperative):
"Validate input with Zod before processing"
```

### Pattern 2: Decision Tables

```markdown
❌ BAD (Prose):
"If the file is TypeScript, use tsx. If it's JavaScript, use jsx.
If it's a style file, use CSS modules. Otherwise, use the default."

✅ GOOD (Table):
| File Type | Extension | Handler |
|-----------|-----------|---------|
| TypeScript | .tsx | tsx loader |
| JavaScript | .jsx | jsx loader |
| Styles | .module.css | CSS modules |
| Other | * | Default handler |
```

### Pattern 3: Numbered Workflows

```markdown
❌ BAD (Unordered):
"Read the file, then validate it, analyze the content,
generate output, and verify the result."

✅ GOOD (Numbered):
1. **READ** - Load file content
2. **VALIDATE** - Check format with schema
3. **ANALYZE** - Extract key information
4. **GENERATE** - Produce output
5. **VERIFY** - Confirm correctness
```

### Pattern 4: Conditional Logic

```markdown
❌ BAD (Nested prose):
"If the project has a package.json and it contains Next.js,
and the version is 15 or higher, then enable PPR..."

✅ GOOD (Decision tree):
```
Project Detection:
├── Has package.json?
│   ├── Yes → Check dependencies
│   │   ├── Has next.js ≥15? → Enable PPR
│   │   └── Has next.js <15? → Use ISR
│   └── No → Static site mode
```
```

---

## 5. OUTPUT STRUCTURING

### Template Pattern

```markdown
## OUTPUT FORMAT

```
[STATUS_ICON] SKILL_NAME vX.Y.Z - [RESULT]

### Summary
- Key finding 1
- Key finding 2

### Details
| Metric | Value | Status |
|--------|-------|--------|
| ...    | ...   | ✅/⚠️/❌ |

### Next Steps
1. Action item 1
2. Action item 2
```
```

### Status Indicators

| Icon | Meaning | Use When |
|------|---------|----------|
| ✅ | Pass/Success | Gate passed, task complete |
| ⚠️ | Warning | Non-blocking issue |
| ❌ | Fail/Error | Blocking issue |
| 🔄 | In Progress | Async operation running |
| ℹ️ | Info | Neutral information |

---

## 6. ERROR HANDLING PATTERNS

### Graceful Degradation

```markdown
## ERROR HANDLING

| Error | Recovery | Fallback |
|-------|----------|----------|
| File not found | Create with template | Ask user for path |
| Invalid format | Attempt auto-fix | Show manual fix |
| Permission denied | Request elevated | Skip with warning |
| Timeout | Retry with backoff | Partial result |
```

### Error Message Format

```markdown
❌ ERROR: [CODE] - [Brief description]

**What happened:** [Explanation]
**How to fix:** [Actionable steps]
**Fallback:** [Alternative approach]
```

---

## 7. CONTEXT MANAGEMENT

### Context Preservation

```markdown
## CONTEXT RULES

1. **Carry forward** - Reference previous decisions
2. **Summarize long outputs** - Compress after 2000 tokens
3. **Checkpoint state** - Save progress at phase boundaries
4. **Clear when done** - Release context after completion
```

### Entropy Prevention

```markdown
## ENTROPY ANCHORING

Every 25,000 tokens:
1. Re-read MASTER_COORDINATE.md
2. Verify current task alignment
3. Summarize progress so far
4. Confirm next steps with user
```

---

## 8. MULTI-TURN PATTERNS

### Conversation State Machine

```
IDLE ──► ANALYZING ──► PLANNING ──► EXECUTING ──► VERIFYING ──► COMPLETE
                          │              │              │
                          └──────────────┴──────────────┘
                                    (iterate)
```

### Phase Transitions

| From | To | Trigger | Output |
|------|-----|---------|--------|
| IDLE | ANALYZING | User request | "Analyzing..." |
| ANALYZING | PLANNING | Analysis complete | Present plan |
| PLANNING | EXECUTING | User approval | "Executing..." |
| EXECUTING | VERIFYING | Execution complete | Run tests |
| VERIFYING | COMPLETE | All tests pass | Final summary |

---

## 9. SAFETY PATTERNS

### Input Sanitization

```markdown
## SANITIZE ALL INPUTS

1. **Paths:** No `../`, absolute paths, or `/etc/`
2. **Commands:** Parameterize, no shell injection
3. **User data:** Escape before rendering
4. **Secrets:** Never log or display
```

### Output Filtering

```markdown
## FILTER OUTPUTS

| Content | Action |
|---------|--------|
| API keys | Replace with [REDACTED] |
| Passwords | Never include |
| PII | Mask or omit |
| Internal errors | Summarize, don't expose stack |
```

---

## 10. OPTIMIZATION TECHNIQUES

### Token Compression

| Technique | Before | After | Savings |
|-----------|--------|-------|---------|
| Tables vs prose | 120 tokens | 45 tokens | 62% |
| Inline vs bullets | 80 tokens | 35 tokens | 56% |
| Acronyms with def | 50 tokens | 20 tokens | 60% |

### Lazy Evaluation

```markdown
## LOAD ON DEMAND

- Don't read files until needed
- Don't compute until required
- Don't fetch until requested
- Cache results for reuse
```

---

## 11. TESTING PATTERNS

### Skill Testing Framework

```markdown
## TEST CATEGORIES

| Category | Purpose | When |
|----------|---------|------|
| Activation | Triggers correctly | After trigger change |
| Behavior | Correct output | After workflow change |
| Integration | Works with others | Before release |
| Performance | Token efficiency | Optimization phase |
```

### Test Cases Template

```markdown
## TEST: [Name]

**Input:** [User prompt]
**Expected:** [Output pattern]
**Actual:** [Run and capture]
**Status:** ✅ PASS / ❌ FAIL
```

---

## 12. VERSIONING PATTERNS

### Semantic Versioning for Skills

```
MAJOR.MINOR.PATCH

MAJOR: Breaking changes (workflow restructure)
MINOR: New features (new commands)
PATCH: Bug fixes (typos, small fixes)
```

### Upgrade Compatibility

```markdown
## UPGRADE RULES

| From | To | Breaking? | Migration |
|------|-----|-----------|-----------|
| 1.x | 2.x | Yes | Run /forge --upgrade |
| 1.1 | 1.2 | No | Automatic |
| 1.0.x | 1.0.y | No | Automatic |
```

---

## 13. CHECKLIST

```markdown
## Prompt Engineering Checklist

### Clarity
- [ ] All instructions are imperative
- [ ] No passive voice
- [ ] No ambiguous pronouns
- [ ] Decision logic is explicit

### Efficiency
- [ ] Using tables for structured data
- [ ] Progressive disclosure implemented
- [ ] No redundant content
- [ ] Lazy loading for references

### Reliability
- [ ] Numbered workflow steps
- [ ] Output templates defined
- [ ] Error handling specified
- [ ] Fallbacks documented

### Safety
- [ ] Input validation rules
- [ ] Output filtering rules
- [ ] Deny patterns configured
- [ ] No secret exposure

### Adaptability
- [ ] Edge cases covered
- [ ] Graceful degradation
- [ ] Context preservation
- [ ] Version compatibility
```

---

_SINGULARITY FORGE v19.0.0 - Prompt Engineering Patterns_
