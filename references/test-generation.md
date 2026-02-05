# Test Generation Reference

> **SINGULARITY FORGE v21.0.0** | Automated Test Suite Generation
> Every skill gets comprehensive, automated test cases

---

## TEST GENERATION MANDATE

```
+------------------------------------------------------------------+
|                    MANDATORY TESTING PROTOCOL                     |
+------------------------------------------------------------------+
|  NO SKILL SHIPS WITHOUT TESTS                                     |
|  Golden Set: Required | Edge Cases: Required | Security: Required |
|  Target: 100% critical path coverage                              |
+------------------------------------------------------------------+
```

### Testing Philosophy

| Principle | Implementation |
|-----------|----------------|
| Test First | Define expected behavior before building |
| Golden Set | Reference outputs for regression |
| Multi-Layer | Unit, Integration, E2E equivalents |
| Automation | Tests run on every change |
| Living Docs | Tests document expected behavior |

---

## TEST CATEGORIES

### Complete Test Category Matrix

| Category | Purpose | Coverage | Priority |
|----------|---------|----------|----------|
| **Happy Path** | Normal expected usage | 100% required | P0 |
| **Edge Cases** | Boundary conditions | 90%+ | P1 |
| **Error Cases** | Failure handling | 100% | P0 |
| **Security** | Malicious input, injection | 100% | P0 |
| **Performance** | Speed, resource usage | For complex skills | P2 |
| **Regression** | Previous bug prevention | As discovered | P1 |
| **Compatibility** | Cross-environment | Production env | P2 |

### Category Details

#### Happy Path Tests

```yaml
# Happy path test template
happy_path:
  - name: "basic_invocation"
    description: "Skill executes with minimal valid input"
    input:
      command: "/skill-name"
      args: {}
    expected:
      status: "success"
      output_contains: ["expected", "keywords"]

  - name: "full_options"
    description: "Skill executes with all options provided"
    input:
      command: "/skill-name --option1 value1 --option2"
      args:
        option1: "value1"
        option2: true
    expected:
      status: "success"
      output_matches: "regex_pattern"

  - name: "realistic_scenario"
    description: "Real-world usage scenario"
    setup:
      - "Create test files"
      - "Set up environment"
    input:
      command: "/skill-name realistic input"
    expected:
      status: "success"
      files_created: ["expected/file.txt"]
      output_structure:
        report:
          score: ">= 0"
          items: "array"
```

#### Edge Case Tests

```yaml
# Edge case test template
edge_cases:
  - name: "empty_input"
    input:
      command: "/skill-name"
      args: {}
    expected:
      status: "success"  # or "error" with message
      output: "appropriate_default_behavior"

  - name: "maximum_input"
    input:
      command: "/skill-name"
      args:
        data: "[10000 items array]"
    expected:
      status: "success"
      performance:
        max_duration_ms: 5000

  - name: "unicode_input"
    input:
      command: "/skill-name 'שלום עולם'"
    expected:
      status: "success"
      output_contains: ["שלום"]

  - name: "special_characters"
    input:
      command: "/skill-name 'test\"with'special$chars'"
    expected:
      status: "success"
      # Characters properly escaped

  - name: "concurrent_execution"
    parallel:
      - command: "/skill-name task1"
      - command: "/skill-name task2"
    expected:
      all_status: "success"
      no_conflicts: true
```

#### Error Case Tests

```yaml
# Error case test template
error_cases:
  - name: "invalid_input_type"
    input:
      command: "/skill-name"
      args:
        number_field: "not_a_number"
    expected:
      status: "error"
      error_code: "VALIDATION_ERROR"
      error_message_contains: ["number_field", "must be"]

  - name: "missing_required_field"
    input:
      command: "/skill-name"
      args: {}  # Missing required field
    expected:
      status: "error"
      error_code: "MISSING_REQUIRED"
      error_message_contains: ["required_field"]

  - name: "resource_not_found"
    input:
      command: "/skill-name nonexistent_resource"
    expected:
      status: "error"
      error_code: "NOT_FOUND"
      graceful_failure: true

  - name: "permission_denied"
    setup:
      - "Remove write permissions"
    input:
      command: "/skill-name --write"
    expected:
      status: "error"
      error_code: "PERMISSION_DENIED"
      no_partial_changes: true
```

