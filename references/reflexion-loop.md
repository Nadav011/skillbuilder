# Reflexion Loop Reference

> **SINGULARITY FORGE v21.0.0** | Self-Evaluation and Learning from Mistakes
> **Pattern**: REFLEXION - Reinforcement Learning from Language Feedback
> **Purpose**: Iterative improvement through self-reflection and memory

---

## CORE PRINCIPLE

```
┌─────────────────────────────────────────────────────────────────┐
│ REFLEXION PHILOSOPHY:                                           │
│                                                                 │
│ "The master has failed more times than the beginner has tried"  │
│                                                                 │
│ • Every failure is data                                         │
│ • Reflection transforms failure into knowledge                  │
│ • Memory prevents repeated mistakes                             │
│ • Iteration leads to mastery                                    │
└─────────────────────────────────────────────────────────────────┘
```

---

## THE 5-STEP REFLEXION LOOP

```
┌─────────────────────────────────────────────────────────────────┐
│                    REFLEXION LOOP                               │
│                                                                 │
│      ┌─────────┐                                                │
│      │  START  │                                                │
│      └────┬────┘                                                │
│           │                                                     │
│           ▼                                                     │
│   ┌───────────────┐                                             │
│   │   1. ACTOR    │ ◄──────────────────────────┐                │
│   │  Build Skill  │                            │                │
│   └───────┬───────┘                            │                │
│           │                                    │                │
│           ▼                                    │                │
│   ┌───────────────┐                            │                │
│   │ 2. EVALUATOR  │                            │                │
│   │  Run 70 Gates │                            │                │
│   └───────┬───────┘                            │                │
│           │                                    │                │
│     ┌─────┴─────┐                              │                │
│     │           │                              │                │
│  SUCCESS     FAILURE                           │                │
│     │           │                              │                │
│     ▼           ▼                              │                │
│  ┌─────┐  ┌───────────────┐                    │                │
│  │ END │  │  3. REFLECT   │                    │                │
│  └─────┘  │ Analyze Failure│                   │                │
│           └───────┬───────┘                    │                │
│                   │                            │                │
│                   ▼                            │                │
│           ┌───────────────┐                    │                │
│           │  4. MEMORY    │                    │                │
│           │ Store Lesson  │                    │                │
│           └───────┬───────┘                    │                │
│                   │                            │                │
│                   ▼                            │                │
│           ┌───────────────┐                    │                │
│           │   5. RETRY    │                    │                │
│           │ iteration++   │────────────────────┘                │
│           └───────────────┘                                     │
│                                                                 │
│   MAX_ITERATIONS: 3                                             │
└─────────────────────────────────────────────────────────────────┘
```

---

## STEP 1: ACTOR

### Build Execution

```typescript
interface ActorPhase {
  input: SkillRequest;
  context: {
    previousReflections: Reflection[];  // From past iterations
    memoryPatterns: Pattern[];          // From Memory MCP
    documentationCache: DocCache;       // From Context7
  };
  output: BuiltSkill;
}

async function executeActor(input: ActorPhase): Promise<BuiltSkill> {
  // Apply lessons from previous iterations
  const adjustedRequest = applyReflections(
    input.input,
    input.context.previousReflections
  );

  // Build with accumulated knowledge
  const skill = await buildSkill(adjustedRequest, {
    avoidPatterns: input.context.memoryPatterns
      .filter(p => p.type === "failed_skill_pattern"),
    usePatterns: input.context.memoryPatterns
      .filter(p => p.type === "successful_skill_pattern")
  });

  return skill;
}
```

---

## STEP 2: EVALUATOR

### Gate Validation Matrix

