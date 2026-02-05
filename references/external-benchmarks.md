# External Benchmarks Reference

> **SINGULARITY FORGE v21.0.0** | Industry Standards Comparison
> Measure skills against authoritative external sources

---

## BENCHMARK MANDATE

```
+------------------------------------------------------------------+
|                    EXTERNAL VALIDATION REQUIRED                   |
+------------------------------------------------------------------+
|  Skills must compare favorably to industry benchmarks             |
|  Sources: Anthropic, OpenAI, Google, Academic Research            |
|  Goal: Meet or exceed published best practices                    |
+------------------------------------------------------------------+
```

---

## BENCHMARK SOURCES

### Primary Sources (Highest Authority)

| Source | Domain | URL | Update Frequency |
|--------|--------|-----|------------------|
| Anthropic Docs | Claude/Prompts | docs.anthropic.com | Weekly |
| Anthropic Research | AI Patterns | anthropic.com/research | Monthly |
| Anthropic Cookbook | Examples | github.com/anthropics | Weekly |

### Secondary Sources (High Authority)

| Source | Domain | URL | Update Frequency |
|--------|--------|-----|------------------|
| OpenAI Docs | GPT Patterns | platform.openai.com | Weekly |
| Google AI | Gemini/Vertex | ai.google.dev | Monthly |
| Microsoft AI | Azure AI | learn.microsoft.com | Monthly |
| Hugging Face | ML Patterns | huggingface.co | Daily |

### Academic Sources

| Source | Domain | URL | Update Frequency |
|--------|--------|-----|------------------|
| arXiv CS.CL | NLP/LLM | arxiv.org/list/cs.CL | Daily |
| arXiv CS.AI | AI General | arxiv.org/list/cs.AI | Daily |
| ACL Anthology | NLP Research | aclanthology.org | Conference-based |
| NeurIPS | ML Research | neurips.cc | Annual |

### Industry Standards

| Source | Domain | URL | Update Frequency |
|--------|--------|-----|------------------|
| OWASP | Security | owasp.org | Annual |
| W3C | Web Standards | w3.org | Varies |
| WCAG | Accessibility | w3.org/WAI | Multi-year |
| IEEE | Engineering | ieee.org | Varies |

---

## SCORING FRAMEWORK

### Multi-Dimensional Score Matrix

| Dimension | Weight | Source | Benchmark |
|-----------|--------|--------|-----------|
| Correctness | 30% | Golden set + Judge | 90%+ pass rate |
| Efficiency | 20% | Performance tests | <P95 industry |
| Safety | 20% | Security tests | 100% pass |
| Clarity | 15% | Output evaluation | 85%+ judge score |
| Robustness | 15% | Edge case tests | 90%+ pass rate |

### Scoring Algorithm

```typescript
interface BenchmarkScore {
  dimension: string;
  weight: number;
  rawScore: number;  // 0-100
  benchmark: number; // Industry standard
  delta: number;     // rawScore - benchmark
  weighted: number;  // rawScore * weight
}

function calculateOverallScore(scores: BenchmarkScore[]): OverallScore {
  const totalWeighted = scores.reduce((sum, s) => sum + s.weighted, 0);
  const avgDelta = scores.reduce((sum, s) => sum + s.delta, 0) / scores.length;

  return {
    overall: totalWeighted,
    grade: getGrade(totalWeighted),
    vsIndustry: avgDelta >= 0 ? 'ABOVE' : 'BELOW',
    deltaFromBenchmark: avgDelta,
    breakdown: scores
  };
}

function getGrade(score: number): Grade {
  if (score >= 95) return 'S';  // Exceptional
  if (score >= 90) return 'A';  // Excellent
  if (score >= 80) return 'B';  // Good
  if (score >= 70) return 'C';  // Acceptable
  if (score >= 60) return 'D';  // Needs improvement
  return 'F';                    // Failing
}
```

### Grade Definitions

| Grade | Score | Meaning | Action |
|-------|-------|---------|--------|
| S | 95-100 | Exceptional, exceeds all benchmarks | Publish as example |
| A | 90-94 | Excellent, meets all benchmarks | Ready for production |
| B | 80-89 | Good, meets most benchmarks | Minor improvements |
| C | 70-79 | Acceptable, meets basic standards | Review recommended |
| D | 60-69 | Needs improvement | Revise before deploy |
| F | <60 | Failing, below standards | Major revision required |

---

## COMPARISON METHODOLOGY

### Benchmark Comparison Process

```
1. IDENTIFY
   Find relevant benchmarks for skill domain
   Source: Primary sources first, then secondary

2. EXTRACT
   Pull specific metrics from benchmark sources
   Document source URL and date

3. TEST
   Run skill against benchmark criteria
   Capture quantitative results

4. COMPARE
   Calculate delta from benchmark
   Categorize as above/at/below

5. REPORT
   Generate comparison report
   Highlight gaps and strengths
```

### Comparison Template

```yaml
benchmark_comparison:
  skill: "skill-name"
  version: "1.0.0"
  date: "2026-01-26"

  benchmarks:
    - source: "Anthropic Prompt Engineering Guide"
      url: "https://docs.anthropic.com/..."
      date_accessed: "2026-01-26"
      criteria:
        - name: "XML Structure Usage"
          benchmark: "Always use for complex prompts"
          skill_status: "Compliant"
          evidence: "SKILL.md uses XML throughout"

        - name: "Chain of Thought"
          benchmark: "Use for multi-step reasoning"
          skill_status: "Compliant"
          evidence: "Includes thinking instructions"

    - source: "OWASP Top 10 2024"
      url: "https://owasp.org/..."
      criteria:
        - name: "Injection Prevention"
          benchmark: "Validate all inputs"
          skill_status: "Compliant"
          evidence: "Input validation in security tests"

  summary:
    total_criteria: 15
    compliant: 14
    partial: 1
    non_compliant: 0
    score: 96
    grade: "S"
```