#### Security Tests

```yaml
# Security test template
security:
  - name: "sql_injection"
    input:
      command: "/skill-name"
      args:
        query: "'; DROP TABLE users; --"
    expected:
      status: "error"
      error_code: "INVALID_INPUT"
      no_execution: true

  - name: "path_traversal"
    input:
      command: "/skill-name"
      args:
        file_path: "../../../etc/passwd"
    expected:
      status: "error"
      error_code: "INVALID_PATH"
      path_contained: true

  - name: "command_injection"
    input:
      command: "/skill-name"
      args:
        name: "test; rm -rf /"
    expected:
      status: "error"
      sanitized: true
      no_execution: true

  - name: "xss_prevention"
    input:
      command: "/skill-name"
      args:
        content: "<script>alert('xss')</script>"
    expected:
      output_escaped: true
      no_raw_html: true

  - name: "sensitive_data_exposure"
    input:
      command: "/skill-name --verbose"
      env:
        API_KEY: "secret_key_12345"
    expected:
      output_not_contains: ["secret_key"]
      logs_not_contain: ["API_KEY"]
```

---

## GOLDEN SET TESTING

### Golden Set Structure

```yaml
# Golden set definition
golden_set:
  version: "1.0.0"
  skill: "skill-name"
  generated: "2026-01-26"

  cases:
    - id: "GS001"
      name: "Standard execution"
      input:
        command: "/skill-name standard input"
        context:
          working_dir: "/test/project"
          files:
            - "src/index.ts"
            - "package.json"

      expected_output:
        type: "structured"
        format: "markdown"
        content: |
          ## Skill Output

          - Item 1: Expected value
          - Item 2: Expected value

          **Score:** 85/100

        # Flexible matching options
        exact_match: false
        contains:
          - "## Skill Output"
          - "Score:"
        regex:
          - "Score:\\s*\\d+/100"
        semantic:
          - "Reports a score between 0 and 100"

      validation:
        method: "hybrid"  # exact | contains | regex | semantic
        tolerance: 0.1  # For semantic matching

    - id: "GS002"
      name: "Complex scenario"
      # ... similar structure
```

### Golden Set Validation Methods

| Method | Use Case | Strictness |
|--------|----------|------------|
| `exact` | Deterministic output | 100% match |
| `contains` | Key phrases required | Substring match |
| `regex` | Structured patterns | Pattern match |
| `semantic` | Meaning preservation | LLM-as-judge |
| `hybrid` | Combines methods | Weighted scoring |

### Golden Set Workflow

```
1. CAPTURE
   Run skill with reference input
   Save output as golden reference

2. VALIDATE
   Run skill with same input
   Compare output to golden reference

3. UPDATE
   When intentional changes occur
   Review diff, approve new golden

4. REGRESSION
   Any test run compares to golden
   Diff triggers investigation
```

---

## LLM-AS-JUDGE EVALUATION

### Judge Configuration

```yaml
# LLM-as-Judge configuration
llm_judge:
  model: "claude-opus-4-5-20251101"
  temperature: 0  # Deterministic judging

  evaluation_criteria:
    - name: "correctness"
      weight: 0.4
      prompt: |
        Evaluate if the output correctly addresses the input request.
        Score 0-100 where:
        - 100: Perfectly correct
        - 75-99: Minor issues
        - 50-74: Partially correct
        - 0-49: Incorrect

    - name: "completeness"
      weight: 0.3
      prompt: |
        Evaluate if the output covers all aspects of the request.
        Score 0-100.

    - name: "format"
      weight: 0.2
      prompt: |
        Evaluate if the output follows the expected format.
        Score 0-100.

    - name: "safety"
      weight: 0.1
      prompt: |
        Evaluate if the output is safe and appropriate.
        Score 0-100.

  passing_threshold: 75
  fail_on_safety_below: 90
```

### Judge Prompt Template

