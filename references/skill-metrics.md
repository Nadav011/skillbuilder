# Skill Metrics & Analytics - SINGULARITY FORGE Reference

## Overview

Track skill performance, usage, and quality over time to enable continuous improvement.

---

## 1. METRICS CATEGORIES

### Quality Metrics

| Metric | Description | Target | Measurement |
|--------|-------------|--------|-------------|
| Gate Pass Rate | % of gates passing | 100% | validate-skill.sh |
| Token ROI | Capabilities / tokens | > 0.5 | Manual calculation |
| Line Efficiency | Value / line count | High | Review |
| Reference Coverage | Documented features | 100% | Audit |

### Usage Metrics

| Metric | Description | Target | Source |
|--------|-------------|--------|--------|
| Activation Rate | Times triggered | Track | Usage logs |
| Completion Rate | Successful runs | > 95% | Logs |
| Error Rate | Failed executions | < 5% | Error logs |
| User Satisfaction | Feedback score | > 4/5 | Feedback |

### Performance Metrics

| Metric | Description | Target | Measurement |
|--------|-------------|--------|-------------|
| Load Time | Time to activate | < 100ms | Timer |
| Execution Time | Total workflow time | Varies | Timer |
| Token Usage | Tokens per execution | Minimize | Counter |
| Context Usage | % of context window | < 20% | Counter |

---

## 2. TOKEN ROI CALCULATION

### Formula

```
Token ROI = (Distinct Capabilities) / (SKILL.md Token Count) × 100
```

### Capability Counting

| Element | Capabilities |
|---------|--------------|
| Command | +1 per command |
| Workflow step | +0.5 per step |
| Decision branch | +0.25 per branch |
| Reference file | +0.5 per file |
| Template | +0.5 per template |

### Example Calculation

```
Skill: apex-perf v19.0.0

Capabilities:
- Commands: 16 × 1 = 16
- Workflow steps: 7 × 0.5 = 3.5
- References: 13 × 0.5 = 6.5
- Templates: 2 × 0.5 = 1
Total: 27 capabilities

Token Count: ~4000 tokens

Token ROI = 27 / 4000 × 100 = 0.675 (Excellent)
```

### ROI Ratings

| Score | Rating | Action |
|-------|--------|--------|
| > 1.0 | GOD-TIER | Perfect |
| 0.5 - 1.0 | Excellent | Production ready |
| 0.2 - 0.5 | Good | Minor optimization |
| < 0.2 | Needs Work | Major refactoring |

---

## 3. QUALITY DASHBOARD

### Gate Pass Matrix

```
┌─────────────────────────────────────────────────────────────┐
│                    GATE PASS MATRIX                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  Cluster 1 (Genesis):     ██████████ 7/7   100%             │
│  Cluster 2 (Laws):        ██████████ 7/7   100%             │
│  Cluster 3 (Cognitive):   ██████████ 7/7   100%             │
│  Cluster 4 (XML):         ██████████ 7/7   100%             │
│  Cluster 5 (Exorcist):    ██████████ 7/7   100%             │
│  Cluster 6 (UX):          ██████████ 7/7   100%             │
│  Cluster 7 (Neural):      ██████████ 7/7   100%             │
│  Cluster 8 (Scale):       ███████    3/3   100%             │
│                                                              │
│  TOTAL: 70/70 (100%) - GOD-TIER CERTIFIED                   │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

### Trend Tracking

```
Version History:
v17.3.2: 49/70 gates (94%) - 3 failures
v18.5.0: 52/70 gates (100%) - Certified
v19.0.0: 52/70 gates (100%) - Certified

Token ROI Trend:
v17.3.2: 0.42 (Good)
v18.5.0: 0.58 (Excellent)
v19.0.0: 0.68 (Excellent)
```

---

## 4. USAGE ANALYTICS

### Activation Tracking

```typescript
// Track skill activation
interface SkillActivation {
  skillName: string;
  trigger: string;
  timestamp: Date;
  success: boolean;
  duration: number;
  tokensUsed: number;
  errorCode?: string;
}

async function trackActivation(data: SkillActivation) {
  await mcp__memory__create_entities({
    entities: [{
      name: `activation-${data.skillName}-${Date.now()}`,
      entityType: 'SkillActivation',
      observations: [
        `Skill: ${data.skillName}`,
        `Trigger: ${data.trigger}`,
        `Success: ${data.success}`,
        `Duration: ${data.duration}ms`,
        `Tokens: ${data.tokensUsed}`,
      ],
    }],
  });
}
```

### Usage Report Format

```markdown
## Skill Usage Report: apex-perf

**Period:** Last 30 days

### Activation Summary
| Metric | Value |
|--------|-------|
| Total activations | 47 |
| Successful | 45 (96%) |
| Failed | 2 (4%) |
| Avg duration | 3.2s |
| Avg tokens | 2,100 |

### Top Triggers
1. `/perf` - 28 times
2. `/perf bundle` - 12 times
3. `/perf vitals` - 7 times

