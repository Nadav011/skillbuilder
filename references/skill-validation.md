# Skill Validation Reference

> 70-Gate validation checklist and token efficiency scoring.

## Table of Contents

1. [Quick Validation](#quick-validation)
2. [70-Gate Checklist](#70-gate-checklist)
3. [Line Count Rules](#line-count-rules)
4. [Token Efficiency Scoring](#token-efficiency-scoring)
5. [Common Violations](#common-violations)

---

## Quick Validation

Run the validation script:

```bash
bash ~/.claude/skills/singularity-forge/scripts/validate-skill.sh [path]
```

### Manual Quick Check

```
[ ] skill.yaml exists and parses as valid YAML
[ ] SKILL.md exists and under 500 lines
[ ] Description follows WHAT + WHEN + TRIGGERS format
[ ] No README.md or CHANGELOG.md present
[ ] All referenced files exist
[ ] Triggers are specific (not generic)
```

---

## 70-Gate Checklist

### Cluster 1: GENESIS_IDENTITY (Gates 1-7)

| Gate | Check | Pass Criteria |
|------|-------|---------------|
| G1 | Intent alignment | Skill matches stated purpose |
| G2 | Version specified | Semantic versioning in yaml |
| G3 | Role explicit | Clear purpose statement |
| G4 | Name unique | kebab-case, no conflicts |
| G5 | Dependencies mapped | All deps documented |
| G6 | Paths valid | No broken file references |
| G7 | Metadata complete | name + description present |

**Validation Commands:**
```bash
# G4: Check name format
grep "^name:" skill.yaml | grep -E "^name: [a-z0-9-]+$"

# G6: Check all referenced paths exist
grep -oE "references/[a-z-]+\.md" SKILL.md | while read f; do test -f "$f" || echo "Missing: $f"; done
```

### Cluster 2: LAW_ALIGNMENT (Gates 8-14)

| Gate | Check | Pass Criteria |
|------|-------|---------------|
| G8 | Zero Trust | Inputs validated/documented |
| G9 | N+1 elimination | No redundant operations |
| G10 | Type sovereignty | Explicit types, no any |
| G11 | Token ROI | No filler content |
| G12 | Law precedence | Laws followed in order |
| G13 | Hard limits | SKILL.md < 500 lines |
| G14 | Safety | No injection vectors |

**Validation Commands:**
```bash
# G13: Check line count
wc -l SKILL.md  # Must be < 500

# G11: Check for filler phrases
grep -c "This skill will help you\|designed to\|In order to" SKILL.md  # Should be 0
```

### Cluster 3: COGNITIVE_STABILITY (Gates 15-21)

| Gate | Check | Pass Criteria |
|------|-------|---------------|
| G15 | Command precision | Unambiguous instructions |
| G16 | Prompt enclosure | Proper XML/Markdown structure |
| G17 | State consistency | Stateless operations |
| G18 | Async handling | Proper timeout/error handling |
| G19 | Error granularity | Specific error messages |
| G20 | Edge cases | Empty, null, overflow covered |
| G21 | Idempotency | Same input = same output |

**Validation Commands:**
```bash
# G15: Check for numbered steps or clear structure
grep -cE "^[0-9]+\.|^##|^\*\*" SKILL.md  # Should be > 0

# G16: Check tag symmetry
OPEN=$(grep -oE "<[a-z_]+>" SKILL.md | wc -l)
CLOSE=$(grep -oE "</[a-z_]+>" SKILL.md | wc -l)
[ "$OPEN" -eq "$CLOSE" ] && echo "PASS" || echo "FAIL"
```

### Cluster 4: XML_SEMANTICS (Gates 22-28)

| Gate | Check | Pass Criteria |
|------|-------|---------------|
| G22 | Tag symmetry | All tags properly closed |
| G23 | Marker precision | Semantic tag names |
| G24 | Context isolation | Logic blocks independent |
| G25 | Logic compression | Maximum density |
| G26 | Prompt leak shield | Encapsulated logic |
| G27 | Structure depth | Max 3 levels nested |
| G28 | Semantic index | Clear headings/TOC |

**Validation Commands:**
```bash
# G27: Check nesting depth
grep -E "^#{4,}" SKILL.md  # Should return nothing (max ### allowed)

# G28: Check for headers
grep -c "^##" SKILL.md  # Should be > 2
```

### Cluster 5: EXORCIST_AUDIT (Gates 29-35)

| Gate | Check | Pass Criteria |
|------|-------|---------------|
| G29 | Ghost logic | No dead code paths |
| G30 | Side effect purge | No unintended effects |
| G31 | Bias removal | No hardcoded assumptions |
| G32 | Logic trap | Guaranteed termination |
| G33 | Efficiency sweep | Token usage optimized |
| G34 | Redundancy purge | No repetition (DRY) |
| G35 | Transcendence | Quality exceeds baseline |

**Validation Commands:**
```bash
# G34: Check for TODO markers
grep -ciE "TODO|FIXME|XXX|HACK" SKILL.md  # Should be 0

# G34: Check for README (should not exist)
test -f README.md && echo "FAIL: README.md exists" || echo "PASS"
```

### Cluster 6: SENSATION_UX (Gates 36-42)

| Gate | Check | Pass Criteria |
|------|-------|---------------|
| G36 | Output fluidity | Smooth reading flow |
| G37 | Actionable insights | Clear next steps |
| G38 | Visual hierarchy | Headers, bullets, tables |
| G39 | Feedback loop | Progress indicators |
| G40 | Instruction clarity | Imperative form |
| G41 | UX flow | Logical progression |
| G42 | Syntax seal | Valid Markdown |

**Validation Commands:**
```bash
# G38: Check for tables
grep -c "^|" SKILL.md  # Should be > 0 for structured data

# G40: Check for imperative form (should NOT have passive)
grep -ciE "should be|will be|is designed to" SKILL.md  # Minimize
```

### Cluster 7: NEURAL_PERFORMANCE (Gates 43-49)

| Gate | Check | Pass Criteria |
|------|-------|---------------|
| G43 | Token efficiency | High info density |
| G44 | Reasoning depth | Match task complexity |
| G45 | Context window ROI | Progressive disclosure |
| G46 | Latency reduction | No unnecessary steps |
| G47 | Logic pruning | Minimal conditionals |
| G48 | Memory optimization | Minimal state |
| G49 | Singularity sync | 48/48 previous gates |

### Cluster 8: SCALE (Gates 50-52)

| Gate | Check | Pass Criteria |
|------|-------|---------------|
| G50 | Spacing scale | 8pt grid (if UI) |
| G51 | Typography scale | Responsive sizes (if UI) |
| G52 | Component sizes | 44x44px touch targets (if UI) |

**Validation Commands:**
```bash
# G50-52: Check for RTL violations in UI skills
grep -ciE "ml-|mr-|pl-|pr-|left-|right-" SKILL.md  # Should be 0
```

---

## Line Count Rules

| File | Max Lines | Action if Exceeded |
|------|-----------|-------------------|
| skill.yaml | 150 | Simplify or split config |
| SKILL.md | 500 | Move to references/ |
| references/*.md | 300 | Split into sub-files |
| scripts/*.sh | 200 | Modularize |

### Counting Lines

```bash
# Count all skill files
find . -name "*.md" -o -name "*.yaml" | xargs wc -l

# Check SKILL.md specifically
wc -l SKILL.md
```

---

## Token Efficiency Scoring

### Scoring Formula

```
Score = (Information Density) × (Structure Bonus) × (No Redundancy)
```

### Information Density

| Pattern | Score | Example |
|---------|-------|---------|
| Table with data | +10 | Command reference table |
| Numbered steps | +5 | Workflow steps |
| Code block | +5 | Example code |
| Prose paragraph | -5 | Explanatory text |
| Filler phrase | -10 | "This skill is designed to..." |

### Structure Bonus

| Structure | Multiplier |
|-----------|------------|
| SKILL.md only | 1.0x |
| SKILL.md + references/ | 1.2x |
| Progressive disclosure | 1.5x |

### Redundancy Penalty

| Issue | Penalty |
|-------|---------|
| Duplicate content | -20% |
| README.md present | -15% |
| CHANGELOG.md present | -10% |
| Repeated instructions | -5% each |

### Target Scores

| Rating | Score | Description |
|--------|-------|-------------|
| GOD-TIER | 90+ | Perfect efficiency |
| Excellent | 75-89 | Production ready |
| Good | 60-74 | Minor improvements needed |
| Needs Work | <60 | Significant refactoring |

---

## Common Violations

### Critical (Must Fix)

| Violation | Problem | Fix |
|-----------|---------|-----|
| SKILL.md > 500 lines | Context bloat | Split to references/ |
| Missing description | Can't trigger | Add WHAT + WHEN + TRIGGERS |
| README.md exists | Redundant | Delete it |
| Invalid YAML | Won't parse | Fix syntax |
| Broken references | Missing files | Create or remove reference |

### Warning (Should Fix)

| Violation | Problem | Fix |
|-----------|---------|-----|
| Passive voice | Ambiguous | Use imperative |
| Prose paragraphs | Low density | Convert to tables |
| Generic triggers | Mis-activation | Make specific |
| No error handling | Incomplete | Add error table |
| Deep nesting | Hard to read | Flatten structure |

### Info (Consider)

| Violation | Problem | Suggestion |
|-----------|---------|------------|
| Few tables | Could be denser | Add structured data |
| No examples | Harder to understand | Add to references/ |
| Long descriptions | Token cost | Condense |

---

## Validation Report Format

```
═══════════════════════════════════════════════════
  SINGULARITY FORGE - SKILL VALIDATION REPORT
═══════════════════════════════════════════════════

Skill: [skill-name]
Path: [path]
Type: [Simple/Standard/Complex/Command]

━━━ CLUSTER 1: GENESIS ━━━
✅ Gate 1: Intent alignment
✅ Gate 2: Version specified
✅ Gate 3: Role explicit
...

━━━ SUMMARY ━━━
Gates Passed: 70/70
Token Efficiency: 87/100
Rating: EXCELLENT

Warnings: 2
- Consider adding examples to references/
- Description could be more specific

═══════════════════════════════════════════════════
  RESULT: CERTIFIED ✅
═══════════════════════════════════════════════════
```

---

<!-- SKILL_VALIDATION v23.0.0 | Updated: 2026-01-27 -->
