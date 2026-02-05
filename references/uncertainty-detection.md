# Uncertainty Detection Reference

> **SINGULARITY FORGE v21.0.0** | DRAGIN-Inspired Research Triggers
> **Pattern**: Dynamic Retrieval Augmented Generation for INcomplete knowledge
> **Purpose**: Automatically detect when research is needed before/during builds

---

## CORE PRINCIPLE

```
┌─────────────────────────────────────────────────────────────────┐
│ "Better to research and verify than to hallucinate and fail"    │
│                                                                 │
│ DRAGIN PHILOSOPHY:                                              │
│ • Detect uncertainty BEFORE generating wrong code               │
│ • Trigger research at the MOMENT of doubt                       │
│ • Never proceed with confidence < 0.7                           │
│ • Uncertainty is a feature, not a bug                           │
└─────────────────────────────────────────────────────────────────┘
```

---

## DETECTION SIGNALS

### Signal Categories

| Category | Signal | Threshold | Action |
|----------|--------|-----------|--------|
| **Confidence** | Self-assessed certainty | < 0.7 | Research |
| **Domain** | Unfamiliar technology | Unknown stack | Research |
| **Temporal** | Knowledge age | > 6 months | Verify |
| **Complexity** | Multi-system integration | > 3 systems | Research |
| **Specificity** | Version-specific behavior | Breaking changes | Research |
| **Contradiction** | Conflicting information | Any conflict | Research |

### Signal Detection Matrix

```
┌──────────────────────────────────────────────────────────────────┐
│                  UNCERTAINTY DETECTION MATRIX                    │
├──────────────────┬───────────┬───────────┬───────────┬──────────┤
│ Signal           │ Low Risk  │ Medium    │ High Risk │ Critical │
├──────────────────┼───────────┼───────────┼───────────┼──────────┤
│ Confidence Score │ 0.9-1.0   │ 0.7-0.89  │ 0.5-0.69  │ < 0.5    │
│ Domain Familiarity│ Expert   │ Working   │ Basic     │ Unknown  │
│ Knowledge Age    │ < 1 month │ 1-6 months│ 6-12 mo   │ > 1 year │
│ API Stability    │ Stable    │ Minor ver │ Major ver │ Breaking │
│ Documentation    │ Complete  │ Partial   │ Sparse    │ None     │
├──────────────────┼───────────┼───────────┼───────────┼──────────┤
│ Action           │ Proceed   │ Optional  │ Mandatory │ STOP     │
│                  │           │ Research  │ Research  │ Research │
└──────────────────┴───────────┴───────────┴───────────┴──────────┘
```

---

## CONFIDENCE SCORING

### Self-Assessment Protocol

```typescript
interface ConfidenceAssessment {
  topic: string;
  overallScore: number;      // 0.0 - 1.0
  factors: {
    knowledgeRecency: number;   // When did I learn this?
    practicalExperience: number; // Have I built this before?
    documentationAccess: number; // Can I cite sources?
    communityConsensus: number;  // Is this well-established?
    versionCertainty: number;    // Am I sure about the version?
  };
  uncertainAspects: string[]; // What am I unsure about?
  recommendation: "PROCEED" | "RESEARCH" | "STOP";
}
```

### Score Calculation

```typescript
function calculateConfidence(factors: ConfidenceFactors): number {
  const weights = {
    knowledgeRecency: 0.25,
    practicalExperience: 0.30,
    documentationAccess: 0.20,
    communityConsensus: 0.15,
    versionCertainty: 0.10
  };

  let score = 0;
  for (const [factor, weight] of Object.entries(weights)) {
    score += factors[factor] * weight;
  }

  // Apply uncertainty penalty
  const uncertaintyPenalty = factors.uncertainAspects.length * 0.05;
  return Math.max(0, score - uncertaintyPenalty);
}
```

### Confidence Thresholds

| Score | Classification | Action Required |
|-------|----------------|-----------------|
| 0.95 - 1.00 | Expert | Proceed confidently |
| 0.85 - 0.94 | High | Proceed, quick verify |
| 0.70 - 0.84 | Medium | Optional research |
| 0.50 - 0.69 | Low | **MANDATORY research** |
| 0.00 - 0.49 | Critical | **STOP - Full research** |

---

## DOMAIN FAMILIARITY ASSESSMENT

### Technology Familiarity Matrix

