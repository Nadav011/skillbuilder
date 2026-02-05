# Research Workflow Reference

> **SINGULARITY FORGE v21.0.0** | Research-First Skill Building
> Every skill build/upgrade MUST start with external research

---

## RESEARCH MANDATE

```
+------------------------------------------------------------------+
|                    RESEARCH-FIRST PRINCIPLE                       |
+------------------------------------------------------------------+
|  NO SKILL SHALL BE BUILT WITHOUT CURRENT EXTERNAL RESEARCH        |
|  Knowledge cutoff means stale patterns - ALWAYS VERIFY            |
|  Research investment = 10-15% of total build time                 |
+------------------------------------------------------------------+
```

**Why Research First?**
- LLM knowledge has cutoff dates (May 2025 for current models)
- Best practices evolve rapidly
- New patterns emerge monthly
- Industry standards shift

---

## MANDATORY RESEARCH PHASES

### Phase 0: Pre-Research Assessment

| Step | Action | Output |
|------|--------|--------|
| 0.1 | Identify skill domain | Domain classification |
| 0.2 | Determine knowledge age risk | Risk level (Low/Medium/High) |
| 0.3 | List research questions | Question inventory |
| 0.4 | Estimate research scope | Time allocation |

**Knowledge Age Risk Matrix**:

| Domain | Risk Level | Research Depth |
|--------|------------|----------------|
| AI/ML/LLM | CRITICAL | Deep (5+ sources) |
| Cloud/DevOps | HIGH | Standard (3-4 sources) |
| Security | CRITICAL | Deep + Compliance check |
| Frontend Frameworks | HIGH | Version-specific |
| Database/Backend | MEDIUM | Standard |
| Core Programming | LOW | Light validation |
| Mathematics/Algorithms | LOW | Light validation |

---

## WEBSEARCH PATTERNS

### Primary Search Templates

```markdown
## DOMAIN-SPECIFIC SEARCHES

### AI/Prompt Engineering
- "Claude prompt engineering [CURRENT_YEAR]"
- "Anthropic best practices [CURRENT_YEAR]"
- "LLM prompt patterns [CURRENT_YEAR]"
- "Claude system prompts guide"
- "XML structured prompts Claude"

### Framework-Specific
- "[FRAMEWORK] best practices [CURRENT_YEAR]"
- "[FRAMEWORK] [VERSION] migration guide"
- "[FRAMEWORK] performance optimization [CURRENT_YEAR]"
- "[FRAMEWORK] security vulnerabilities [CURRENT_YEAR]"

### Security Domain
- "OWASP top 10 [CURRENT_YEAR]"
- "[TECHNOLOGY] security audit checklist [CURRENT_YEAR]"
- "CVE [TECHNOLOGY] [CURRENT_YEAR]"

### DevOps/Infrastructure
- "[PLATFORM] deployment best practices [CURRENT_YEAR]"
- "CI/CD [TOOL] patterns [CURRENT_YEAR]"
- "Infrastructure as code [CURRENT_YEAR] trends"
```

### Search Execution Protocol

```typescript
interface ResearchQuery {
  primary: string;      // Main search query
  variants: string[];   // Alternative phrasings
  domains?: string[];   // Allowed domains filter
  recency: 'day' | 'week' | 'month' | 'year';
  minSources: number;   // Minimum sources required
}

// Example research execution
const researchPlan: ResearchQuery[] = [
  {
    primary: "Claude prompt engineering 2026",
    variants: [
      "Anthropic Claude best practices 2026",
      "Claude system prompt patterns"
    ],
    domains: ["anthropic.com", "docs.anthropic.com"],
    recency: "month",
    minSources: 3
  },
  {
    primary: "[DOMAIN] industry standards 2026",
    variants: ["[DOMAIN] best practices guide"],
    recency: "year",
    minSources: 2
  }
];
```

---

## RESEARCH EXECUTION WORKFLOW

### Step 1: Initial Broad Search

```
SEARCH SEQUENCE:
1. Official documentation (highest priority)
2. Vendor/creator resources
3. Industry publications
4. Academic papers (if applicable)
5. Community best practices (GitHub, blogs)
```

**Search Decision Tree**:

```
START: Define Research Goal
    |
    v
[Is this AI/LLM related?]
    |
   YES --> Search Anthropic docs FIRST
    |        |
    |        v
    |      [Found sufficient info?]
    |        |
    |       YES --> Validate with 2+ external sources
    |        |
    |       NO --> Expand to OpenAI, Google AI docs
    |
   NO --> [Is this framework-specific?]
            |
           YES --> Search official docs for version
            |        |
            |        v
            |      Search "[framework] [version] changelog"
            |
           NO --> General domain search
                    |
                    v
                  Validate with industry sources
```

### Step 2: Source Validation

| Source Type | Trust Level | Validation Required |
|-------------|-------------|---------------------|
| Official Docs | HIGH | Version check only |
| Vendor Blogs | HIGH | Date check |
| Academic Papers | HIGH | Citation check |
| Tech Blogs (major) | MEDIUM | Cross-reference |
| Stack Overflow | MEDIUM | Vote count + recency |
| Personal Blogs | LOW | Heavy validation |
| AI-Generated Content | SUSPECT | Manual verification |

