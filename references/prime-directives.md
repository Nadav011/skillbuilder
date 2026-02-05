# Prime Directives

## The 9 Immutable Laws of Skill Creation

These directives override all other considerations. NEVER violate them.

---

## Directive 1: READ MASTER_COORDINATE.md FIRST

**Before generating ANY skill content:**

```bash
Read ~/.claude/MASTER_COORDINATE.md
```

**Why:** MASTER_COORDINATE defines:
- 6 APEX Laws (Zero Trust, N+1, Immortality, ROI, RTL, Responsive)
- 70-Gate validation matrix
- 9 Dimensions framework
- Token efficiency standards
- Security protocols

**Consequence of violation:** Non-compliant skills that fail validation.

**When to read:**
- Start of every skill generation
- During skill upgrades
- When uncertain about standards

---

## Directive 2: Never Exceed 500 Lines in SKILL.md

**Rule:** SKILL.md MUST be < 500 lines. No exceptions.

**If content exceeds limit:**

1. **Extract to references/:**
   - Detailed examples → references/examples.md
   - API docs → references/api-reference.md
   - Troubleshooting → references/troubleshooting.md
   - Patterns → references/patterns.md

2. **Compress using tables:**
   - Convert prose to tables (30-40% savings)
   - Use bullet lists, not paragraphs
   - Remove redundant explanations

3. **Use progressive disclosure:**
   - Core workflow in SKILL.md
   - Deep dives in references/
   - Load references on-demand

**Verification:**
```bash
wc -l SKILL.md
# Must output < 500
```

**Why:** Token efficiency, fast loading, cognitive load management.

---

## Directive 3: Enforce RTL (Law #5)

**MANDATORY for all skills that generate UI code:**

### Use ONLY:
- `ms-*` (margin-inline-start)
- `me-*` (margin-inline-end)
- `ps-*` (padding-inline-start)
- `pe-*` (padding-inline-end)
- `start-*` (inset-inline-start)
- `end-*` (inset-inline-end)
- `text-start` / `text-end`

### NEVER use:
- `ml-*` (margin-left)
- `mr-*` (margin-right)
- `pl-*` (padding-left)
- `pr-*` (padding-right)
- `left-*` (left)
- `right-*` (right)
- `text-left` / `text-right`

**Validation pattern:**
```bash
# Fail if SKILL.md contains RTL violations
grep -E "(ml-|mr-|pl-|pr-|left-|right-|text-left|text-right)" SKILL.md
# Must return no matches
```

**Why:** Hebrew/Arabic support (Israel/Middle East markets).

---

## Directive 4: Enforce Responsive (Law #6)

**MANDATORY for all skills that generate UI code:**

### Mobile-First Rules:
- Design for mobile (< 425px) first
- Scale up to tablet (425-768px)
- Then desktop (> 768px)

### Touch Target Minimum:
- Buttons: `min-h-11 min-w-11` (44x44px)
- Links: `min-h-11 py-2`
- Icons (tappable): `h-11 w-11`
- Form inputs: `h-11`

### Spacing (8pt grid):
- Use only: 1, 2, 3, 4, 6, 8, 12, 16 (Tailwind)
- Pixels: 4, 8, 12, 16, 24, 32, 48, 64
- NO arbitrary values like `m-[13px]`

**Why:** 60%+ mobile traffic, accessibility standards.

---

## Directive 5: Use Tables Over Prose

**Rule:** Whenever presenting structured information, use tables.

**Savings:** 30-40% token reduction.

---

## Directive 6: Include Security Deny Patterns

**Every skill.yaml MUST include:**

```yaml
security:
  deny:
    - ".env"
    - ".env.*"
    - "secrets/**"
    - "*.pem"
    - "*.key"
    - "**/api_keys*"
```

**Why:** Prevents accidental credential exposure.

---

## Directive 7: Verify 52/70 Gates Pass

**Before marking skill complete:**

```bash
# Run validation script
bash ~/.claude/skills/singularity-forge/scripts/validate-skill.sh <skill-path>
```

**If any gate fails:** Refactor and re-validate. Never skip.

---

## Directive 8: Apply Token ROI Optimization

**Calculate Token ROI:**

```
Token ROI = (# of distinct capabilities) / (SKILL.md token count) × 100
```

**Target:** > 0.5 (GOD-TIER: > 1.0)

**Why:** Faster loading, lower latency, better context management.

---

## Directive 9: ASK if Intent Ambiguous

**If user request is unclear:**

1. **STOP immediately**
2. **ASK clarifying questions:**
   - What's the primary goal?
   - What type of skill? (Simple/Standard/Complex/Command)
   - What tools are needed?
   - What constraints apply?

**Never guess.** Better to ask than generate wrong skill.

---

## Prime Directive Summary (Quick Reference)

| # | Directive | Check |
|---|-----------|-------|
| 1 | Read MASTER_COORDINATE.md first | `Read ~/.claude/MASTER_COORDINATE.md` |
| 2 | SKILL.md < 500 lines | `wc -l SKILL.md` |
| 3 | Enforce RTL (Law #5) | No ml-/mr-/left-/right- |
| 4 | Enforce Responsive (Law #6) | 44x44px touch, mobile-first |
| 5 | Tables over prose | 30-40% token savings |
| 6 | Security deny patterns | `.env`, `*.key`, `secrets/**` |
| 7 | 52/70 gates pass | Run validate-skill.sh |
| 8 | Token ROI > 0.5 | Calculate: capabilities / tokens |
| 9 | ASK if ambiguous | Never guess, always clarify |

**Enforcement:** ALL directives are MANDATORY. Violations result in skill rejection.
