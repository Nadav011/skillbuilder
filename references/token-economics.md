# Token Economics & Efficiency

## Progressive Disclosure Strategy

Claude Code loads skills in layers. Optimize for lazy loading.

### 3-Layer Architecture

| Layer | File | Token Cost | When Loaded |
|-------|------|------------|-------------|
| **L1** | skill.yaml | ~100 tokens | Always (skill registry scan) |
| **L2** | SKILL.md | <5000 tokens | On trigger activation |
| **L3** | references/*.md | Variable | On-demand (when referenced) |

**Design Principle:** Front-load critical info in L1/L2, defer details to L3.

---

## Token Efficiency Rules

### Rule 1: Tables Over Prose (30-40% savings)

**Bad (218 tokens):**
```markdown
When you need to validate user input, you should use Zod for schema validation.
For error handling, the Result pattern is recommended. If you're working with
authentication, make sure to check the user's session. For database queries,
always use parameterized queries to prevent SQL injection.
```

**Good (127 tokens):**
```markdown
| Task | Tool |
|------|------|
| Input validation | Zod |
| Error handling | Result<T,E> |
| Auth check | validateSession() |
| DB queries | Parameterized |
```

**Savings: 42%**

### Rule 2: No Redundancy (20-30% savings)

**Bad:**
```markdown
## Triggers
The skill triggers on these phrases:
- "create skill"
- "build skill"

## When to Use
Use this skill when you want to create a new skill or build a new skill.
```

**Good:**
```markdown
## Triggers
- create skill
- build skill
```

**Avoid:**
- Repeating trigger phrases in description
- Saying "you should" or "you need to" (implied)
- Explaining what's obvious from context

### Rule 3: Imperative Form (10-20% savings)

**Bad (verbose):**
```markdown
You should validate the input before processing it. Then you need to
transform the data. After that, you should return the result.
```

**Good (imperative):**
```markdown
1. Validate input
2. Transform data
3. Return result
```

**Technique:**
- Use verbs directly (Validate, Transform, Return)
- Omit subjects (you, the system, etc.)
- Remove filler words (should, need to, make sure to)

### Rule 4: Lazy Loading (50-70% savings)

**Bad (all in SKILL.md):**
```markdown
## API Reference
[500 lines of API documentation]

## Examples
[300 lines of examples]

## Troubleshooting
[200 lines of FAQs]
```

**Good (split to references/):**
```markdown
## API Reference
See references/api-reference.md

## Examples
See references/examples.md

## Troubleshooting
See references/troubleshooting.md
```

**Result:** SKILL.md loads instantly, references load only when needed.

---

## Token ROI Scoring

Calculate efficiency score for your skill:

```
Score = (Functionality / Token Cost) × 100

Where:
- Functionality = # of distinct capabilities
- Token Cost = SKILL.md token count
```

**Benchmarks:**

| Score | Rating | Example |
|-------|--------|---------|
| > 1.0 | GOD-TIER | singularity-forge (8 capabilities / 5000 tokens = 1.6) |
| 0.5-1.0 | Excellent | Most standard skills |
| 0.2-0.5 | Good | Complex skills with many examples |
| < 0.2 | Needs optimization | Bloated, redundant content |

**Optimization Tactics:**

1. **Extract examples** → references/examples.md
2. **Condense checklists** → Use tables
3. **Remove redundancy** → Delete repeated explanations
4. **Split large sections** → Create reference files
5. **Use diagrams** → ASCII art more efficient than prose

---

## Content Compression Techniques

### Technique 1: Decision Trees → Tables

**Before (120 tokens):**
```markdown
If the user wants a simple skill with less than 200 lines, create a SKILL.md file.
If they need a standard skill with 200-400 lines, create SKILL.md and references/.
For complex skills over 400 lines, add scripts/ directory too.
```

**After (45 tokens):**
```markdown
| Lines | Structure |
|-------|-----------|
| <200 | SKILL.md only |
| 200-400 | + references/ |
| 400-500 | + scripts/ |
```

**Savings: 62%**

### Technique 2: Bullet Lists → Inline

**Before (80 tokens):**
```markdown
Prerequisites:
- Node.js installed
- Git configured
- TypeScript knowledge
- Zod understanding
```

**After (35 tokens):**
```markdown
Prerequisites: Node.js, Git, TypeScript, Zod
```

**Savings: 56%**

### Technique 3: Commands → Table

**Before (150 tokens):**
```markdown
Use /forge [description] to build a new skill from a description.
Use /forge --validate [path] to validate an existing skill.
Use /forge --upgrade [path] to upgrade a skill to the latest version.
```

**After (60 tokens):**
```markdown
| Command | Purpose |
|---------|---------|
| /forge [desc] | Build new skill |
| /forge --validate [path] | Validate skill |
| /forge --upgrade [path] | Upgrade skill |
```

**Savings: 60%**

---

## Measurement Tools

### Token Counter
```bash
# Count tokens in SKILL.md (rough estimate: 1 token ≈ 4 chars)
wc -c SKILL.md | awk '{print $1/4}'
```

### Before/After Comparison
```bash
# Original
wc -l skill-before.md  # 800 lines
# Optimized
wc -l SKILL.md  # 350 lines
wc -l references/*.md | tail -1  # 450 lines
# Result: 56% loaded upfront, 44% lazy-loaded
```

---

## Progressive Disclosure Pattern

**Layer 1 (skill.yaml):** Just enough to identify and trigger
- Name, version, description (< 1024 chars)
- Triggers, commands
- **No** implementation details

**Layer 2 (SKILL.md):** Core workflow and primary patterns
- Role, capabilities
- Structured Chain-of-Thought
- Primary commands with brief examples
- References to L3 for details

**Layer 3 (references/):** Deep dives and edge cases
- Comprehensive examples
- API documentation
- Troubleshooting guides
- Advanced patterns

**Golden Rule:** If it's not needed for 80% of use cases, push to L3.

---

## Anti-Patterns (Token Waste)

| Anti-Pattern | Why Bad | Fix |
|--------------|---------|-----|
| Repeating skill.yaml in SKILL.md | Duplicate content | Reference, don't repeat |
| Long prose explanations | High token cost | Use tables/lists |
| Inline large examples | Bloats core file | Move to references/examples.md |
| Explaining obvious things | Wasted tokens | Assume competent user |
| Passive voice | Wordy | Use imperative form |

---

## Efficiency Checklist

Before finalizing any skill:

- [ ] skill.yaml < 150 lines
- [ ] SKILL.md < 500 lines (ideally < 400)
- [ ] Tables used instead of prose where possible
- [ ] No duplicate content between files
- [ ] Examples moved to references/
- [ ] Imperative form used throughout
- [ ] Token ROI score > 0.5
- [ ] L3 files load on-demand only

**Target:** 70% of token budget in SKILL.md, 30% in references/ (lazy-loaded).