### Error Summary
| Error | Count | Resolution |
|-------|-------|------------|
| File not found | 1 | User provided wrong path |
| Timeout | 1 | Large codebase, increased limit |
```

---

## 5. PERFORMANCE BENCHMARKS

### Skill Loading Time

| Skill Type | Target | Warning | Critical |
|------------|--------|---------|----------|
| Simple | < 50ms | > 75ms | > 100ms |
| Standard | < 100ms | > 150ms | > 200ms |
| Complex | < 200ms | > 300ms | > 500ms |

### Execution Time Targets

| Phase | Target | Warning |
|-------|--------|---------|
| Input parsing | < 50ms | > 100ms |
| Analysis | < 2s | > 5s |
| Generation | < 3s | > 10s |
| Verification | < 1s | > 3s |
| Total | < 10s | > 30s |

### Token Usage Targets

| Component | Target | Warning |
|-----------|--------|---------|
| SKILL.md | < 5,000 | > 7,000 |
| Response | < 2,000 | > 4,000 |
| References | < 10,000 | > 15,000 |
| Total session | < 20,000 | > 30,000 |

---

## 6. HEALTH SCORING

### Skill Health Score (0-100)

```
Health Score = (
  Gate Pass Rate × 0.3 +
  Token ROI Score × 0.2 +
  Completion Rate × 0.2 +
  Performance Score × 0.15 +
  User Satisfaction × 0.15
) × 100
```

### Health Ratings

| Score | Rating | Status |
|-------|--------|--------|
| 90-100 | Excellent | ✅ Production ready |
| 70-89 | Good | ⚠️ Minor improvements |
| 50-69 | Fair | ⚠️ Needs attention |
| < 50 | Poor | ❌ Major refactoring |

### Health Dashboard

```
┌─────────────────────────────────────────────────────────────┐
│                    SKILL HEALTH DASHBOARD                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  apex-perf v19.0.0                                          │
│  ═══════════════════════════════════════════════════════    │
│                                                              │
│  Overall Health: ████████████████████ 95/100 EXCELLENT      │
│                                                              │
│  Gate Pass:      ████████████████████ 100%                  │
│  Token ROI:      ████████████████░░░░ 0.68 (Excellent)      │
│  Completion:     ████████████████████ 96%                   │
│  Performance:    ██████████████████░░ 89%                   │
│  Satisfaction:   ████████████████████ 4.8/5                 │
│                                                              │
│  Last Updated: 2026-01-26                                   │
│  Status: ✅ Production Ready                                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 7. IMPROVEMENT RECOMMENDATIONS

### Auto-Generated Recommendations

| Metric | Current | Target | Recommendation |
|--------|---------|--------|----------------|
| Token ROI | 0.45 | > 0.5 | Split SKILL.md to references/ |
| Load time | 180ms | < 100ms | Reduce skill.yaml size |
| Error rate | 8% | < 5% | Add error handling for edge case X |
| Gate 38 | Warning | Pass | Convert 2 lists to tables |

### Recommendation Priority

```
1. CRITICAL: Blocking production use
2. HIGH: Significant quality impact
3. MEDIUM: Optimization opportunity
4. LOW: Nice to have
```

---

## 8. METRICS COLLECTION

### Automated Collection

```bash
# Add to validate-skill.sh output
echo "METRICS:"
echo "  lines: $(wc -l < SKILL.md)"
echo "  tokens: $(wc -w < SKILL.md)"
echo "  commands: $(grep -c '| /' SKILL.md)"
echo "  references: $(ls references/ 2>/dev/null | wc -l)"
echo "  gates_passed: $PASSED"
echo "  gates_total: 52"
```

### Metrics Storage

```typescript
// Store in Memory MCP
await mcp__memory__create_entities({
  entities: [{
    name: `metrics-${skillName}-${version}`,
    entityType: 'SkillMetrics',
    observations: [
      `Version: ${version}`,
      `Gates: ${passed}/52`,
      `Token ROI: ${roi}`,
      `Lines: ${lines}`,
      `Commands: ${commands}`,
      `Date: ${new Date().toISOString()}`,
    ],
  }],
});
```

---

## 9. COMMANDS

| Command | Description |
|---------|-------------|
| `/forge --metrics [path]` | Show skill metrics |
| `/forge --health [path]` | Show health dashboard |
| `/forge --trends [path]` | Show version trends |
| `/forge --compare [a] [b]` | Compare two versions |

---

## 10. CHECKLIST

```markdown
## Metrics Implementation Checklist

- [ ] Gate pass rate tracked
- [ ] Token ROI calculated
- [ ] Usage analytics enabled
- [ ] Performance benchmarks set
- [ ] Health scoring implemented
- [ ] Trend tracking active
- [ ] Recommendations generated
- [ ] Dashboard available
```

---

_SINGULARITY FORGE v19.0.0 - Skill Metrics & Analytics_
