# Templates Library Reference

> Prebuilt starter templates for rapid skill development across all complexity levels.

---

## Overview

### Template Hierarchy

```
┌─────────────────────────────────────────────────────────────────┐
│                      TEMPLATE HIERARCHY                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  STARTER (L1)          DOMAIN (L2)           ADVANCED (L3)      │
│  ┌──────────┐         ┌──────────┐          ┌──────────┐        │
│  │ Basic    │    ──▶  │ API      │    ──▶   │ Multi-   │        │
│  │ Agent    │         │ Database │          │ Agent    │        │
│  │ Validate │         │ UI       │          │ Self-    │        │
│  │ Command  │         │ Testing  │          │ Improve  │        │
│  └──────────┘         └──────────┘          └──────────┘        │
│       │                    │                      │              │
│       ▼                    ▼                      ▼              │
│   <100 lines          100-300 lines          300-500 lines      │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

### Template Selection Matrix

| Need | Template | Lines | Structure |
|------|----------|-------|-----------|
| Simple automation | Basic | <100 | SKILL.md only |
| Agent capability | Agent | <150 | SKILL.md + references/ |
| Input validation | Validation | <100 | SKILL.md only |
| User command | Command | <50 | Single file |
| API integration | API Domain | <250 | SKILL.md + refs |
| Database work | Database Domain | <300 | SKILL.md + refs |
| UI components | UI Domain | <250 | SKILL.md + refs |
| Testing suite | Testing Domain | <300 | SKILL.md + refs |
| Multiple agents | Multi-Agent | <400 | Complex structure |
| Self-improvement | Self-Improve | <500 | Complex + learning |

---

## Starter Templates

### T1: Basic Skill Template

```yaml
# skill.yaml
name: basic-skill-template
version: "1.0.0"
description: |
  Basic skill template for simple automation tasks.
  Use when: Single input, single output, no complex logic.
```

```markdown
# BASIC SKILL v1.0

> Single-purpose automation skill.

## PURPOSE
[Describe what this skill does in 1-2 sentences]

## WORKFLOW
1. **Parse** - Extract input parameters
2. **Validate** - Check required fields
3. **Execute** - Run core operation
4. **Output** - Return formatted result

## INPUT
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| input | string | Yes | Primary input |

## OUTPUT
| Field | Type | Description |
|-------|------|-------------|
| result | string | Operation output |
| success | boolean | Completion status |
```

### T2: Agent Skill Template

```markdown
# AGENT SKILL v1.0

> Context-aware agent with domain expertise.

## PURPOSE
[Domain expertise description]

## TRIGGER CONDITIONS
Activate when user context includes:
- [Condition 1]
- [Condition 2]

## CAPABILITIES
| Capability | Description | Tools Used |
|------------|-------------|------------|
| Analyze | [Description] | Read, Grep |
| Transform | [Description] | Read, Write |

## WORKFLOW

### Phase 1: Context Analysis
1. Read relevant files
2. Extract key patterns
3. Build mental model

### Phase 2: Reasoning
1. Apply domain knowledge
2. Consider edge cases
3. Formulate approach

### Phase 3: Execution
1. Execute planned actions
2. Validate results
3. Report findings

## DECISION TREE
```
Is context sufficient?
├── No → Request clarification
└── Yes → Determine action type
    ├── Analysis → Use read-only tools
    └── Modification → Confirm before changes
```
```

### T3: Validation Skill Template

```markdown
# VALIDATION SKILL v1.0

> Schema validation and data quality enforcement.

## SCHEMAS

### Input Schema
```typescript
const InputSchema = z.object({
  data: z.unknown(),
  schema: z.string(),
  options: z.object({
    strict: z.boolean().default(true),
  }).optional(),
});
```

## VALIDATION RULES
| Rule | Description | Error Code |
|------|-------------|------------|
| Required | Field must exist | REQUIRED |
| Type | Must match expected type | TYPE_MISMATCH |
| Format | Must match pattern | INVALID_FORMAT |
| Range | Must be within bounds | OUT_OF_RANGE |
```

### T4: Command Template

```markdown
---
description: "Execute [action] on [target]. Usage: /command [args]"
---

# /COMMAND

Execute [action] with provided arguments.

## USAGE
```
/command <required_arg> [optional_arg]
```

## ARGUMENTS
| Arg | Type | Required | Description |
|-----|------|----------|-------------|
| required_arg | string | Yes | [Description] |

## EXECUTION
1. Parse arguments
2. Validate inputs
3. Execute action
4. Return result
```

---

## Domain-Specific Templates

### T5: API Integration Template

```markdown
# API INTEGRATION v1.0

> External API integration with retry logic.

## CONFIGURATION
```typescript
interface APIConfig {
  baseUrl: string;
  auth: { type: 'bearer' | 'api_key'; credentials: string; };
  timeout: number;
  retries: { count: number; backoff: 'linear' | 'exponential'; };
}
```

