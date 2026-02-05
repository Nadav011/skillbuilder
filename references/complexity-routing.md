# Complexity Routing Reference

> Model selection and human escalation based on task complexity scoring.

---

## Overview

### Routing Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    COMPLEXITY ROUTER                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  INPUT ──▶ SCORE ──▶ ROUTE ──▶ EXECUTE                          │
│              │         │                                         │
│              ▼         ▼                                         │
│         ┌────────┬─────────┬────────┐                           │
│         │ SIMPLE │ MEDIUM  │COMPLEX │                           │
│         │ (0-30) │ (31-70) │(71-100)│                           │
│         └───┬────┴────┬────┴───┬────┘                           │
│             ▼         ▼        ▼                                 │
│          HAIKU     SONNET    OPUS                               │
│                                 │                                │
│                         [HUMAN ESCALATION]                       │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Complexity Scoring

### Scoring Algorithm

```typescript
interface ComplexityFactors {
  reasoning_depth: number;    // 0-25: How much reasoning required
  domain_expertise: number;   // 0-25: Specialized knowledge needed
  output_length: number;      // 0-25: Expected output size
  safety_sensitivity: number; // 0-25: Risk level of operation
}

function calculateComplexityScore(factors: ComplexityFactors): number {
  const weights = {
    reasoning_depth: 0.35,
    domain_expertise: 0.25,
    output_length: 0.15,
    safety_sensitivity: 0.25,
  };

  let score = 0;
  for (const [factor, weight] of Object.entries(weights)) {
    score += factors[factor as keyof ComplexityFactors] * weight * 4;
  }

  return Math.min(100, Math.max(0, score));
}
```

### Factor Definitions

#### Reasoning Depth (0-25)

| Score | Level | Example |
|-------|-------|---------|
| 0-5 | Trivial | Format conversion, simple lookup |
| 6-10 | Low | Template filling, basic calculation |
| 11-15 | Medium | Multi-step workflow, conditionals |
| 16-20 | High | Architecture decisions, trade-offs |
| 21-25 | Expert | Novel solutions, research synthesis |

#### Domain Expertise (0-25)

| Score | Level | Example |
|-------|-------|---------|
| 0-5 | General | Text formatting, file operations |
| 6-10 | Standard | Common programming patterns |
| 11-15 | Specialized | Framework-specific knowledge |
| 16-20 | Expert | Security, performance optimization |
| 21-25 | Research | Cutting-edge, novel domains |

#### Safety Sensitivity (0-25)

| Score | Level | Example |
|-------|-------|---------|
| 0-5 | Read-only | Analysis, queries |
| 6-10 | Low risk | Create new files |
| 11-15 | Medium risk | Modify existing files |
| 16-20 | High risk | Database changes, deployments |
| 21-25 | Critical | Production systems, security |

---

## Model Selection Matrix

### Primary Model Selection

| Score Range | Model | Cost/1K | Latency | Use Case |
|-------------|-------|---------|---------|----------|
| 0-30 | Haiku | $0.25 | Fast | Simple tasks |
| 31-70 | Sonnet | $3.00 | Medium | Standard tasks |
| 71-100 | Opus | $15.00 | Slow | Complex tasks |

### Model Capabilities

| Capability | Haiku | Sonnet | Opus |
|------------|-------|--------|------|
| Context window | 200K | 200K | 200K |
| Reasoning | Basic | Good | Excellent |
| Code quality | Adequate | Good | Best |
| Creativity | Limited | Good | Excellent |
| Speed | Fastest | Medium | Slowest |

### Selection Logic

```typescript
type ModelTier = 'haiku' | 'sonnet' | 'opus';

function selectModel(score: number, overrides?: ModelOverrides): ModelTier {
  if (overrides?.forced_model) {
    return overrides.forced_model;
  }

  if (score <= 30) return 'haiku';
  if (score <= 70) return 'sonnet';
  return 'opus';
}
```

---

## Human Escalation Triggers

### Trigger Matrix

