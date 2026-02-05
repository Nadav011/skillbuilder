# Skill Composition Reference

> **SINGULARITY FORGE v21.0.0** | Composite Skill Architecture
> Combine multiple skills into powerful workflows

---

## COMPOSITION OVERVIEW

```
+------------------------------------------------------------------+
|                    SKILL COMPOSITION ENGINE                       |
+------------------------------------------------------------------+
|  Build complex workflows from atomic skills                       |
|  Patterns: Sequential, Parallel, Conditional, Hybrid              |
|  Benefits: Reusability, maintainability, testability              |
+------------------------------------------------------------------+
```

### Why Compose Skills?

| Benefit | Description |
|---------|-------------|
| DRY | Don't repeat common patterns |
| Modularity | Swap components easily |
| Testing | Test units independently |
| Scaling | Add capabilities incrementally |
| Maintenance | Update once, benefit everywhere |

---

## COMPOSITION PATTERNS

### Pattern 1: Sequential Composition

```
[Skill A] → [Skill B] → [Skill C] → [Output]
```

**Use When**: Steps must execute in order, each depends on previous.

```yaml
# Sequential composition definition
composition:
  name: "deploy-full"
  type: sequential
  skills:
    - skill: "lint-check"
      output_map:
        lint_results: "$.results"

    - skill: "test-run"
      requires: "lint_results.passed == true"
      output_map:
        test_results: "$.coverage"

    - skill: "build-app"
      requires: "test_results.coverage >= 80"
      output_map:
        build_artifact: "$.artifact_path"

    - skill: "deploy-vercel"
      input_map:
        artifact: "build_artifact"
      output_map:
        deploy_url: "$.url"

  final_output:
    - deploy_url
    - test_results.coverage
```

**Execution Flow**:

```
START
  │
  ├─► [lint-check]
  │      │
  │      ├─ PASS ─► [test-run]
  │      │              │
  │      │              ├─ PASS ─► [build-app]
  │      │              │              │
  │      │              │              └─► [deploy-vercel] ─► SUCCESS
  │      │              │
  │      │              └─ FAIL ─► ABORT (low coverage)
  │      │
  │      └─ FAIL ─► ABORT (lint errors)
  │
  └─► END
```

### Pattern 2: Parallel Composition

```
         ┌─► [Skill A] ─┐
[Input] ─┼─► [Skill B] ─┼─► [Merge] → [Output]
         └─► [Skill C] ─┘
```

**Use When**: Independent tasks can run simultaneously.

```yaml
# Parallel composition definition
composition:
  name: "full-audit"
  type: parallel
  skills:
    - skill: "security-scan"
      weight: 0.4  # Importance for scoring

    - skill: "performance-check"
      weight: 0.3

    - skill: "accessibility-audit"
      weight: 0.2

    - skill: "bundle-analyze"
      weight: 0.1

  merge_strategy: "aggregate"  # combine | aggregate | first_success

  aggregation:
    type: "weighted_score"
    format: |
      ## Full Audit Report

      ### Security: {security-scan.score}/100 (weight: 40%)
      {security-scan.summary}

      ### Performance: {performance-check.score}/100 (weight: 30%)
      {performance-check.summary}

      ### Accessibility: {accessibility-audit.score}/100 (weight: 20%)
      {accessibility-audit.summary}

      ### Bundle: {bundle-analyze.score}/100 (weight: 10%)
      {bundle-analyze.summary}

      **Overall Score: {weighted_average}/100**
```

### Pattern 3: Conditional Composition

```
              ┌─► [Skill A] (if condition X)
[Input] ─► [Router] ─┼─► [Skill B] (if condition Y)
              └─► [Skill C] (default)
```

**Use When**: Different paths based on input or context.

```yaml
# Conditional composition definition
composition:
  name: "smart-commit"
  type: conditional

  router:
    input: "file_changes"
    conditions:
      - when: "has_migrations()"
        skill: "commit-migration"

      - when: "only_docs()"
        skill: "commit-docs"

      - when: "has_breaking_changes()"
        skill: "commit-breaking"

      - when: "is_hotfix()"
        skill: "commit-hotfix"

      - default: true
        skill: "commit-standard"

  router_functions:
    has_migrations: |
      return files.some(f => f.path.includes('/migrations/'))

    only_docs: |
      return files.every(f => f.path.endsWith('.md'))

    has_breaking_changes: |
      return files.some(f => f.diff.includes('BREAKING'))

    is_hotfix: |
      return context.branch.startsWith('hotfix/')
```

### Pattern 4: Hybrid Composition

