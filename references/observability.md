# Observability Reference

> Structured logging, distributed tracing, and anomaly detection for skill execution monitoring.

---

## Overview

### Three Pillars of Observability

```
┌─────────────────────────────────────────────────────────────────┐
│                    OBSERVABILITY ARCHITECTURE                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│   ┌─────────┐      ┌─────────┐      ┌─────────┐                │
│   │  LOGS   │      │ TRACES  │      │ METRICS │                │
│   └────┬────┘      └────┬────┘      └────┬────┘                │
│        │                │                │                      │
│        ▼                ▼                ▼                      │
│   What happened?   How did it flow?  What's the state?         │
│                                                                  │
│        └────────────────┼────────────────┘                      │
│                         ▼                                        │
│              ┌──────────────────┐                                │
│              │ ANOMALY DETECTION│                                │
│              └────────┬─────────┘                                │
│                       ▼                                          │
│              ┌──────────────────┐                                │
│              │      ALERTS      │                                │
│              └──────────────────┘                                │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Structured Logging

### Log Format Specification

```typescript
interface StructuredLog {
  // Required fields
  timestamp: string;          // ISO 8601
  level: LogLevel;            // DEBUG, INFO, WARN, ERROR, FATAL
  message: string;            // Human-readable description

  // Context fields
  service: string;            // singularity-forge
  version: string;            // 21.0.0
  environment: string;        // development, staging, production

  // Correlation
  trace_id: string;           // UUID for distributed tracing
  span_id: string;            // UUID for span within trace

  // Execution context
  skill_name?: string;
  command?: string;
  user_id?: string;           // Anonymized

  // Performance
  duration_ms?: number;
  token_count?: number;

  // Error details
  error?: { code: string; message: string; stack?: string; };
}

type LogLevel = 'DEBUG' | 'INFO' | 'WARN' | 'ERROR' | 'FATAL';
```

### Log Level Guidelines

| Level | Usage | Example |
|-------|-------|---------|
| DEBUG | Development details | "Parsed input: {tokens: 150}" |
| INFO | Normal operations | "Skill execution started" |
| WARN | Potential issues | "Approaching token limit (90%)" |
| ERROR | Recoverable failures | "API call failed, retrying" |
| FATAL | Unrecoverable failures | "Circuit breaker open" |

### Log Sanitization

```typescript
const SENSITIVE_FIELDS = ['password', 'token', 'api_key', 'secret', 'authorization'];

function sanitizeLog(log: Record<string, unknown>): Record<string, unknown> {
  const sanitized = { ...log };
  for (const key of Object.keys(sanitized)) {
    if (SENSITIVE_FIELDS.some(f => key.toLowerCase().includes(f))) {
      sanitized[key] = '[REDACTED]';
    }
  }
  return sanitized;
}
```

---

## Execution Tracing

### Trace Structure

```
Trace (trace_id: abc123)
├── Span: skill_execution (span_id: span1, 250ms)
│   ├── Span: input_validation (span_id: span2, 5ms)
│   ├── Span: safety_checks (span_id: span3, 10ms)
│   ├── Span: llm_call (span_id: span4, 200ms)
│   │   ├── Attribute: model = claude-3-opus
│   │   ├── Attribute: tokens = 1500
│   │   └── Attribute: temperature = 0.7
│   └── Span: output_validation (span_id: span5, 15ms)
└── Status: OK
```

### Span Implementation

```typescript
interface Span {
  trace_id: string;
  span_id: string;
  parent_span_id?: string;
  operation_name: string;
  start_time: Date;
  end_time?: Date;
  duration_ms?: number;
  status: 'OK' | 'ERROR' | 'CANCELLED';
  attributes: Record<string, string | number | boolean>;
  events: SpanEvent[];
}

class Tracer {
  startSpan(operationName: string, parentSpanId?: string): Span {
    return {
      trace_id: getCurrentTraceId() || crypto.randomUUID(),
      span_id: crypto.randomUUID(),
      parent_span_id: parentSpanId,
      operation_name: operationName,
      start_time: new Date(),
      status: 'OK',
      attributes: {},
      events: [],
    };
  }