## ENDPOINTS
| Endpoint | Method | Purpose | Rate Limit |
|----------|--------|---------|------------|
| /resource | GET | Fetch resources | 100/min |
| /resource | POST | Create resource | 50/min |

## ERROR HANDLING
| Status | Meaning | Action |
|--------|---------|--------|
| 400 | Bad Request | Log error, don't retry |
| 401 | Unauthorized | Refresh token, retry |
| 429 | Rate Limited | Wait, retry with backoff |
| 500 | Server Error | Retry with backoff |
```

### T6: Database Template

```markdown
# DATABASE SKILL v1.0

> Database operations with type-safe queries.

## QUERY PATTERNS

### Select with Type Safety
```typescript
async function getUser(id: string): Promise<Result<User, NotFoundError>> {
  const { data, error } = await supabase
    .from('users')
    .select('id, email, created_at')
    .eq('id', id)
    .single();

  if (error) return err(new NotFoundError('User'));
  return ok(data as User);
}
```

## SECURITY
| Rule | Implementation |
|------|----------------|
| Use RLS | `ALTER TABLE users ENABLE ROW LEVEL SECURITY;` |
| Parameterize | Never concatenate user input |
| Least privilege | Use read-only connections where possible |
```

### T7: UI Component Template

```markdown
# UI COMPONENT SKILL v1.0

> React component generation with RTL and responsive support.

## BASE COMPONENT TEMPLATE
```tsx
export const ComponentName = forwardRef<HTMLDivElement, ComponentNameProps>(
  ({ className, variant = 'default', size = 'md', children, ...props }, ref) => {
    return (
      <div
        ref={ref}
        className={cn(
          'rounded-lg border transition-colors',
          // RTL-safe spacing (ms-/me- instead of ml-/mr-)
          'ps-4 pe-4',
          // Touch target
          'min-h-11',
          className
        )}
        {...props}
      >
        {children}
      </div>
    );
  }
);
```

## RTL REQUIREMENTS (Law #5)
| Use | Instead Of |
|-----|------------|
| `ms-4`, `me-4` | `ml-4`, `mr-4` |
| `ps-4`, `pe-4` | `pl-4`, `pr-4` |
| `text-start` | `text-left` |
```

---

## Advanced Templates

### T8: Multi-Agent Template

```markdown
# MULTI-AGENT SKILL v1.0

> Orchestrated multi-agent system.

## AGENT ARCHITECTURE
```
┌─────────────────────────────────────────────────────────────────┐
│                      ORCHESTRATOR                                │
├─────────────────────────────────────────────────────────────────┤
│  ┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐      │
│  │ PLANNER │───▶│ EXECUTOR│───▶│ REVIEWER│───▶│ REPORTER│      │
│  └─────────┘    └─────────┘    └─────────┘    └─────────┘      │
└─────────────────────────────────────────────────────────────────┘
```

## COORDINATION PATTERNS
- Sequential Pipeline: Input → Agent1 → Result1 → Agent2 → Output
- Parallel Execution: Input splits, agents work, results merge
- Feedback Loop: Agent1 → Agent2 → Review → Revision → Agent1
```

### T9: Self-Improving Template

```markdown
# SELF-IMPROVING SKILL v1.0

> Adaptive skill that learns from execution history.

## LEARNING LOOP
```
EXECUTE ──▶ EVALUATE ──▶ LEARN ──▶ ADAPT ──▶ EXECUTE
```

## FEEDBACK COLLECTION
```typescript
interface ExecutionFeedback {
  execution_id: string;
  success: boolean;
  user_satisfaction?: 1 | 2 | 3 | 4 | 5;
  duration_ms: number;
  token_count: number;
  corrections_needed: number;
}
```

## GUARDRAILS
| Guardrail | Purpose |
|-----------|---------|
| Rollback trigger | Revert if quality drops >10% |
| Change rate limit | Max 1 adaptation per hour |
| Human review | Major changes require approval |
```

---

## Template Selection Guide

### Decision Matrix

```
What type of task?
├── User command → T4: Command Template
├── Simple automation → T1: Basic Template
├── Agent capability → T2: Agent Template
│   └── What domain?
│       ├── API work → T5: API Template
│       ├── Database → T6: Database Template
│       ├── UI work → T7: UI Template
│       └── Testing → T8: Testing Template
├── Validation → T3: Validation Template
└── Complex workflow
    ├── Multiple agents → T8: Multi-Agent Template
    └── Learning system → T9: Self-Improving Template
```

### Complexity Indicators

| Indicator | Simple (T1-T4) | Domain (T5-T7) | Advanced (T8-T9) |
|-----------|----------------|----------------|------------------|
| Steps | 1-3 | 3-7 | 7+ |
| Tools | 1-2 | 2-5 | 5+ |
| Conditions | Linear | Branching | Complex DAG |
| Learning | None | Minimal | Continuous |

---

<!-- TEMPLATES_LIBRARY v23.0.0 | Updated: 2026-01-27 -->