```
         ┌─► [Skill A] ─┐
[Input] ─┤              ├─► [Skill D] ─► [Skill E] → [Output]
         └─► [Skill B] ─┘
              │
              └─► (conditional) [Skill C]
```

**Use When**: Complex workflows with mixed dependencies.

```yaml
# Hybrid composition definition
composition:
  name: "release-workflow"
  type: hybrid

  stages:
    - name: "validation"
      type: parallel
      skills:
        - "lint-check"
        - "test-run"
        - "type-check"
      gate: "all_pass"

    - name: "security"
      type: conditional
      router:
        - when: "has_dependencies_changed()"
          skill: "deps-audit"
        - when: "has_api_changes()"
          skill: "security-scan"
        - default: "security-quick"

    - name: "build"
      type: sequential
      skills:
        - "changelog-generate"
        - "version-bump"
        - "build-production"

    - name: "deploy"
      type: sequential
      requires: "stages.validation.passed && stages.build.passed"
      skills:
        - "deploy-staging"
        - "e2e-smoke"
        - "deploy-production"
```

---

## DEPENDENCY RESOLUTION

### Dependency Types

| Type | Symbol | Description |
|------|--------|-------------|
| Hard | `requires` | Must complete successfully |
| Soft | `prefers` | Use if available, continue without |
| Optional | `optional` | Skip if unavailable |
| Blocking | `blocks` | Prevents parallel execution |

### Dependency Graph

```yaml
# Dependency specification
dependencies:
  skill_a:
    requires: []  # No dependencies

  skill_b:
    requires: ["skill_a"]  # Must run after A

  skill_c:
    requires: ["skill_a"]  # Can run parallel with B

  skill_d:
    requires: ["skill_b", "skill_c"]  # Needs both B and C
    prefers: ["skill_e"]  # Better with E, but optional

  skill_e:
    optional: true  # Skip if not available
```

### Resolution Algorithm

```typescript
interface DependencyNode {
  skill: string;
  requires: string[];
  prefers: string[];
  optional: boolean;
  status: 'pending' | 'running' | 'complete' | 'failed' | 'skipped';
}

function resolveDependencies(nodes: DependencyNode[]): ExecutionPlan {
  const graph = buildDependencyGraph(nodes);
  const order = topologicalSort(graph);

  // Identify parallel groups
  const groups = identifyParallelGroups(order, graph);

  return {
    executionOrder: order,
    parallelGroups: groups,
    criticalPath: findCriticalPath(graph)
  };
}

// Execution plan output
interface ExecutionPlan {
  executionOrder: string[];
  parallelGroups: string[][];
  criticalPath: string[];
}
```

### Circular Dependency Detection

```
CIRCULAR DEPENDENCY ERROR

Detected cycle in skill dependencies:
  skill_a → skill_b → skill_c → skill_a

Resolution options:
  1. Remove dependency: skill_c → skill_a
  2. Merge skills: skill_a + skill_c
  3. Introduce intermediate skill
```

---

## CONFLICT HANDLING

### Conflict Types

| Conflict | Description | Resolution |
|----------|-------------|------------|
| Output Collision | Same output name | Namespace prefix |
| Resource Lock | Same file/resource | Sequential execution |
| State Conflict | Incompatible state changes | Transaction rollback |
| Version Mismatch | Different dependency versions | Highest compatible |

### Conflict Resolution Strategies

```yaml
# Conflict resolution configuration
conflict_resolution:
  output_collision:
    strategy: "namespace"  # namespace | override | merge
    pattern: "{skill_name}_{output_name}"

  resource_lock:
    strategy: "queue"  # queue | fail | timeout
    timeout: 30000  # ms
    retry: 3

  state_conflict:
    strategy: "rollback"  # rollback | force | manual
    checkpoint: true

  version_mismatch:
    strategy: "highest"  # highest | lowest | exact
    fail_on_incompatible: true
```

### Conflict Detection

```typescript
interface ConflictReport {
  type: 'output' | 'resource' | 'state' | 'version';
  skills: string[];
  resource: string;
  severity: 'warning' | 'error' | 'critical';
  suggestion: string;
}

function detectConflicts(composition: Composition): ConflictReport[] {
  const conflicts: ConflictReport[] = [];

  // Check output names
  const outputs = collectOutputs(composition.skills);
  const duplicateOutputs = findDuplicates(outputs);
  // ... generate reports

  return conflicts;
}
```

---

## INTERFACE COMPATIBILITY

### Interface Definition

```yaml
# Skill interface specification
interface:
  name: "AuditSkill"
  version: "1.0.0"

  inputs:
    - name: "target"
      type: "string | string[]"
      required: true
    - name: "config"
      type: "AuditConfig"
      required: false

  outputs:
    - name: "score"
      type: "number"
      range: [0, 100]
    - name: "findings"
      type: "Finding[]"
    - name: "summary"
      type: "string"

  contracts:
    - "score >= 0 && score <= 100"
    - "findings.length >= 0"
```

