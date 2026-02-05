# Memory Integration Reference

> **SINGULARITY FORGE v21.0.0** | Persistent Learning Across Sessions
> **MCP Server**: `mcp__memory__*`
> **Architecture**: MUSE (Memory Unified Storage Engine)

---

## CORE PRINCIPLE

```
┌─────────────────────────────────────────────────────────────────┐
│ SINGULARITY FORGE learns from EVERY skill build:               │
│                                                                 │
│   SUCCESS → Store pattern in memory for reuse                   │
│   FAILURE → Store anti-pattern to prevent repeat                │
│   PARTIAL → Store observation for refinement                    │
│                                                                 │
│ "Those who cannot remember the past are condemned to repeat it" │
└─────────────────────────────────────────────────────────────────┘
```

---

## ENTITY TYPES

### Primary Entities

| Entity Type | Purpose | Observations Stored |
|-------------|---------|---------------------|
| `successful_skill_pattern` | Proven working patterns | Code, gates passed, metrics |
| `failed_skill_pattern` | Anti-patterns to avoid | Failure reason, gate failed |
| `domain_knowledge` | Technology expertise | Best practices, gotchas |
| `user_preference` | User-specific settings | Style, stack, conventions |

### Entity Schema

```typescript
// Successful Skill Pattern
interface SuccessfulSkillPattern {
  name: string;           // Pattern identifier
  entityType: "successful_skill_pattern";
  observations: [
    "skill_type: {type}",
    "domain: {domain}",
    "gates_passed: {count}/52",
    "code_pattern: {base64_encoded}",
    "build_date: {ISO_date}",
    "reuse_count: {number}",
    "effectiveness_score: {0-100}"
  ];
}

// Failed Skill Pattern
interface FailedSkillPattern {
  name: string;
  entityType: "failed_skill_pattern";
  observations: [
    "failure_reason: {description}",
    "gate_failed: {gate_id}",
    "attempted_pattern: {description}",
    "root_cause: {analysis}",
    "prevention_strategy: {how_to_avoid}"
  ];
}

// Domain Knowledge
interface DomainKnowledge {
  name: string;           // e.g., "nextjs_app_router"
  entityType: "domain_knowledge";
  observations: [
    "best_practice: {pattern}",
    "common_gotcha: {issue}",
    "performance_tip: {optimization}",
    "security_concern: {vulnerability}",
    "version_specific: {version}:{note}"
  ];
}

// User Preference
interface UserPreference {
  name: string;           // e.g., "nadavcohen_preferences"
  entityType: "user_preference";
  observations: [
    "code_style: {style}",
    "preferred_stack: {technologies}",
    "naming_convention: {convention}",
    "rtl_priority: {true/false}",
    "test_coverage_threshold: {percentage}"
  ];
}
```

---

## LEARNING LOOP

### The Three-Phase Memory Cycle

```
┌─────────────────────────────────────────────────────────────┐
│                    LEARNING LOOP                            │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  PHASE 1: RECALL (Before Build)                             │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ search_nodes(skill_request_keywords)                │    │
│  │ open_nodes([relevant_pattern_names])                │    │
│  │ → Apply learned patterns to new build               │    │
│  └─────────────────────────────────────────────────────┘    │
│                          ↓                                  │
│  PHASE 2: BUILD (During Build)                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ Execute skill build with recalled knowledge         │    │
│  │ Track decisions, patterns used, gates passed        │    │
│  └─────────────────────────────────────────────────────┘    │
│                          ↓                                  │
│  PHASE 3: LEARN (After Build)                               │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ IF SUCCESS:                                         │    │
│  │   create_entities([new_successful_pattern])         │    │
│  │   add_observations([existing_pattern, reuse_count]) │    │
│  │                                                     │    │
│  │ IF FAILURE:                                         │    │
│  │   create_entities([new_failed_pattern])             │    │
│  │   create_relations([failure → cause])               │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## MUSE ARCHITECTURE

### Three Memory Types

```
┌─────────────────────────────────────────────────────────────┐
│                  MUSE MEMORY SYSTEM                         │
├──────────────────┬──────────────────┬───────────────────────┤
│    EPISODIC      │    SEMANTIC      │     PROCEDURAL        │
│    (Events)      │   (Knowledge)    │      (Skills)         │
├──────────────────┼──────────────────┼───────────────────────┤
│ • Build sessions │ • Domain facts   │ • Code patterns       │
│ • User requests  │ • Best practices │ • Build workflows     │
│ • Gate results   │ • Technology     │ • Validation steps    │
│ • Timestamps     │   relationships  │ • Auto-heal recipes   │
├──────────────────┼──────────────────┼───────────────────────┤
│ Entity Type:     │ Entity Type:     │ Entity Type:          │
│ build_session    │ domain_knowledge │ successful_skill_     │
│ user_request     │ technology_fact  │ pattern               │
│ gate_result      │ relationship     │ build_procedure       │
└──────────────────┴──────────────────┴───────────────────────┘
```

### Memory Decay & Reinforcement

| Memory Age | Retrieval Priority | Action |
|------------|-------------------|--------|
| < 7 days | HIGH | Use directly |
| 7-30 days | MEDIUM | Verify before use |
| 30-90 days | LOW | Validate with Context7 |
| > 90 days | VERY LOW | Consider refresh |

**Reinforcement Rules**:
- Each successful reuse: +10 to effectiveness_score
- Each failed reuse: -20 to effectiveness_score
- Score < 30: Flag for review
- Score > 80: Prioritize in recall

---

## MCP OPERATIONS

### 1. Create Entities (After Successful Build)

```typescript
await mcp.call("create_entities", {
  entities: [
    {
      name: "nextjs_server_action_pattern_v1",
      entityType: "successful_skill_pattern",
      observations: [
        "skill_type: code_pattern",
        "domain: nextjs_app_router",
        "gates_passed: 70/70",
        "code_pattern: " + base64Encode(codeSnippet),
        "build_date: 2026-01-26T12:00:00Z",
        "reuse_count: 1",
        "effectiveness_score: 85"
      ]
    }
  ]
});
```

### 2. Add Observations (Update Existing)

```typescript
await mcp.call("add_observations", {
  observations: [
    {
      entityName: "nextjs_server_action_pattern_v1",
      contents: [
        "reuse_count: 5",
        "effectiveness_score: 95",
        "last_used: 2026-01-26T15:00:00Z",
        "variant: with_optimistic_updates"
      ]
    }
  ]
});
```

### 3. Create Relations (Link Entities)

```typescript
await mcp.call("create_relations", {
  relations: [
    {
      from: "gate_failure_rls_missing",
      relationType: "CAUSED_BY",
      to: "missing_supabase_rls_pattern"
    },
    {
      from: "successful_skill_auth_flow",
      relationType: "USES",
      to: "nextjs_server_action_pattern_v1"
    },
    {
      from: "user_preference_nadavcohen",
      relationType: "PREFERS",
      to: "rtl_first_pattern"
    }
  ]
});
```

### 4. Search Nodes (Before Build)

```typescript
const searchResults = await mcp.call("search_nodes", {
  query: "nextjs server actions form validation"
});
// Returns: Matching entities with relevance scores
```

### 5. Open Nodes (Get Full Details)

```typescript
const nodes = await mcp.call("open_nodes", {
  names: [
    "nextjs_server_action_pattern_v1",
    "zod_validation_pattern"
  ]
});
// Returns: Full entity details with all observations
```

### 6. Read Graph (Full Knowledge Base)

```typescript
const fullGraph = await mcp.call("read_graph", {});
// Returns: All entities and relations
// Use sparingly - can be large
```

### 7. Delete Operations (Cleanup)

```typescript
// Delete outdated entities
await mcp.call("delete_entities", {
  entityNames: ["deprecated_pattern_v1"]
});