```
┌──────────────────────────────────────────────────────────────────┐
│                    70-GATE EVALUATION                            │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  CLUSTER 1: STRUCTURE (Gates 1-8)                                │
│  ┌────┬────┬────┬────┬────┬────┬────┬────┐                      │
│  │ G1 │ G2 │ G3 │ G4 │ G5 │ G6 │ G7 │ G8 │                      │
│  └────┴────┴────┴────┴────┴────┴────┴────┘                      │
│                                                                  │
│  CLUSTER 2: TYPE SAFETY (Gates 9-16)                             │
│  ┌────┬────┬────┬────┬────┬────┬────┬────┐                      │
│  │ G9 │G10 │G11 │G12 │G13 │G14 │G15 │G16 │                      │
│  └────┴────┴────┴────┴────┴────┴────┴────┘                      │
│                                                                  │
│  CLUSTER 3: SECURITY (Gates 17-24)                               │
│  ┌────┬────┬────┬────┬────┬────┬────┬────┐                      │
│  │G17 │G18 │G19 │G20 │G21 │G22 │G23 │G24 │                      │
│  └────┴────┴────┴────┴────┴────┴────┴────┘                      │
│                                                                  │
│  CLUSTER 4: PERFORMANCE (Gates 25-32)                            │
│  ... (continue for all 7 clusters)                               │
│                                                                  │
│  EVALUATION RESULT:                                              │
│  ┌─────────────────────────────────────────────────────────┐     │
│  │ PASSED: 48/52  │  FAILED: 4  │  STATUS: NEEDS_RETRY     │     │
│  │ Failed Gates: G19, G23, G31, G45                        │     │
│  └─────────────────────────────────────────────────────────┘     │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

### Evaluation Output

```typescript
interface EvaluationResult {
  success: boolean;
  gatesPassed: number;
  gatesTotal: 52;
  failedGates: GateFailure[];
  metrics: {
    typeScore: number;
    securityScore: number;
    performanceScore: number;
    accessibilityScore: number;
    rtlScore: number;
    responsiveScore: number;
    documentationScore: number;
  };
  recommendation: "ACCEPT" | "RETRY" | "ESCALATE";
}

interface GateFailure {
  gateId: string;
  gateName: string;
  cluster: string;
  severity: "CRITICAL" | "HIGH" | "MEDIUM" | "LOW";
  actualValue: any;
  expectedValue: any;
  errorMessage: string;
  suggestedFix: string;
}
```

---

## STEP 3: REFLECT

### Reflection Template

```typescript
interface Reflection {
  iteration: number;
  timestamp: string;

  // What happened
  failedGates: string[];
  errorMessages: string[];

  // Why it happened
  rootCauseAnalysis: {
    primaryCause: string;
    contributingFactors: string[];
    knowledgeGap: string | null;
  };

  // What to do differently
  actionItems: {
    immediate: string[];      // Fix for this iteration
    preventive: string[];     // Avoid in future builds
    learning: string[];       // Store in memory
  };

  // Self-assessment
  reflectionQuality: {
    specificEnough: boolean;
    actionable: boolean;
    addressesRootCause: boolean;
  };
}
```

### Reflection Generation Process

```typescript
async function generateReflection(
  skill: BuiltSkill,
  evaluation: EvaluationResult,
  iteration: number
): Promise<Reflection> {
  // 1. Analyze each failed gate
  const failureAnalysis = evaluation.failedGates.map(gate => ({
    gate: gate.gateId,
    cause: analyzeFailureCause(gate, skill),
    pattern: identifyFailurePattern(gate)
  }));

  // 2. Find root cause (not just symptoms)
  const rootCause = findRootCause(failureAnalysis);

  // 3. Generate specific action items
  const actions = generateActionItems(rootCause, failureAnalysis);

  // 4. Check if this is a known issue from memory
  const knownIssue = await searchMemory(rootCause.description);

  return {
    iteration,
    timestamp: new Date().toISOString(),
    failedGates: evaluation.failedGates.map(g => g.gateId),
    errorMessages: evaluation.failedGates.map(g => g.errorMessage),
    rootCauseAnalysis: {
      primaryCause: rootCause.description,
      contributingFactors: rootCause.factors,
      knowledgeGap: knownIssue ? null : rootCause.description
    },
    actionItems: actions,
    reflectionQuality: assessReflectionQuality(actions)
  };
}
```

### Root Cause Analysis Template

```
┌─────────────────────────────────────────────────────────────────┐
│                  ROOT CAUSE ANALYSIS                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  SYMPTOM: Gate G23 (Input Validation) failed                    │
│  ERROR: "Zod schema missing for form input"                     │
│                                                                 │
│  5-WHYS ANALYSIS:                                               │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ 1. WHY did validation fail?                             │    │
│  │    → No Zod schema was generated for user input         │    │
│  │                                                         │    │
│  │ 2. WHY was no schema generated?                         │    │
│  │    → Pattern didn't include validation step             │    │
│  │                                                         │    │
│  │ 3. WHY didn't pattern include validation?               │    │
│  │    → Used outdated server action pattern                │    │
│  │                                                         │    │
│  │ 4. WHY was outdated pattern used?                       │    │
│  │    → Memory contained pre-Zod pattern                   │    │
│  │                                                         │    │
│  │ 5. WHY wasn't pattern updated?                          │    │
│  │    → Context7 wasn't queried for latest patterns        │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                 │
│  ROOT CAUSE: Skipped Context7 research, relied on stale memory  │
│                                                                 │
│  ACTION: Always query Context7 when building form handlers      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## STEP 4: MEMORY

