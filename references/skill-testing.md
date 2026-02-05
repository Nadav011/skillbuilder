# Skill Testing Reference

> Testing framework for validating skill activation, behavior, and performance.

## Table of Contents

1. [Testing Overview](#testing-overview)
2. [Activation Testing](#activation-testing)
3. [Trigger Matching Tests](#trigger-matching-tests)
4. [Output Validation](#output-validation)
5. [Performance Profiling](#performance-profiling)
6. [Test Templates](#test-templates)

---

## Testing Overview

### Test Categories

| Category | Purpose | When to Run |
|----------|---------|-------------|
| Activation | Skill triggers correctly | After trigger changes |
| Behavior | Produces correct output | After workflow changes |
| Integration | Works with other skills | Before release |
| Performance | Token/time efficiency | Optimization phase |

### Test Execution Flow

```
1. ACTIVATION TESTS
   └── Does skill activate on triggers?

2. INPUT VALIDATION TESTS
   └── Does skill handle all input types?

3. WORKFLOW TESTS
   └── Does skill follow documented steps?

4. OUTPUT TESTS
   └── Does output match expected format?

5. PERFORMANCE TESTS
   └── Is execution within limits?
```

---

## Activation Testing

### Manual Activation Test

1. Open Claude Code in test project
2. Type each trigger phrase
3. Verify skill activates (not another skill)
4. Document any mis-activations

### Trigger Test Matrix

| Trigger Phrase | Expected Skill | Actual | Pass/Fail |
|----------------|----------------|--------|-----------|
| "build skill" | singularity-forge | - | - |
| "create skill" | singularity-forge | - | - |
| "forge skill" | singularity-forge | - | - |

### Negative Tests (Should NOT Activate)

| Phrase | Should NOT Trigger |
|--------|-------------------|
| "build component" | singularity-forge |
| "skill training" | singularity-forge |
| "forge ahead" | singularity-forge |

### Conflict Detection

Check for trigger overlaps with other skills:

```bash
# Find all triggers across skills
grep -rh "^  - " ~/.claude/skills/*/skill.yaml | sort | uniq -d
```

---

## Trigger Matching Tests

### Test Cases for Trigger Specificity

| Test | Input | Expected | Rationale |
|------|-------|----------|-----------|
| Exact match | "build skill" | Activate | Primary trigger |
| Variation | "build a skill" | Activate | Common variation |
| Partial | "skill" alone | No activate | Too generic |
| Similar | "build component" | No activate | Different intent |
| Typo | "buld skill" | ? | Depends on Claude |

### Trigger Quality Checklist

```
[ ] Trigger is 2+ words (not too generic)
[ ] Trigger is domain-specific
[ ] Trigger doesn't overlap with common phrases
[ ] Trigger variations are covered
[ ] Trigger intent is clear
```

### Trigger Improvement Process

1. Document all mis-activations for 1 week
2. Analyze patterns
3. Add/modify triggers
4. Re-test trigger matrix
5. Document in skill.yaml

---

## Output Validation

### Expected Output Checklist

```
[ ] Output matches documented format
[ ] All required sections present
[ ] Code blocks have language tags
[ ] Tables are properly formatted
[ ] No truncation in output
[ ] References resolve correctly
```

### Output Format Tests

| Test | Input | Expected Output |
|------|-------|-----------------|
| Basic | Minimal input | Standard response |
| Complex | Full options | Complete response |
| Error | Invalid input | Error message |
| Edge | Empty/null | Graceful handling |

### Validation Script

```bash
#!/bin/bash
# Test skill output structure

SKILL_OUTPUT="$1"

# Check for required sections
REQUIRED_SECTIONS=(
  "## PURPOSE"
  "## WORKFLOW"
)

for section in "${REQUIRED_SECTIONS[@]}"; do
  if ! grep -q "$section" "$SKILL_OUTPUT"; then
    echo "FAIL: Missing $section"
    exit 1
  fi
done

echo "PASS: All required sections present"
```

---

## Performance Profiling

### Token Efficiency Metrics

| Metric | Target | Measurement |
|--------|--------|-------------|
| SKILL.md tokens | <5000 | Count via tiktoken |
| Response tokens | <2000 | Measure output |
| Context usage | <20% | Progressive loading |

### Measuring Token Usage

```python
# Token counting (approximate)
def count_tokens(text):
    # Rough estimate: 1 token per 4 characters
    return len(text) / 4

# Or use tiktoken for accuracy
import tiktoken
enc = tiktoken.encoding_for_model("gpt-4")
tokens = len(enc.encode(text))
```

### Latency Profiling

| Phase | Target | Actual |
|-------|--------|--------|
| Skill loading | <100ms | - |
| Input parsing | <50ms | - |
| Core execution | <5s | - |
| Output generation | <500ms | - |

### Performance Test Matrix

| Test | Input Size | Expected Time | Pass Criteria |
|------|------------|---------------|---------------|
| Small | 10 items | <1s | Instant feel |
| Medium | 100 items | <5s | Acceptable |
| Large | 1000 items | <30s | With progress |
| Huge | 10000 items | Batch | Must batch |

---

## Test Templates

### Activation Test Template

```markdown
# Activation Test: [Skill Name]

## Test Date: YYYY-MM-DD

## Triggers Tested

| Trigger | Activated? | Correct Skill? | Notes |
|---------|------------|----------------|-------|
| "trigger 1" | Yes/No | Yes/No | |
| "trigger 2" | Yes/No | Yes/No | |

## Conflicts Found

| Trigger | Conflicting Skill |
|---------|-------------------|
| | |

## Result: PASS / FAIL
```

### Behavior Test Template

```markdown
# Behavior Test: [Skill Name]

## Test Date: YYYY-MM-DD

## Test Cases

### Test 1: [Name]

**Input:**
```
input here
```

**Expected:**
```
expected output
```

**Actual:**
```
actual output
```

**Result:** PASS / FAIL

### Test 2: [Name]
...

## Summary

| Test | Result |
|------|--------|
| Test 1 | PASS/FAIL |
| Test 2 | PASS/FAIL |

## Overall: PASS / FAIL
```

### Performance Test Template

```markdown
# Performance Test: [Skill Name]

## Test Date: YYYY-MM-DD
## Environment: [Claude Code version]

## Token Usage

| File | Tokens | Target | Status |
|------|--------|--------|--------|
| skill.yaml | | <500 | |
| SKILL.md | | <5000 | |
| Total | | <6000 | |

## Latency

| Operation | Time | Target | Status |
|-----------|------|--------|--------|
| Load | | <100ms | |
| Parse | | <50ms | |
| Execute | | <5s | |

## Result: PASS / FAIL
```

---

## Continuous Testing

### Pre-Release Checklist

```
[ ] All activation tests pass
[ ] All behavior tests pass
[ ] Performance within targets
[ ] No trigger conflicts
[ ] Documentation matches behavior
[ ] 70-gate validation passes
```

### Test Automation

```bash
#!/bin/bash
# run-skill-tests.sh

SKILL_PATH="$1"

echo "Running Skill Tests for: $SKILL_PATH"

# 1. Structure validation
echo "=== Structure Validation ==="
bash ~/.claude/skills/singularity-forge/scripts/validate-skill.sh "$SKILL_PATH"

# 2. YAML validation
echo "=== YAML Validation ==="
python3 -c "import yaml; yaml.safe_load(open('$SKILL_PATH/skill.yaml'))" && echo "PASS" || echo "FAIL"

# 3. Line count check
echo "=== Line Count ==="
LINES=$(wc -l < "$SKILL_PATH/SKILL.md")
[ "$LINES" -lt 500 ] && echo "PASS: $LINES lines" || echo "FAIL: $LINES lines (>500)"

# 4. Anti-pattern check
echo "=== Anti-Pattern Check ==="
[ -f "$SKILL_PATH/README.md" ] && echo "FAIL: README.md exists" || echo "PASS: No README.md"
[ -f "$SKILL_PATH/CHANGELOG.md" ] && echo "FAIL: CHANGELOG.md exists" || echo "PASS: No CHANGELOG.md"

echo "=== Tests Complete ==="
```

---

## Debugging Failed Tests

### Common Failures

| Failure | Cause | Solution |
|---------|-------|----------|
| Skill doesn't activate | Triggers too specific | Add variations |
| Wrong skill activates | Trigger overlap | Make more specific |
| Output truncated | Too much content | Use progressive disclosure |
| Slow execution | Too much processing | Batch or optimize |
| Invalid format | Missing structure | Follow templates |

### Debug Process

1. **Reproduce** - Get exact input that fails
2. **Isolate** - Test with minimal input
3. **Trace** - Follow workflow step by step
4. **Fix** - Update skill
5. **Verify** - Re-run full test suite
6. **Document** - Update test cases

---

<!-- SKILL_TESTING v23.0.0 | Updated: 2026-01-27 -->
