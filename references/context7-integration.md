# Context7 Integration Reference

> **SINGULARITY FORGE v21.0.0** | Always-Updated Documentation Lookup
> **MCP Server**: `plugin_context7_context7`
> **Purpose**: Real-time documentation retrieval during skill building

---

## MANDATORY USAGE PROTOCOL

### Two-Step Documentation Flow

```
STEP 1: resolve-library-id
         ↓
    [Get Context7 Library ID]
         ↓
STEP 2: query-docs
         ↓
    [Retrieve Documentation]
```

### Critical Rule

| Scenario | Action | Reason |
|----------|--------|--------|
| Building ANY skill | MUST call Context7 | Ensures up-to-date patterns |
| User provides library ID | Skip resolve, use directly | Format: `/org/project` |
| Unknown library | ALWAYS resolve first | Get valid Context7 ID |
| Max calls per question | 3 per tool | Prevent rate limiting |

---

## MCP TOOL REFERENCE

### Tool 1: resolve-library-id

**Purpose**: Convert package name to Context7-compatible library ID

```typescript
// MCP Call Structure
{
  tool: "mcp__plugin_context7_context7__resolve-library-id",
  params: {
    libraryName: string,  // e.g., "next.js", "react", "supabase"
    query: string         // User's original question for ranking
  }
}
```

**Selection Criteria** (in priority order):
1. Name similarity (exact matches prioritized)
2. Description relevance to query intent
3. Documentation coverage (higher Code Snippet counts)
4. Source reputation (High/Medium preferred)
5. Benchmark Score (100 = highest quality)

**Response Handling**:
```typescript
// Success: Extract library ID
const libraryId = response.libraryId; // e.g., "/vercel/next.js"

// Ambiguous: Multiple good matches
// → Proceed with most relevant, acknowledge alternatives

// No match: Suggest query refinements
// → Try broader/narrower search terms
```

### Tool 2: query-docs

**Purpose**: Retrieve documentation and code examples

```typescript
// MCP Call Structure
{
  tool: "mcp__plugin_context7_context7__query-docs",
  params: {
    libraryId: string,  // From resolve-library-id or user
    query: string       // Specific question about the library
  }
}
```

**Query Best Practices**:

| Good Query | Bad Query |
|------------|-----------|
| "How to set up authentication with JWT in Express.js" | "auth" |
| "React useEffect cleanup function examples" | "hooks" |
| "Next.js App Router server actions with forms" | "forms" |
| "Supabase RLS policy for user-owned rows" | "security" |

---

## INTEGRATION POINTS

### Phase 0: Research Integration

```
┌─────────────────────────────────────────────────────────┐
│ PHASE 0: DISCOVERY                                      │
├─────────────────────────────────────────────────────────┤
│ 1. Parse user request for technology keywords           │
│ 2. FOR EACH technology:                                 │
│    └─ resolve-library-id(tech, user_request)            │
│    └─ query-docs(libraryId, specific_questions)         │
│ 3. Synthesize documentation into skill requirements     │
│ 4. Store findings in Memory MCP for future reference    │
└─────────────────────────────────────────────────────────┘
```

### Phase 5: Validation Integration

```typescript
// Validate skill patterns against current docs
async function validateAgainstDocs(skill: Skill): Promise<ValidationResult> {
  const technologies = extractTechnologies(skill);

  for (const tech of technologies) {
    const libraryId = await resolveLibraryId(tech);
    const currentDocs = await queryDocs(libraryId, "best practices");

    const alignment = comparePatterns(skill.patterns, currentDocs);
    if (alignment.score < 0.8) {
      return {
        valid: false,
        reason: `Outdated pattern detected in ${tech}`,
        suggestion: currentDocs.recommendedPattern
      };
    }
  }

  return { valid: true };
}
```

### Auto-Heal Integration

```
TRIGGER: Gate failure due to outdated pattern
    ↓
CONTEXT7 LOOKUP:
    1. Identify failed technology
    2. resolve-library-id for that tech
    3. query-docs for "current best practices"
    ↓
AUTO-HEAL:
    1. Extract current pattern from docs
    2. Replace outdated pattern in skill
    3. Re-validate against gate
```

---

## DECISION TREE: When to Use Context7

```
                    ┌─────────────────────┐
                    │ Building/Updating   │
                    │ a Skill?            │
                    └─────────┬───────────┘
                              │
                    ┌─────────▼───────────┐
                    │ Does skill involve  │
            ┌───NO──│ external libraries? │──YES──┐
            │       └─────────────────────┘       │
            │                                     │
   ┌────────▼────────┐               ┌───────────▼───────────┐
   │ Skip Context7   │               │ Is knowledge current? │
   │ (Pure logic     │               │ (< 6 months old)      │
   │ skill only)     │               └───────────┬───────────┘
   └─────────────────┘                           │
                                       ┌─────────┴─────────┐
                                       │                   │
                               ┌───────▼───────┐   ┌───────▼───────┐
                               │ YES: Optional │   │ NO: MANDATORY │
                               │ Context7 call │   │ Context7 call │
                               │ (validation)  │   │ (research)    │
                               └───────────────┘   └───────────────┘
```

---

## CODE EXAMPLES

### Example 1: Basic Documentation Lookup

```typescript
// Building a Next.js skill - get latest App Router patterns
const libraryId = await mcp.call("resolve-library-id", {
  libraryName: "next.js",
  query: "Server Components and App Router patterns"
});
// Result: { libraryId: "/vercel/next.js" }

const docs = await mcp.call("query-docs", {
  libraryId: "/vercel/next.js",
  query: "How to implement Server Actions with form handling"
});
// Result: Current documentation with code examples
```

### Example 2: Multi-Library Skill Research

```typescript
// Building a full-stack skill - research all technologies
const technologies = ["next.js", "supabase", "tailwindcss"];

const research = await Promise.all(
  technologies.map(async (tech) => {
    const { libraryId } = await mcp.call("resolve-library-id", {
      libraryName: tech,
      query: skillRequest
    });

    const docs = await mcp.call("query-docs", {
      libraryId,
      query: `Best practices and patterns for ${tech}`
    });

    return { tech, libraryId, docs };
  })
);
```

### Example 3: Validation with Documentation

```typescript
// Validate skill patterns are current
async function validateSkillPatterns(skill: BuiltSkill) {
  const outdatedPatterns: string[] = [];

  for (const pattern of skill.codePatterns) {
    const docs = await mcp.call("query-docs", {
      libraryId: pattern.libraryId,
      query: `Current implementation of ${pattern.name}`
    });

    if (!docs.content.includes(pattern.signature)) {
      outdatedPatterns.push(pattern.name);
    }
  }

  return {
    valid: outdatedPatterns.length === 0,
    outdatedPatterns
  };
}
```

---

## CACHING STRATEGY

| Cache Type | Duration | Invalidation |
|------------|----------|--------------|
| Library IDs | 7 days | Manual or version bump |
| Documentation queries | 24 hours | On build failure |
| Best practices | 3 days | On gate failure |

---

## ERROR HANDLING

| Error | Cause | Resolution |
|-------|-------|------------|
| "Library not found" | Typo or obscure library | Try alternative names |
| "Rate limited" | > 3 calls per question | Wait and retry |
| "Empty response" | Too broad query | Make query more specific |
| "Timeout" | Network or server issue | Retry with backoff |

---

## SECURITY CONSTRAINTS

**NEVER include in queries**:
- API keys or tokens
- Passwords or credentials
- Personal data (PII)
- Proprietary code snippets

---

*Reference Version: 1.0.0 | Last Updated: 2026-01-26*