### Store Reflection in Memory

```typescript
async function storeReflectionInMemory(reflection: Reflection): Promise<void> {
  // 1. Create failure pattern entity
  await mcp.call("create_entities", {
    entities: [{
      name: `reflection_${reflection.timestamp.replace(/[^0-9]/g, '')}`,
      entityType: "failed_skill_pattern",
      observations: [
        `failed_gates: ${reflection.failedGates.join(',')}`,
        `root_cause: ${reflection.rootCauseAnalysis.primaryCause}`,
        `prevention: ${reflection.actionItems.preventive.join('; ')}`,
        `iteration: ${reflection.iteration}`,
        `timestamp: ${reflection.timestamp}`
      ]
    }]
  });

  // 2. If knowledge gap identified, create learning entity
  if (reflection.rootCauseAnalysis.knowledgeGap) {
    await mcp.call("create_entities", {
      entities: [{
        name: `knowledge_gap_${Date.now()}`,
        entityType: "domain_knowledge",
        observations: [
          `gap: ${reflection.rootCauseAnalysis.knowledgeGap}`,
          `learned_from: reflection_${reflection.timestamp}`,
          `action_required: research`
        ]
      }]
    });
  }

  // 3. Create relation between reflection and failed gates
  await mcp.call("create_relations", {
    relations: reflection.failedGates.map(gate => ({
      from: `reflection_${reflection.timestamp.replace(/[^0-9]/g, '')}`,
      relationType: "FAILED_AT",
      to: `gate_${gate}`
    }))
  });
}
```

### Retrieve Past Reflections

```typescript
async function getPastReflections(topic: string): Promise<Reflection[]> {
  const results = await mcp.call("search_nodes", {
    query: `reflection ${topic} failed_skill_pattern`
  });

  const reflections = await mcp.call("open_nodes", {
    names: results.map(r => r.name)
  });

  return reflections.map(parseReflectionFromEntity);
}
```

---

## STEP 5: RETRY

### Iteration Control

```typescript
const MAX_ITERATIONS = 3;

interface RetryDecision {
  shouldRetry: boolean;
  reason: string;
  modifications: string[];
}

function decideRetry(
  iteration: number,
  reflection: Reflection,
  evaluationHistory: EvaluationResult[]
): RetryDecision {
  // Check iteration limit
  if (iteration >= MAX_ITERATIONS) {
    return {
      shouldRetry: false,
      reason: "Maximum iterations reached",
      modifications: []
    };
  }

  // Check if making progress
  const progressRate = calculateProgressRate(evaluationHistory);
  if (progressRate < 0.1) {
    return {
      shouldRetry: false,
      reason: "Insufficient progress between iterations",
      modifications: []
    };
  }

  // Check if reflection is actionable
  if (!reflection.reflectionQuality.actionable) {
    return {
      shouldRetry: false,
      reason: "Cannot generate actionable improvements",
      modifications: []
    };
  }

  return {
    shouldRetry: true,
    reason: "Actionable improvements identified",
    modifications: reflection.actionItems.immediate
  };
}
```

### Iteration State Management