**Source Quality Checklist**:

```markdown
## Source Validation Checklist

- [ ] Published within last 12 months (or version-specific)
- [ ] Author has verifiable expertise
- [ ] Content matches current version/release
- [ ] No contradictions with official docs
- [ ] Includes working code examples (if technical)
- [ ] Referenced by other credible sources
```

### Step 3: Information Synthesis

```
SYNTHESIS PROTOCOL:
1. Extract key patterns from each source
2. Identify consensus across sources
3. Note contradictions for resolution
4. Map to existing skill patterns
5. Document novel techniques
```

**Synthesis Template**:

```markdown
## Research Synthesis: [SKILL_NAME]

### Consensus Findings
| Finding | Sources | Confidence |
|---------|---------|------------|
| [Pattern A] | 4/5 sources | HIGH |
| [Pattern B] | 3/5 sources | MEDIUM |

### Contradictions Found
| Topic | Source A Says | Source B Says | Resolution |
|-------|---------------|---------------|------------|
| [Topic] | [Claim] | [Counter-claim] | [Decision] |

### Novel Techniques Discovered
1. [Technique]: [Description] - Source: [URL]
2. [Technique]: [Description] - Source: [URL]

### Gaps Identified
- [Gap 1]: No current best practice found
- [Gap 2]: Conflicting information, needs experimentation
```

---

## MULTI-AGENT RESEARCH SPAWNING

### When to Spawn Research Agents

| Condition | Agent Count | Specialization |
|-----------|-------------|----------------|
| Simple skill (single domain) | 0-1 | None |
| Cross-domain skill | 2-3 | Domain-specific |
| Complex/Enterprise skill | 3-5 | Domain + validation |
| Security-critical skill | 2+ | Security-focused |

### Research Agent Architecture

```yaml
# Multi-Agent Research Configuration
research_orchestration:
  coordinator:
    role: "Research Coordinator"
    responsibilities:
      - Define research questions
      - Allocate domains to agents
      - Synthesize findings
      - Resolve contradictions

  agents:
    - name: "Domain Expert Agent"
      focus: "Primary skill domain"
      searches: 5-10
      output: "domain-findings.md"

    - name: "Security Auditor Agent"
      focus: "Security implications"
      searches: 3-5
      output: "security-findings.md"

    - name: "Best Practices Agent"
      focus: "Industry patterns"
      searches: 3-5
      output: "patterns-findings.md"

    - name: "Validation Agent"
      focus: "Cross-reference verification"
      searches: 2-3
      output: "validation-report.md"
```

### Agent Communication Protocol

```typescript
interface ResearchAgentMessage {
  agentId: string;
  type: 'finding' | 'question' | 'contradiction' | 'complete';
  payload: {
    topic: string;
    content: string;
    sources: string[];
    confidence: 'high' | 'medium' | 'low';
  };
  timestamp: string;
}

// Coordination flow
const coordinationFlow = {
  phase1: "Parallel research by all agents",
  phase2: "Finding consolidation by coordinator",
  phase3: "Contradiction resolution",
  phase4: "Final synthesis",
  phase5: "Memory MCP integration"
};
```

---

## MEMORY MCP INTEGRATION

### Research Storage Schema

```typescript
// Memory MCP Entity Types for Research
interface ResearchEntity {
  entityType: 'research_finding' | 'research_source' | 'research_synthesis';
  name: string;
  observations: string[];
}

// Storage patterns
const researchEntities = [
  {
    entityType: 'research_finding',
    name: '[SKILL_NAME]_finding_[TOPIC]',
    observations: [
      'Finding: [description]',
      'Source: [URL]',
      'Date: [YYYY-MM-DD]',
      'Confidence: [HIGH/MEDIUM/LOW]',
      'Applied_to: [skill_component]'
    ]
  },
  {
    entityType: 'research_source',
    name: '[SKILL_NAME]_source_[INDEX]',
    observations: [
      'URL: [source_url]',
      'Title: [source_title]',
      'Author: [author_name]',
      'Published: [date]',
      'Trust_level: [HIGH/MEDIUM/LOW]',
      'Topics_covered: [topic1, topic2]'
    ]
  },
  {
    entityType: 'research_synthesis',
    name: '[SKILL_NAME]_synthesis_[DATE]',
    observations: [
      'Total_sources: [N]',
      'Key_findings: [summary]',
      'Contradictions_resolved: [N]',
      'Novel_patterns: [list]',
      'Gaps_identified: [list]'
    ]
  }
];
```

### Memory Storage Commands

