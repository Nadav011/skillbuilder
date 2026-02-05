# Execution Guardrails Reference

> Industry-standard guardrail patterns for skill execution safety, validation, and quality assurance.

---

## Overview

### Guardrail Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                     GUARDRAIL EXECUTION FLOW                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  INPUT → [PRE-GUARD] → EXECUTION → [IN-GUARD] → [POST-GUARD] → OUTPUT
│              │              │           │            │
│              ▼              ▼           ▼            ▼
│          Validate       Monitor     Throttle     Validate
│          Classify       Resource    Timeout      Filter
│          Sanitize       Track       Circuit      Quality
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Guardrail Classification

| Type | When | Purpose | Latency Impact |
|------|------|---------|----------------|
| PRE | Before execution | Prevent bad input | <10ms |
| IN | During execution | Resource protection | <1ms per check |
| POST | After execution | Output quality | <50ms |

---

## PRE-EXECUTION GUARDRAILS

### G1: Input Validation

```typescript
import { z } from 'zod';

const SkillInputSchema = z.object({
  command: z.string().min(1).max(10000),
  context: z.object({
    files: z.array(z.string()).max(100),
    workingDir: z.string().regex(/^[a-zA-Z0-9\/_.-]+$/),
  }),
  options: z.record(z.unknown()).optional(),
});

function validateInput(raw: unknown): Result<SkillInput, ValidationError> {
  const result = SkillInputSchema.safeParse(raw);
  if (!result.success) {
    return err(new ValidationError(result.error.flatten()));
  }
  return ok(result.data);
}
```

### G2: Intent Classification

| Intent Category | Action | Example |
|-----------------|--------|---------|
| CREATE | Standard execution | "Create a new component" |
| MODIFY | Validate existing | "Update the config file" |
| DELETE | Require confirmation | "Remove all test files" |
| READ | Lower privilege | "Show me the schema" |
| EXECUTE | Elevated scrutiny | "Run the deploy script" |
| UNKNOWN | Request clarification | Ambiguous request |

### G3: Safety Checks

| Check | Block If | Severity |
|-------|----------|----------|
| Path Traversal | Contains `../` or absolute paths outside workspace | CRITICAL |
| Credential Access | Targets `.env`, `*.pem`, `*secret*` | CRITICAL |
| System Modification | Modifies `/etc`, `/usr`, system files | CRITICAL |
| Infinite Loop Risk | Unbounded recursion detected | HIGH |
| Resource Exhaustion | Request exceeds resource limits | HIGH |

```typescript
const BLOCKED_PATTERNS = [
  { pattern: /\.\.\//, reason: 'Path traversal detected', severity: 'CRITICAL' },
  { pattern: /\.(env|pem|key)$/i, reason: 'Credential file access', severity: 'CRITICAL' },
  { pattern: /\/etc\/|\/usr\/|\/var\//, reason: 'System path access', severity: 'CRITICAL' },
  { pattern: /rm\s+-rf\s+\//, reason: 'Dangerous delete command', severity: 'CRITICAL' },
];

function runSafetyChecks(input: string): SafetyCheckResult {
  for (const check of BLOCKED_PATTERNS) {
    if (check.pattern.test(input)) {
      return { passed: false, blocked_reason: check.reason, severity: check.severity };
    }
  }
  return { passed: true };
}
```

### G4: Rate Limiting

```typescript
interface RateLimitConfig {
  requests_per_minute: number;
  requests_per_hour: number;
  tokens_per_minute: number;
  concurrent_executions: number;
}

const DEFAULT_LIMITS: RateLimitConfig = {
  requests_per_minute: 60,
  requests_per_hour: 1000,
  tokens_per_minute: 100000,
  concurrent_executions: 5,
};
```

---

## IN-PROCESS GUARDRAILS

### G5: Token Limits

| Limit Type | Threshold | Action |
|------------|-----------|--------|
| Input tokens | 50,000 | Truncate or summarize |
| Output tokens | 16,000 | Stream with limit |
| Total context | 200,000 | Summarize history |
| Per-step tokens | 8,000 | Force checkpoint |

### G6: Timeout Handling

| Operation | Timeout | Recovery |
|-----------|---------|----------|
| File read | 5s | Skip with warning |
| API call | 30s | Retry with backoff |
| Code execution | 60s | Terminate gracefully |
| Total skill | 300s | Checkpoint and resume |

### G7: Resource Monitoring

```typescript
const RESOURCE_THRESHOLDS = {
  memory_mb: { warning: 512, critical: 1024 },
  cpu_percent: { warning: 70, critical: 90 },
  open_files: { warning: 100, critical: 500 },
  active_connections: { warning: 50, critical: 100 },
};
```

### G8: Circuit Breaker