```typescript
interface IterationState {
  currentIteration: number;
  maxIterations: 3;
  history: {
    iteration: number;
    skill: BuiltSkill;
    evaluation: EvaluationResult;
    reflection: Reflection | null;
  }[];
  accumulatedLearning: string[];
}

function updateIterationState(
  state: IterationState,
  skill: BuiltSkill,
  evaluation: EvaluationResult,
  reflection: Reflection | null
): IterationState {
  return {
    ...state,
    currentIteration: state.currentIteration + 1,
    history: [...state.history, {
      iteration: state.currentIteration,
      skill,
      evaluation,
      reflection
    }],
    accumulatedLearning: reflection
      ? [...state.accumulatedLearning, ...reflection.actionItems.learning]
      : state.accumulatedLearning
  };
}
```

---

## ESCALATION PROTOCOL

### When MAX_ITERATIONS Reached

```
┌─────────────────────────────────────────────────────────────────┐
│                  ESCALATION PROTOCOL                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  AFTER 3 FAILED ITERATIONS:                                     │
│                                                                 │
│  1. COMPILE REFLECTION SUMMARY                                  │
│     • All failed gates across iterations                        │
│     • All root causes identified                                │
│     • All attempted fixes                                       │
│                                                                 │
│  2. PRESENT TO USER                                             │
│     "I've attempted 3 iterations but cannot resolve:            │
│      - [Gate failures]                                          │
│      - Root causes: [causes]                                    │
│      - What I tried: [fixes]                                    │
│      Please provide guidance on [specific question]"            │
│                                                                 │
│  3. STORE AS LEARNING OPPORTUNITY                               │
│     Create memory entity: "unresolved_challenge"                │
│     Include all context for future reference                    │
│                                                                 │
│  4. OFFER PARTIAL DELIVERY                                      │
│     "Would you like the skill with [X/52] gates passing?        │
│      Known limitations: [list]"                                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## REFLECTION EXAMPLES

### Example 1: Type Safety Failure

```json
{
  "iteration": 1,
  "timestamp": "2026-01-26T14:30:00Z",
  "failedGates": ["G12", "G14"],
  "errorMessages": [
    "Type 'any' found in function parameter",
    "Missing return type annotation"
  ],
  "rootCauseAnalysis": {
    "primaryCause": "Code generation used implicit any for unknown types",
    "contributingFactors": [
      "Complex generic type not properly inferred",
      "External library types not imported"
    ],
    "knowledgeGap": "TypeScript inference limits with third-party generics"
  },
  "actionItems": {
    "immediate": [
      "Add explicit type annotations to all function parameters",
      "Import types from @types/library-name"
    ],
    "preventive": [
      "Always check for @types packages before using libraries",
      "Generate type stubs for untyped libraries"
    ],
    "learning": [
      "TypeScript cannot infer types from untyped JS libraries"
    ]
  },
  "reflectionQuality": {
    "specificEnough": true,
    "actionable": true,
    "addressesRootCause": true
  }
}
```

### Example 2: Security Failure

```json
{
  "iteration": 2,
  "timestamp": "2026-01-26T14:45:00Z",
  "failedGates": ["G19", "G21"],
  "errorMessages": [
    "RLS policy missing on 'orders' table",
    "Service role key exposed in client code"
  ],
  "rootCauseAnalysis": {
    "primaryCause": "Security patterns not applied to all database operations",
    "contributingFactors": [
      "Focused on functionality, deferred security",
      "Copy-pasted pattern without security layer"
    ],
    "knowledgeGap": null
  },
  "actionItems": {
    "immediate": [
      "Add RLS policy: auth.uid() = user_id for orders table",
      "Move service role key to server-only module"
    ],
    "preventive": [
      "Run security gate check after EVERY database operation added",
      "Use checklist: RLS, key exposure, SQL injection"
    ],
    "learning": [
      "Security must be applied incrementally, not at the end"
    ]
  },
  "reflectionQuality": {
    "specificEnough": true,
    "actionable": true,
    "addressesRootCause": true
  }
}
```

---

## METRICS

```typescript
interface ReflexionMetrics {
  totalBuilds: number;
  firstAttemptSuccess: number;
  requiredIterations: {
    one: number;
    two: number;
    three: number;
    escalated: number;
  };
  commonFailurePatterns: {
    pattern: string;
    occurrences: number;
    successfulFix: string;
  }[];
  averageIterationsToSuccess: number;
  reflectionEffectiveness: number; // % of reflections that led to fix
}
```

---

*Reference Version: 1.0.0 | Last Updated: 2026-01-26*