```markdown
## Store Research Findings

### Create Entities
mcp__memory__create_entities({
  entities: [
    {
      name: "buildskill_research_2026_01",
      entityType: "research_synthesis",
      observations: [
        "Domain: Skill building",
        "Sources: 5",
        "Key finding: XML structure preferred for Claude",
        "Key finding: Research-first approach critical",
        "Date: 2026-01-26"
      ]
    }
  ]
})

### Create Relations
mcp__memory__create_relations({
  relations: [
    {
      from: "buildskill_research_2026_01",
      relationType: "informs",
      to: "singularity_forge_v21"
    }
  ]
})
```

---

## SOURCE CITATION TRACKING

### Citation Format

```markdown
## Standard Citation Format

### In-Document Citations
[^1]: [Author/Org] - "[Title]" - [URL] - Accessed [YYYY-MM-DD]

### Citation Database Entry
| ID | Source | URL | Date Accessed | Trust | Topics |
|----|--------|-----|---------------|-------|--------|
| R001 | Anthropic Docs | docs.anthropic.com/... | 2026-01-26 | HIGH | Prompts |
| R002 | [Author] Blog | [url] | 2026-01-26 | MEDIUM | Patterns |
```

### Citation Tracking System

```yaml
# Citation tracking for skill: [SKILL_NAME]
citations:
  - id: "R001"
    source: "Anthropic Documentation"
    url: "https://docs.anthropic.com/..."
    accessed: "2026-01-26"
    trust_level: "HIGH"
    findings_extracted:
      - "XML preferred for structured prompts"
      - "Use thinking blocks for complex reasoning"
    applied_to:
      - "prompt-engineering.md"
      - "SKILL.md section 3.2"

  - id: "R002"
    source: "[Tech Blog]"
    url: "[url]"
    accessed: "2026-01-26"
    trust_level: "MEDIUM"
    findings_extracted:
      - "[Finding]"
    validation:
      cross_referenced_with: ["R001", "R003"]
      conflicts: []
```

---

## RESEARCH QUALITY GATES

### Pre-Build Research Checklist

```markdown
## Research Completion Checklist

### Minimum Requirements
- [ ] 3+ sources consulted for primary domain
- [ ] Official documentation reviewed
- [ ] Knowledge dated within 12 months
- [ ] No unresolved critical contradictions
- [ ] Security implications researched (if applicable)
- [ ] All sources documented with citations

### Quality Metrics
| Metric | Minimum | Target |
|--------|---------|--------|
| Unique sources | 3 | 5+ |
| Official docs coverage | 1 | 2+ |
| Recency (avg months) | <12 | <6 |
| Cross-validation rate | 60% | 80%+ |
| Citation completeness | 100% | 100% |

### Sign-off
- [ ] Research phase complete
- [ ] Findings integrated into build plan
- [ ] Memory MCP updated
- [ ] Ready for Phase 1: Foundation
```

---

## RESEARCH TEMPLATES

### Quick Research Template (Simple Skills)

```markdown
# Quick Research: [SKILL_NAME]

## Search Queries Executed
1. "[Query 1]" - [N results reviewed]
2. "[Query 2]" - [N results reviewed]

## Key Sources
| Source | Finding | Applied To |
|--------|---------|------------|
| [URL] | [Finding] | [Component] |

## Validation
- [ ] Findings consistent with official docs
- [ ] No security concerns identified

## Time Spent: [X minutes]
```

### Deep Research Template (Complex Skills)

```markdown
# Deep Research: [SKILL_NAME]

## Research Plan
- Domain: [Primary domain]
- Risk Level: [LOW/MEDIUM/HIGH/CRITICAL]
- Estimated Time: [X hours]
- Agents Spawned: [N]

## Phase 1: Broad Search
[Queries and initial findings]

## Phase 2: Deep Dive
[Detailed analysis per topic]

## Phase 3: Validation
[Cross-reference results]

## Phase 4: Synthesis
[Final consolidated findings]

## Appendix: Full Citation Database
[Complete citation list]
```

---

## ANTI-PATTERNS

| Anti-Pattern | Problem | Correct Approach |
|--------------|---------|------------------|
| Skip research for "simple" skills | Stale patterns | Always validate basics |
| Trust single source | Bias risk | Minimum 3 sources |
| Ignore contradictions | Build on uncertainty | Resolve before building |
| No citation tracking | Lost provenance | Document everything |
| Outdated official docs | Version mismatch | Check release dates |
| Over-research | Time waste | Set scope limits |

---

## RESEARCH METRICS

### Time Allocation Guidelines

| Skill Complexity | Research Time | % of Total Build |
|------------------|---------------|------------------|
| Simple | 15-30 min | 10% |
| Standard | 30-60 min | 12% |
| Complex | 1-2 hours | 15% |
| Enterprise | 2-4 hours | 15-20% |

### Quality Indicators

```
RESEARCH QUALITY SCORE = (
  (Sources_Count / Min_Sources) * 0.2 +
  (Official_Docs_Count / 2) * 0.3 +
  (Avg_Recency_Score) * 0.2 +
  (Cross_Validation_Rate) * 0.2 +
  (Citation_Completeness) * 0.1
) * 100

Target Score: >= 80%
```

---

<!-- RESEARCH_WORKFLOW v23.0.0 | Updated: 2026-01-27 -->