### Compatibility Check

```typescript
function checkCompatibility(
  source: SkillInterface,
  target: SkillInterface
): CompatibilityResult {
  const issues: CompatibilityIssue[] = [];

  // Check output -> input mapping
  for (const input of target.inputs) {
    if (input.required) {
      const matchingOutput = source.outputs.find(
        o => isTypeCompatible(o.type, input.type)
      );
      if (!matchingOutput) {
        issues.push({
          type: 'missing_input',
          field: input.name,
          expected: input.type
        });
      }
    }
  }

  return {
    compatible: issues.length === 0,
    issues,
    suggestions: generateSuggestions(issues)
  };
}
```

### Type Compatibility Matrix

| Source Type | Target Type | Compatible | Notes |
|-------------|-------------|------------|-------|
| `string` | `string` | Yes | Exact match |
| `string[]` | `string` | Yes | First element |
| `number` | `string` | Yes | Coercion |
| `object` | `interface` | Maybe | Shape check |
| `any` | `*` | Yes | Universal |
| `unknown` | `typed` | No | Needs narrowing |

---

## EXAMPLE COMPOSITIONS

### Example 1: CI/CD Pipeline

```yaml
composition:
  name: "cicd-pipeline"
  version: "2.0.0"
  type: hybrid

  stages:
    - name: "quality"
      type: parallel
      skills:
        - "lint-check"
        - "type-check"
        - "unit-tests"

    - name: "security"
      type: parallel
      skills:
        - "deps-audit"
        - "secret-scan"
        - "sast-scan"

    - name: "build"
      type: sequential
      requires: ["quality", "security"]
      skills:
        - "build-app"
        - "bundle-analyze"

    - name: "deploy"
      type: conditional
      router:
        - when: "branch == 'main'"
          skill: "deploy-production"
        - when: "branch.startsWith('release/')"
          skill: "deploy-staging"
        - default: "deploy-preview"
```

### Example 2: Code Review Composite

```yaml
composition:
  name: "full-code-review"
  version: "1.0.0"
  type: parallel

  skills:
    - skill: "review-security"
      weight: 0.25
    - skill: "review-performance"
      weight: 0.20
    - skill: "review-maintainability"
      weight: 0.20
    - skill: "review-testing"
      weight: 0.15
    - skill: "review-documentation"
      weight: 0.10
    - skill: "review-accessibility"
      weight: 0.10

  merge:
    strategy: "aggregate"
    output:
      overall_score: "weighted_average(scores)"
      critical_issues: "flatten(findings.critical)"
      recommendations: "dedupe(suggestions)"
```

### Example 3: Migration Workflow

```yaml
composition:
  name: "safe-migration"
  version: "1.0.0"
  type: sequential

  skills:
    - skill: "backup-database"
      rollback_on_fail: false

    - skill: "validate-migration"
      gate: "errors.length == 0"

    - skill: "apply-migration"
      rollback_skill: "rollback-migration"

    - skill: "verify-migration"
      retry: 3

    - skill: "update-types"
      optional: true

  rollback:
    trigger: "any_failure"
    skills: ["rollback-migration", "verify-rollback"]
```

---

## COMPOSITION COMMANDS

### Creation Commands

| Command | Description |
|---------|-------------|
| `/compose new [name]` | Create new composition |
| `/compose add [skill]` | Add skill to composition |
| `/compose link [a] [b]` | Create dependency |
| `/compose validate` | Check composition validity |
| `/compose visualize` | Show dependency graph |

### Execution Commands

| Command | Description |
|---------|-------------|
| `/compose run [name]` | Execute composition |
| `/compose dry-run [name]` | Simulate without execution |
| `/compose status [name]` | Show execution status |
| `/compose cancel [name]` | Stop running composition |

---

## BEST PRACTICES

### Do

| Practice | Reason |
|----------|--------|
| Keep skills atomic | Easier to compose |
| Define clear interfaces | Better compatibility |
| Handle partial failures | Robust compositions |
| Use meaningful names | Self-documenting |
| Version compositions | Change tracking |

### Don't

| Anti-Pattern | Problem |
|--------------|---------|
| Deep nesting | Hard to debug |
| Tight coupling | Reduces flexibility |
| Ignoring failures | Silent errors |
| Circular deps | Deadlock |
| Giant skills | Not composable |

---

<!-- SKILL_COMPOSITION v23.0.0 | Updated: 2026-01-27 -->