| Trigger | Condition | Action | SLA |
|---------|-----------|--------|-----|
| UNCERTAINTY | Confidence < 50% | Clarify with user | Immediate |
| HIGH_RISK | Safety score > 20 | Require approval | 5 min |
| COST_THRESHOLD | Estimated > $1 | Confirm proceed | Immediate |
| SENSITIVE_DATA | PII/credentials | Block + alert | Immediate |
| NOVEL_DOMAIN | No similar patterns | Request guidance | 1 hour |
| CONFLICT | Multiple valid approaches | Present options | Immediate |

### Escalation Logic

```typescript
function checkEscalation(context: ExecutionContext, score: number): EscalationDecision {
  // Check uncertainty
  if (context.confidence < 0.5) {
    return {
      escalate: true,
      reason: 'UNCERTAINTY',
      action: 'clarify',
      message: 'Request is ambiguous. Please clarify.',
    };
  }

  // Check high risk
  if (context.safety_score > 20 && !context.pre_approved) {
    return {
      escalate: true,
      reason: 'HIGH_RISK',
      action: 'approve',
      message: `This action affects ${context.risk_target}. Approve? [y/n]`,
    };
  }

  // Check cost
  const estimatedCost = estimateCost(score, context.expected_tokens);
  if (estimatedCost > 1.0 && !context.cost_approved) {
    return {
      escalate: true,
      reason: 'COST_THRESHOLD',
      action: 'approve',
      message: `Estimated cost: $${estimatedCost.toFixed(2)}. Proceed?`,
    };
  }

  return { escalate: false };
}
```

---

## Routing Decision Tree

### Complete Decision Flow

```
INPUT
  │
  ▼
Calculate complexity score
  │
  ├── Score 0-30 ──▶ Route to HAIKU
  │                    │
  │                    ├── Check escalation triggers
  │                    │     ├── None ──▶ Execute
  │                    │     └── Triggered ──▶ Escalate
  │                    │
  │                    └── If quality fails ──▶ Upgrade to SONNET
  │
  ├── Score 31-70 ──▶ Route to SONNET
  │                    │
  │                    └── If quality fails ──▶ Upgrade to OPUS
  │
  └── Score 71-100 ──▶ Route to OPUS
                       │
                       └── If quality fails ──▶ Escalate to HUMAN
```

---

## Override Mechanisms

### Override Types

| Override | Effect | Use Case |
|----------|--------|----------|
| `forced_model` | Use specific model | Testing, consistency |
| `minimum_model` | Floor for model selection | Quality guarantee |
| `skip_escalation` | Bypass escalation checks | Pre-approved tasks |
| `cost_limit` | Max cost per task | Budget control |

### Configuration

```yaml
routing:
  defaults:
    minimum_model: haiku
    cost_limit: 0.50

  overrides:
    # Force Opus for architecture tasks
    - pattern: "architecture|design|system"
      forced_model: opus

    # Minimum Sonnet for code review
    - pattern: "review|audit"
      minimum_model: sonnet
```

---

## Cost Optimization

### Cost Estimation

```typescript
const PRICING: Record<ModelTier, { input: number; output: number }> = {
  haiku: { input: 0.00025, output: 0.00125 },
  sonnet: { input: 0.003, output: 0.015 },
  opus: { input: 0.015, output: 0.075 },
};

function estimateCost(model: ModelTier, tokens: TokenEstimate): number {
  const pricing = PRICING[model];
  return (tokens.input / 1000) * pricing.input +
         (tokens.output / 1000) * pricing.output;
}
```

### Optimization Strategies

| Strategy | Implementation | Savings |
|----------|----------------|---------|
| Right-sizing | Use smallest capable model | 50-80% |
| Caching | Cache frequent responses | 30-50% |
| Batching | Combine related tasks | 20-30% |
| Early exit | Stop when sufficient | 10-20% |

---

<!-- COMPLEXITY_ROUTING v23.0.0 | Updated: 2026-01-27 -->