  endSpan(spanId: string, status: 'OK' | 'ERROR' = 'OK'): void {
    const span = this.activeSpans.get(spanId);
    if (span) {
      span.end_time = new Date();
      span.duration_ms = span.end_time.getTime() - span.start_time.getTime();
      span.status = status;
      this.exportSpan(span);
    }
  }
}
```

---

## Anomaly Detection

### Statistical Deviation Detection

```typescript
class ZScoreDetector {
  detect(metric: MetricValue, baseline: MetricBaseline): AnomalyResult {
    const zscore = (metric.value - baseline.mean) / baseline.stddev;
    const absZscore = Math.abs(zscore);

    return {
      is_anomaly: absZscore > 3,  // 3-sigma rule
      score: Math.min(absZscore / 5, 1),
      deviation_sigmas: absZscore,
      direction: zscore > 0 ? 'HIGH' : zscore < 0 ? 'LOW' : 'NORMAL',
    };
  }
}
```

### Pattern Matching Anomalies

| Pattern | Detection | Threshold |
|---------|-----------|-----------|
| Spike | Value > p99 | Immediate alert |
| Trend | 5+ consecutive increases | Warn on 3rd |
| Absence | No events in window | Based on expected frequency |
| Frequency | Events/min > 2x baseline | Alert + throttle |

---

## Metrics Collection

### Core Metrics

| Metric | Type | Labels | Purpose |
|--------|------|--------|---------|
| `skill_executions_total` | Counter | skill, status | Total executions |
| `skill_duration_seconds` | Histogram | skill | Execution time |
| `skill_tokens_used` | Counter | skill, type | Token consumption |
| `guardrail_blocks_total` | Counter | guardrail, reason | Security blocks |
| `anomaly_detections_total` | Counter | type, severity | Anomalies found |

```typescript
class SkillMetrics {
  recordExecution(skill: string, status: 'success' | 'error', durationMs: number): void {
    this.registry.counter('skill_executions_total', { skill, status });
    this.registry.histogram('skill_duration_seconds', durationMs / 1000, { skill });
  }

  recordTokens(skill: string, type: 'input' | 'output', count: number): void {
    this.registry.counter('skill_tokens_used', { skill, type });
  }
}
```

---

## Dashboard Integration

### Health Dashboard Layout

```
┌─────────────────────────────────────────────────────────────────┐
│                  SKILL OBSERVABILITY DASHBOARD                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────┐  ┌─────────────────┐  ┌─────────────────┐ │
│  │ HEALTH: 98%     │  │ ERROR RATE: 0.2%│  │ P99 LATENCY: 2s │ │
│  └─────────────────┘  └─────────────────┘  └─────────────────┘ │
│                                                                  │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │ EXECUTIONS (24h)                                           │ │
│  │     ▂▄▆█▆▄▂▁▂▄▆█▆▄▂▁▂▄▆█▆▄▂▁                              │ │
│  └────────────────────────────────────────────────────────────┘ │
│                                                                  │
│  ┌─────────────────────────┐  ┌────────────────────────────┐   │
│  │ TOP SKILLS (by calls)   │  │ RECENT ERRORS              │   │
│  │ 1. forge-skill    1.2k  │  │ 12:34 [ERR] Timeout on... │   │
│  │ 2. qa-runner      890   │  │ 12:33 [ERR] Validation... │   │
│  └─────────────────────────┘  └────────────────────────────┘   │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## Alert Triggers

### Alert Rules

| Alert | Condition | Severity | Action |
|-------|-----------|----------|--------|
| High Error Rate | error_rate > 5% for 5m | CRITICAL | Page on-call |
| Latency Spike | p99 > 5s for 3m | WARNING | Notify channel |
| Token Exhaustion | tokens > 90% limit | WARNING | Throttle + notify |
| Anomaly Detected | score > 0.9 | INFO | Log for review |
| Circuit Open | breaker = OPEN | CRITICAL | Page + rollback |

```typescript
const ALERT_RULES: AlertRule[] = [
  {
    name: 'HighErrorRate',
    condition: 'error_rate > 0.05',
    duration: '5m',
    severity: 'CRITICAL',
    actions: [{ type: 'pagerduty' }, { type: 'slack', channel: '#alerts' }],
  },
  {
    name: 'LatencySpike',
    condition: 'p99_latency > 5s',
    duration: '3m',
    severity: 'WARNING',
    actions: [{ type: 'slack', channel: '#monitoring' }],
  },
];
```

---

<!-- OBSERVABILITY v23.0.0 | Updated: 2026-01-27 -->