```
┌────────────────────────────────────────────────────────────────┐
│              DOMAIN FAMILIARITY LEVELS                         │
├─────────────┬──────────────────────────────────────────────────┤
│ EXPERT      │ • Built production systems                       │
│ (1.0)       │ • Know edge cases and gotchas                    │
│             │ • Can debug without documentation                 │
├─────────────┼──────────────────────────────────────────────────┤
│ PROFICIENT  │ • Built multiple projects                        │
│ (0.8)       │ • Know common patterns                           │
│             │ • Need docs for edge cases                        │
├─────────────┼──────────────────────────────────────────────────┤
│ WORKING     │ • Built 1-2 projects                             │
│ (0.6)       │ • Know basic patterns                            │
│             │ • Need docs frequently                            │
├─────────────┼──────────────────────────────────────────────────┤
│ BASIC       │ • Tutorials and courses only                     │
│ (0.4)       │ • Conceptual understanding                       │
│             │ • Need docs constantly                            │
├─────────────┼──────────────────────────────────────────────────┤
│ UNKNOWN     │ • Never used this technology                     │
│ (0.0)       │ • No practical experience                        │
│             │ • **RESEARCH REQUIRED**                          │
└─────────────┴──────────────────────────────────────────────────┘
```

---

## RESEARCH TRIGGER LOGIC

### Decision Tree

```
                    ┌─────────────────────────┐
                    │ Skill Build Request     │
                    └───────────┬─────────────┘
                                │
                    ┌───────────▼─────────────┐
                    │ Assess Confidence       │
                    │ (all factors)           │
                    └───────────┬─────────────┘
                                │
              ┌─────────────────┼─────────────────┐
              │                 │                 │
      ┌───────▼───────┐ ┌──────▼──────┐ ┌───────▼───────┐
      │ Score >= 0.85 │ │ 0.7 - 0.84  │ │ Score < 0.7   │
      │ PROCEED       │ │ OPTIONAL    │ │ MANDATORY     │
      └───────────────┘ └──────┬──────┘ └───────┬───────┘
                               │                │
                    ┌──────────▼──────────┐     │
                    │ Check Risk Factors: │     │
                    │ • Breaking changes? │     │
                    │ • Security impact?  │     │
                    │ • Production use?   │     │
                    └──────────┬──────────┘     │
                               │                │
                  ┌────────────┼────────────┐   │
                  │            │            │   │
          ┌───────▼───┐ ┌──────▼─────┐     │   │
          │ Low Risk  │ │ High Risk  │     │   │
          │ PROCEED   │ │ RESEARCH   │─────┴───┘
          └───────────┘ └────────────┘
                               │
                    ┌──────────▼──────────┐
                    │ TRIGGER RESEARCH    │
                    │ WORKFLOW            │
                    └─────────────────────┘
```

### Trigger Conditions (Any TRUE = Research)

```typescript
const triggerConditions = {
  // Confidence-based
  lowConfidence: confidence.overallScore < 0.7,

  // Domain-based
  unknownDomain: !knownDomains.includes(request.technology),
  newMajorVersion: isNewMajorVersion(request.technology),

  // Temporal-based
  outdatedKnowledge: getKnowledgeAge(request.topic) > 180, // days
  recentBreakingChange: hasRecentBreakingChange(request.technology),

  // Complexity-based
  multiSystemIntegration: request.integrations.length > 3,
  securitySensitive: request.involvesSecurity,

  // Contradiction-based
  conflictingPatterns: hasConflictingMemory(request.topic),
  deprecatedPattern: isPatternDeprecated(request.pattern)
};

const shouldResearch = Object.values(triggerConditions).some(v => v);
```

---

## RESEARCH WORKFLOW

### Triggered Research Flow