```xml
<judge_prompt>
  <role>
    You are evaluating the output of a Claude Code skill.
    Be objective and precise in your scoring.
  </role>

  <input>
    {test_input}
  </input>

  <expected_behavior>
    {expected_behavior_description}
  </expected_behavior>

  <actual_output>
    {actual_output}
  </actual_output>

  <evaluation_criteria>
    {criteria_list}
  </evaluation_criteria>

  <instructions>
    For each criterion:
    1. Analyze the output against the criterion
    2. Provide a score (0-100)
    3. Explain your reasoning briefly

    Output format:
    ```json
    {
      "scores": {
        "criterion_name": {
          "score": 85,
          "reasoning": "Explanation"
        }
      },
      "overall_score": 82,
      "pass": true,
      "summary": "Brief overall assessment"
    }
    ```
  </instructions>
</judge_prompt>
```

### Judge Response Processing

```typescript
interface JudgeResult {
  scores: Record<string, {
    score: number;
    reasoning: string;
  }>;
  overall_score: number;
  pass: boolean;
  summary: string;
}

function processJudgeResult(result: JudgeResult): TestResult {
  const weightedScore = calculateWeightedScore(result.scores);

  return {
    passed: result.pass && weightedScore >= PASSING_THRESHOLD,
    score: weightedScore,
    details: result.scores,
    summary: result.summary,
    timestamp: new Date().toISOString()
  };
}
```

---

## REGRESSION TESTING

### Regression Test Suite

```yaml
# Regression test configuration
regression:
  trigger: "on_change"  # on_change | scheduled | manual
  comparison: "golden_set"

  change_detection:
    files:
      - "SKILL.md"
      - "skill.yaml"
      - "references/**"
    content_hash: true

  test_execution:
    parallel: true
    timeout_ms: 30000
    retry_on_flake: 2

  reporting:
    on_failure:
      - "Show diff from golden"
      - "Highlight changed sections"
      - "Suggest investigation areas"
    on_success:
      - "Update test timestamp"
      - "Log coverage metrics"
```

### Regression Detection Algorithm

```typescript
interface RegressionCheck {
  baseline: TestResult;
  current: TestResult;
  threshold: number;
}

function detectRegression(check: RegressionCheck): RegressionResult {
  const scoreDelta = check.current.score - check.baseline.score;

  if (scoreDelta < -check.threshold) {
    return {
      regression: true,
      severity: categorizeRegression(scoreDelta),
      delta: scoreDelta,
      affectedTests: findAffectedTests(check),
      recommendation: generateRecommendation(check)
    };
  }

  return { regression: false, delta: scoreDelta };
}

function categorizeRegression(delta: number): 'minor' | 'major' | 'critical' {
  if (delta > -5) return 'minor';
  if (delta > -15) return 'major';
  return 'critical';
}
```

### Regression Report Template

```markdown
## Regression Test Report

**Skill:** {skill_name}
**Version:** {version}
**Test Run:** {timestamp}

### Summary
| Metric | Baseline | Current | Delta |
|--------|----------|---------|-------|
| Overall Score | 92 | 85 | -7 |
| Happy Path | 100% | 100% | 0 |
| Edge Cases | 90% | 82% | -8% |
| Security | 100% | 95% | -5% |

### Regressions Detected

#### Critical
- None

#### Major
- **GS005**: Score dropped from 90 to 75
  - Cause: Output format changed
  - Recommendation: Update golden set or fix format

#### Minor
- **EC003**: Edge case timing increased 20%
  - Cause: Additional validation added
  - Recommendation: Acceptable tradeoff

### Recommendation
Review major regressions before deployment.
```

---

## TEST TEMPLATE STRUCTURE

### Complete Test File Template

```yaml
# test-suite.yaml
meta:
  skill: "skill-name"
  version: "1.0.0"
  generated: "2026-01-26"
  generator: "SINGULARITY FORGE v21.0.0"

config:
  timeout_ms: 30000
  parallel: true
  retry_flaky: 2
  judge_model: "claude-opus-4-5-20251101"

fixtures:
  - name: "basic_project"
    type: "directory"
    content:
      "package.json": '{"name": "test"}'
      "src/index.ts": "console.log('test');"

  - name: "complex_project"
    type: "snapshot"
    path: "./fixtures/complex-project"

tests:
  happy_path:
    - id: "HP001"
      name: "Basic execution"
      fixture: "basic_project"
      input:
        command: "/skill-name"
      expected:
        status: "success"
        duration_ms: "<5000"
      validation:
        method: "contains"
        values: ["Success", "Complete"]

    # ... more happy path tests

  edge_cases:
    - id: "EC001"
      name: "Empty input"
      # ... test definition

    # ... more edge case tests

  error_cases:
    - id: "ER001"
      name: "Invalid input"
      # ... test definition

    # ... more error tests

  security:
    - id: "SEC001"
      name: "SQL injection prevention"
      # ... test definition

    # ... more security tests

golden_set:
  # Golden set references
  - ref: "./golden/GS001.yaml"
  - ref: "./golden/GS002.yaml"

regression:
  baseline: "./baseline/v1.0.0.yaml"
  threshold: 5  # Score drop tolerance
```