---

## GAP ANALYSIS TEMPLATE

### Gap Identification

```markdown
## Gap Analysis Report

**Skill:** {skill_name}
**Benchmark Source:** {source}
**Analysis Date:** {date}

### Gaps Identified

| # | Gap | Severity | Benchmark Says | Skill Does | Remediation |
|---|-----|----------|----------------|------------|-------------|
| 1 | Missing retry logic | Medium | "Always implement retries" | No retries | Add exponential backoff |
| 2 | No rate limiting | Low | "Consider rate limits" | Not implemented | Add optional rate limit |

### Gap Severity Matrix

| Severity | Count | % of Total |
|----------|-------|------------|
| Critical | 0 | 0% |
| High | 0 | 0% |
| Medium | 1 | 50% |
| Low | 1 | 50% |

### Remediation Priority

1. **Medium: Missing retry logic**
   - Effort: 2 hours
   - Impact: Reliability improvement
   - Recommended: Before production

2. **Low: No rate limiting**
   - Effort: 1 hour
   - Impact: Edge case handling
   - Recommended: Future enhancement
```

### Gap Severity Definitions

| Severity | Definition | Required Action |
|----------|------------|-----------------|
| Critical | Fails core benchmark | Block deployment |
| High | Missing important feature | Fix before production |
| Medium | Below optimal | Plan remediation |
| Low | Nice to have | Track for future |

---

## INDUSTRY TRENDS TRACKING

### Trend Monitoring

```yaml
# Trend tracking configuration
trend_tracking:
  domains:
    - name: "LLM Prompting"
      sources:
        - "Anthropic Research"
        - "arXiv CS.CL"
      keywords:
        - "prompt engineering"
        - "in-context learning"
        - "chain of thought"
      frequency: "weekly"

    - name: "AI Security"
      sources:
        - "OWASP"
        - "Anthropic Safety"
      keywords:
        - "AI security"
        - "prompt injection"
        - "jailbreak prevention"
      frequency: "weekly"

  alerts:
    - condition: "new_best_practice"
      action: "flag_for_review"
    - condition: "deprecation_notice"
      action: "immediate_review"
    - condition: "security_advisory"
      action: "urgent_review"
```

### Trend Impact Assessment

| Trend | Impact | Skill Affected | Action |
|-------|--------|----------------|--------|
| New prompting technique | Medium | prompt-engineering.md | Evaluate adoption |
| Security vulnerability | High | All skills | Immediate review |
| API deprecation | Critical | Affected integrations | Migration required |
| Performance optimization | Low | Performance-focused | Optional enhancement |

---

## BENCHMARK REPORT FORMAT

### Executive Summary

```markdown
# Benchmark Report: {skill_name}

## Executive Summary

| Metric | Score | Industry | Status |
|--------|-------|----------|--------|
| Overall | 92/100 | 85 | +7 Above |
| Correctness | 95/100 | 90 | +5 Above |
| Efficiency | 88/100 | 85 | +3 Above |
| Safety | 100/100 | 100 | = At |
| Clarity | 90/100 | 85 | +5 Above |
| Robustness | 85/100 | 80 | +5 Above |

**Grade: A (Excellent)**
**Status: Above Industry Benchmark**
```

### Detailed Breakdown

```markdown
## Detailed Benchmark Analysis

### Correctness (30% weight)

**Score: 95/100 | Benchmark: 90 | Delta: +5**

| Criterion | Source | Required | Actual | Pass |
|-----------|--------|----------|--------|------|
| Input validation | Anthropic | Always | Yes | ✓ |
| Output formatting | Internal | 90%+ | 95% | ✓ |
| Error handling | OWASP | Complete | Yes | ✓ |

### Efficiency (20% weight)

**Score: 88/100 | Benchmark: 85 | Delta: +3**

| Criterion | Source | Target | Actual | Pass |
|-----------|--------|--------|--------|------|
| Response time | Internal | <5s | 3.2s | ✓ |
| Token usage | Anthropic | Optimize | Good | ✓ |
| Memory | Internal | <100MB | 45MB | ✓ |

[... continues for each dimension ...]
```

---

## QUICK COMMANDS

| Command | Description |
|---------|-------------|
| `/benchmark skill-name` | Run full benchmark comparison |
| `/benchmark skill-name --gaps` | Show gap analysis only |
| `/benchmark skill-name --trends` | Check against latest trends |
| `/benchmark skill-name --report` | Generate detailed report |
| `/benchmark --sources` | List all benchmark sources |

---

## BENCHMARK CHECKLIST

### Pre-Deployment Benchmark Check

```markdown
## Benchmark Checklist

### Core Requirements
- [ ] Compared against Anthropic guidelines
- [ ] Security benchmarks met (OWASP)
- [ ] Performance within industry norms
- [ ] Accessibility standards checked (if applicable)

### Score Requirements
- [ ] Overall score >= 80 (Grade B+)
- [ ] No critical gaps
- [ ] Safety score = 100%

### Documentation
- [ ] Benchmark sources documented
- [ ] Gap analysis completed
- [ ] Remediation plan for any gaps

### Sign-off
- [ ] Benchmark comparison complete
- [ ] Results acceptable for deployment
- [ ] Trend alignment verified
```

---

<!-- EXTERNAL_BENCHMARKS v23.0.0 | Updated: 2026-01-27 -->