```
┌─────────────────────────────────────────────────────────────┐
│                    RESEARCH WORKFLOW                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  STEP 1: IDENTIFY GAPS                                      │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ • List specific uncertain aspects                   │    │
│  │ • Prioritize by impact on build                     │    │
│  │ • Formulate specific questions                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                          ↓                                  │
│  STEP 2: QUERY CONTEXT7                                     │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ FOR EACH uncertain_aspect:                          │    │
│  │   1. resolve-library-id(technology)                 │    │
│  │   2. query-docs(libraryId, specific_question)       │    │
│  │   3. Extract relevant patterns                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                          ↓                                  │
│  STEP 3: QUERY MEMORY                                       │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ • search_nodes(uncertain_aspect)                    │    │
│  │ • Check for past failures on similar builds         │    │
│  │ • Retrieve successful patterns in same domain       │    │
│  └─────────────────────────────────────────────────────┘    │
│                          ↓                                  │
│  STEP 4: SYNTHESIZE                                         │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ • Combine Context7 docs + Memory patterns           │    │
│  │ • Resolve any contradictions                        │    │
│  │ • Update confidence score                           │    │
│  └─────────────────────────────────────────────────────┘    │
│                          ↓                                  │
│  STEP 5: VALIDATE OR ESCALATE                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ IF new_confidence >= 0.7:                           │    │
│  │   → Proceed with build                              │    │
│  │ ELSE:                                               │    │
│  │   → Ask user for clarification                      │    │
│  │   → Or: Flag as "experimental" build                │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## UNCERTAINTY SCENARIOS

### Scenario 1: Unknown Technology

```typescript
// User requests: "Build a skill for Bun runtime"
const assessment = {
  topic: "Bun runtime",
  confidence: 0.3,
  factors: {
    knowledgeRecency: 0.2,    // New technology
    practicalExperience: 0.0, // Never used
    documentationAccess: 0.5, // Docs exist
    communityConsensus: 0.4,  // Still evolving
    versionCertainty: 0.3     // Frequent changes
  },
  uncertainAspects: [
    "Bun-specific APIs",
    "Compatibility with Node.js patterns",
    "Production readiness"
  ],
  recommendation: "STOP"
};

// Action: Full Context7 research before proceeding
```

### Scenario 2: Version Uncertainty

```typescript
// User requests: "Next.js server actions"
// But: Is this Next.js 13, 14, or 15?
const assessment = {
  topic: "Next.js Server Actions",
  confidence: 0.65,
  factors: {
    knowledgeRecency: 0.7,
    practicalExperience: 0.8,
    documentationAccess: 0.9,
    communityConsensus: 0.7,
    versionCertainty: 0.3     // Which version?
  },
  uncertainAspects: [
    "Server Action syntax changed between versions",
    "Form handling differences",
    "Caching behavior"
  ],
  recommendation: "RESEARCH"
};

// Action: Query Context7 for latest patterns, ask user for version
```

### Scenario 3: Conflicting Memory

```typescript
// Memory has two patterns for same task
const memoryConflict = {
  pattern1: {
    name: "supabase_auth_v1",
    approach: "Client-side auth with onAuthStateChange",
    effectivenessScore: 75
  },
  pattern2: {
    name: "supabase_auth_v2",
    approach: "Server-side auth with getUser()",
    effectivenessScore: 85
  }
};

// Both have good scores - which is current best practice?
const assessment = {
  confidence: 0.55,
  uncertainAspects: [
    "Which auth pattern is current best practice?",
    "Are both still valid for different use cases?"
  ],
  recommendation: "RESEARCH"
};

// Action: Query Context7 for current Supabase auth best practices
```

---

## ESCALATION PROTOCOL

### When Research Doesn't Resolve Uncertainty

```
IF (post_research_confidence < 0.7):
    ┌─────────────────────────────────────────┐
    │ ESCALATION OPTIONS                      │
    ├─────────────────────────────────────────┤
    │ 1. ASK USER for clarification           │
    │    "I need more details about..."       │
    │                                         │
    │ 2. BUILD EXPERIMENTAL                   │
    │    Flag output as "needs validation"    │
    │    Add warnings to generated skill      │
    │                                         │
    │ 3. PARTIAL BUILD                        │
    │    Build confident parts only           │
    │    Leave uncertain parts as TODOs       │
    │                                         │
    │ 4. DECLINE BUILD                        │
    │    "I cannot confidently build this"    │
    │    Suggest alternatives or resources    │
    └─────────────────────────────────────────┘
```

---

## INTEGRATION WITH BUILD PHASES

| Phase | Uncertainty Check | Action if Uncertain |
|-------|-------------------|---------------------|
| Phase 0 | Initial assessment | Full research workflow |
| Phase 1 | Pattern selection | Query Memory + Context7 |
| Phase 2 | Code generation | Verify syntax with docs |
| Phase 3 | Testing | Research test patterns |
| Phase 4 | Optimization | Verify best practices |
| Phase 5 | Validation | Cross-check with sources |

---

## METRICS & MONITORING

```typescript
interface UncertaintyMetrics {
  totalBuilds: number;
  researchTriggered: number;        // How often
  researchResolvedUncertainty: number; // Success rate
  falsePositives: number;           // Unnecessary research
  falseNegatives: number;           // Missed uncertainty
  averageConfidenceIncrease: number; // Research effectiveness
}
```

---

*Reference Version: 1.0.0 | Last Updated: 2026-01-26*