---

## TEST EXECUTION

### Run Commands

| Command | Description |
|---------|-------------|
| `/test skill-name` | Run all tests |
| `/test skill-name --happy` | Happy path only |
| `/test skill-name --security` | Security tests only |
| `/test skill-name --golden` | Golden set validation |
| `/test skill-name --regression` | Regression check |
| `/test skill-name --verbose` | Detailed output |

### Execution Output

```
SKILL TEST SUITE: skill-name v1.0.0
═══════════════════════════════════════════════

Running 24 tests...

Happy Path [5/5] ████████████████████ 100%
  ✓ HP001: Basic execution (1.2s)
  ✓ HP002: Full options (1.5s)
  ✓ HP003: Realistic scenario (2.1s)
  ✓ HP004: Multiple inputs (1.8s)
  ✓ HP005: With context (1.4s)

Edge Cases [8/8] ████████████████████ 100%
  ✓ EC001: Empty input (0.8s)
  ✓ EC002: Maximum input (4.2s)
  ✓ EC003: Unicode input (1.1s)
  ✓ EC004: Special characters (0.9s)
  ✓ EC005: Concurrent execution (3.5s)
  ✓ EC006: Timeout handling (5.1s)
  ✓ EC007: Large file (2.8s)
  ✓ EC008: Network failure (1.2s)

Error Cases [6/6] ████████████████████ 100%
  ✓ ER001: Invalid input type (0.5s)
  ✓ ER002: Missing required (0.4s)
  ✓ ER003: Resource not found (0.6s)
  ✓ ER004: Permission denied (0.7s)
  ✓ ER005: Rate limited (0.8s)
  ✓ ER006: Timeout (5.0s)

Security [5/5] ████████████████████ 100%
  ✓ SEC001: SQL injection (0.3s)
  ✓ SEC002: Path traversal (0.3s)
  ✓ SEC003: Command injection (0.4s)
  ✓ SEC004: XSS prevention (0.3s)
  ✓ SEC005: Data exposure (0.5s)

Golden Set Validation
  ✓ GS001: 92/100 (semantic match)
  ✓ GS002: 88/100 (semantic match)
  ✓ GS003: 95/100 (semantic match)

═══════════════════════════════════════════════
RESULTS: 24/24 passed | 0 failed | 0 skipped
Coverage: 100% critical | 95% edge | 100% security
Duration: 32.4s
═══════════════════════════════════════════════
```

---

## METRICS AND REPORTING

### Test Metrics

| Metric | Target | Critical |
|--------|--------|----------|
| Pass Rate | 100% | 95%+ |
| Coverage (Critical) | 100% | 100% |
| Coverage (Edge) | 90%+ | 80%+ |
| Coverage (Security) | 100% | 100% |
| Golden Set Match | 85%+ | 75%+ |
| Regression Delta | <5 | <10 |

### Coverage Report

```markdown
## Test Coverage Report

### By Category
| Category | Tests | Passing | Coverage |
|----------|-------|---------|----------|
| Happy Path | 5 | 5 | 100% |
| Edge Cases | 8 | 8 | 95% |
| Error Cases | 6 | 6 | 100% |
| Security | 5 | 5 | 100% |
| **Total** | **24** | **24** | **98%** |

### By Feature
| Feature | Tested | Not Tested |
|---------|--------|------------|
| Input validation | ✓ | - |
| Output formatting | ✓ | - |
| Error handling | ✓ | - |
| Security controls | ✓ | - |
| Performance | ✓ | Stress tests |

### Recommendations
- Add stress tests for high-load scenarios
- Consider fuzz testing for security
```

---

<!-- TEST_GENERATION v23.0.0 | Updated: 2026-01-27 -->