// Delete specific observations
await mcp.call("delete_observations", {
  deletions: [
    {
      entityName: "some_pattern",
      observations: ["outdated_observation"]
    }
  ]
});

// Delete relations
await mcp.call("delete_relations", {
  relations: [
    {
      from: "pattern_a",
      relationType: "USES",
      to: "deprecated_pattern"
    }
  ]
});
```

---

## INTEGRATION WORKFLOWS

### Pre-Build Recall

```typescript
async function recallRelevantKnowledge(request: SkillRequest) {
  // 1. Extract keywords from request
  const keywords = extractKeywords(request);

  // 2. Search for relevant patterns
  const searchResults = await mcp.call("search_nodes", {
    query: keywords.join(" ")
  });

  // 3. Get full details of top matches
  const relevantPatterns = await mcp.call("open_nodes", {
    names: searchResults.slice(0, 5).map(r => r.name)
  });

  // 4. Filter by effectiveness score
  return relevantPatterns.filter(p =>
    getObservation(p, "effectiveness_score") > 70
  );
}
```

### Post-Build Learning

```typescript
async function learnFromBuild(result: BuildResult) {
  if (result.success) {
    // Store successful pattern
    await mcp.call("create_entities", {
      entities: [{
        name: generatePatternName(result),
        entityType: "successful_skill_pattern",
        observations: extractObservations(result)
      }]
    });
  } else {
    // Store failure for prevention
    await mcp.call("create_entities", {
      entities: [{
        name: `failure_${result.skillName}_${Date.now()}`,
        entityType: "failed_skill_pattern",
        observations: [
          `failure_reason: ${result.error}`,
          `gate_failed: ${result.failedGate}`,
          `prevention_strategy: ${result.suggestedFix}`
        ]
      }]
    });
  }
}
```

---

## KNOWLEDGE GRAPH SCHEMA

```
                     ┌─────────────────┐
                     │ user_preference │
                     └────────┬────────┘
                              │ PREFERS
                              ▼
┌──────────────┐    ┌─────────────────────┐    ┌───────────────┐
│ build_session│───►│successful_skill_    │◄───│domain_        │
└──────────────┘    │pattern              │    │knowledge      │
      │             └─────────┬───────────┘    └───────────────┘
      │ PRODUCED              │ USES                    ▲
      ▼                       ▼                         │ RELATES_TO
┌──────────────┐    ┌─────────────────────┐            │
│ gate_result  │    │ code_pattern        │────────────┘
└──────────────┘    └─────────────────────┘
      │                       ▲
      │ REVEALED              │ PREVENTS
      ▼                       │
┌──────────────┐    ┌─────────────────────┐
│ failed_skill_│───►│ anti_pattern        │
│ pattern      │    └─────────────────────┘
└──────────────┘
```

---

## BEST PRACTICES

| Practice | Implementation |
|----------|----------------|
| Atomic observations | One fact per observation string |
| Consistent naming | `{domain}_{pattern}_{version}` |
| Regular cleanup | Delete entities with score < 30 |
| Version tracking | Include version in entity name |
| Cross-reference | Create relations between related entities |

---

*Reference Version: 1.0.0 | Last Updated: 2026-01-26*