```typescript
class CircuitBreaker {
  private state: 'CLOSED' | 'OPEN' | 'HALF_OPEN' = 'CLOSED';
  private failures = 0;

  async execute<T>(operation: () => Promise<T>): Promise<Result<T, CircuitOpenError>> {
    if (this.state === 'OPEN') {
      if (this.shouldAttemptReset()) {
        this.state = 'HALF_OPEN';
      } else {
        return err(new CircuitOpenError('Circuit breaker is open'));
      }
    }

    try {
      const result = await operation();
      this.recordSuccess();
      return ok(result);
    } catch (error) {
      this.recordFailure();
      throw error;
    }
  }
}
```

---

## POST-EXECUTION GUARDRAILS

### G9: Output Validation

```typescript
const OUTPUT_VALIDATORS = [
  {
    name: 'no_credentials',
    pattern: /(api[_-]?key|password|secret|token)\s*[:=]\s*['"]\w+['"]/gi,
    severity: 'CRITICAL',
    action: 'REDACT',
  },
  {
    name: 'no_pii',
    pattern: /\b[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}\b/gi,
    severity: 'HIGH',
    action: 'MASK',
  },
];
```

### G10: Content Filtering

| Filter Type | Detects | Action |
|-------------|---------|--------|
| Credential | API keys, passwords, tokens | Redact |
| PII | Emails, phones, SSN | Mask |
| Profanity | Inappropriate language | Remove |
| Code Injection | XSS, SQL injection | Sanitize |

### G11: Quality Checks

```typescript
interface QualityMetrics {
  completeness: number;     // Did it answer the question?
  correctness: number;      // Is the answer accurate?
  coherence: number;        // Is it well-structured?
  conciseness: number;      // Is it appropriately brief?
  actionability: number;    // Can user act on it?
}

const QUALITY_THRESHOLDS = {
  minimum: 0.6,
  acceptable: 0.75,
  good: 0.85,
  excellent: 0.95,
};
```

---

## Guardrail Bypass Detection

### Bypass Patterns

| Pattern | Description | Detection |
|---------|-------------|-----------|
| Prompt Injection | Malicious instructions in input | Pattern matching |
| Jailbreak Attempt | Override system instructions | Boundary detection |
| Context Manipulation | Fake system messages | Role validation |
| Encoding Tricks | Base64, unicode obfuscation | Decode and re-analyze |

```typescript
const BYPASS_DETECTORS = [
  {
    type: 'PROMPT_INJECTION',
    patterns: [
      /ignore\s+(previous|all)\s+instructions/i,
      /disregard\s+(your|the)\s+(rules|guidelines)/i,
      /you\s+are\s+now\s+[a-z]+/i,
    ],
  },
  {
    type: 'JAILBREAK',
    patterns: [
      /DAN\s+mode/i,
      /developer\s+mode\s+enabled/i,
    ],
  },
];
```

---

## Escalation Triggers

| Trigger | Condition | Action | SLA |
|---------|-----------|--------|-----|
| CRITICAL_SAFETY | Safety check fails | Block + Alert | Immediate |
| HIGH_RISK | Delete/execute on production | Human approval | 5 min |
| UNCERTAINTY | Confidence < 0.5 | Request clarification | - |
| RESOURCE_LIMIT | >90% threshold | Throttle + Alert | 1 min |
| REPEATED_FAILURE | 3+ consecutive fails | Circuit break | - |

---

## Complete Guardrail Pipeline

```typescript
async function executeWithGuardrails<T>(
  input: unknown,
  executor: (validated: ValidatedInput) => Promise<T>
): Promise<Result<T, GuardrailError>> {
  // PRE-EXECUTION
  const validationResult = validateInput(input);
  if (!validationResult.success) {
    return err(new GuardrailError('INPUT_VALIDATION', validationResult.error));
  }

  const safetyResult = runSafetyChecks(validationResult.data.command);
  if (!safetyResult.passed) {
    return err(new GuardrailError('SAFETY_CHECK', safetyResult.blocked_reason));
  }

  // IN-PROCESS
  const circuitBreaker = new CircuitBreaker(DEFAULT_CIRCUIT_CONFIG);
  const executionResult = await circuitBreaker.execute(async () => {
    return await withTimeout(executor(validationResult.data), 300000, 'skill_execution');
  });

  if (!executionResult.success) {
    return executionResult;
  }

  // POST-EXECUTION
  const outputValidation = validateOutput(String(executionResult.data));
  if (!outputValidation.content_safe) {
    const filtered = filterContent(String(executionResult.data), DEFAULT_FILTER_CONFIG);
    return ok(filtered.content as T);
  }

  return executionResult;
}
```

---

<!-- GUARDRAILS v23.0.0 | Updated: 2026-01-27 -->
