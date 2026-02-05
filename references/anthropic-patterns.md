# Anthropic Official Best Practices

> **Source**: docs.anthropic.com | Official Anthropic documentation
> **Version**: SINGULARITY FORGE v21.0.0
> **Impact**: +30% quality improvement when applied correctly

---

## CORE PRINCIPLE: DOCUMENT POSITION RULE

**Critical Rule**: Long documents TOP, queries BOTTOM = +30% quality improvement.

Claude processes prompts sequentially. Strategic positioning maximizes attention:

| Position | Content Type | Why |
|----------|--------------|-----|
| **TOP** | Long documents, references | Context loads into memory first |
| **MIDDLE** | Instructions, constraints | Applied after context understood |
| **BOTTOM** | Query/question | Fresh in attention window |

```xml
<!-- CORRECT: Documents first, query last -->
<documents>
  [Long reference content here - TOP]
</documents>

<instructions>
  [Task directives here - MIDDLE]
</instructions>

[Specific query here - BOTTOM]
```

```xml
<!-- WRONG: Query before context -->
What is the main theme?  <!-- Query too early - loses context -->

<document>
  [Long document here]
</document>
```

---

## SYSTEM PROMPT RULE

**Critical**: System prompts are for ROLE ONLY, not instructions.

### DO: Role Definition Only

```xml
<system>
You are an expert TypeScript architect with 15 years of experience.
You specialize in Next.js, React, and type-safe systems.
You communicate with precision and provide production-ready code.
</system>
```

### DON'T: Instructions in System

```xml
<!-- BAD: Instructions in system prompt -->
<system>
You are a developer.
Always respond in JSON format.           <!-- INSTRUCTION - move to user -->
First analyze, then suggest.             <!-- WORKFLOW - move to user -->
Here's an example: {...}                 <!-- EXAMPLE - move to user -->
</system>
```

### Why This Matters

- System prompts are cached separately (cost optimization)
- Instructions should be in user messages for flexibility
- Keeps system prompt reusable across different tasks
- Improves prompt caching efficiency by ~90%

---

## XML TAGS USAGE

### Primary Tags and Purposes

| Tag | Purpose | When to Use |
|-----|---------|-------------|
| `<document>` | Reference content | Wrap materials to analyze |
| `<instructions>` | Task directives | Explicit task definition |
| `<examples>` | I/O demonstrations | Show expected behavior |
| `<thinking>` | Chain of thought | Complex reasoning tasks |
| `<answer>` | Final response | Clear output boundary |
| `<context>` | Background info | Domain knowledge setup |
| `<constraints>` | Limitations | Rules and boundaries |

### Document Wrapping Pattern

```xml
<documents>
  <document index="1">
    <source>api-spec.yaml</source>
    <document_content>
    openapi: 3.0.0
    info:
      title: User API
      version: 1.0.0
    paths:
      /users:
        get:
          summary: List all users
    </document_content>
  </document>

  <document index="2">
    <source>requirements.md</source>
    <document_content>
    # Requirements
    - Must support pagination
    - Must validate input with Zod
    </document_content>
  </document>
</documents>
```

---

## EXAMPLES SECTION (3-5 DIVERSE EXAMPLES)

**Optimal Count**: 3-5 examples provides best cost/quality ratio.

| Count | Quality | Notes |
|-------|---------|-------|
| 0 | Unreliable | Model guesses format |
| 1-2 | Limited | May not generalize |
| **3-5** | **Optimal** | Best cost/quality ratio |
| 6+ | Diminishing | Wastes tokens |

### Complete Examples Structure

```xml
<examples>
  <!-- Example 1: Typical/Happy Path -->
  <example type="typical">
    <description>Standard successful operation</description>
    <input>
    User requests: Create a user with email "john@example.com"
    </input>
    <output>
    {
      "success": true,
      "data": {
        "id": "user_123",
        "email": "john@example.com",
        "createdAt": "2026-01-26T10:00:00Z"
      }
    }
    </output>
  </example>

  <!-- Example 2: Edge Case -->
  <example type="edge-case">
    <description>Handling ambiguous or partial input</description>
    <input>
    User requests: Create a user (no email provided)
    </input>
    <output>
    {
      "success": false,
      "error": {
        "code": "VALIDATION_ERROR",
        "message": "Email is required",
        "field": "email"
      }
    }
    </output>
  </example>

  <!-- Example 3: Error Case -->
  <example type="error">
    <description>Invalid input handling</description>
    <input>
    User requests: Create a user with email "invalid-email"
    </input>
    <output>
    {
      "success": false,
      "error": {
        "code": "VALIDATION_ERROR",
        "message": "Invalid email format",
        "field": "email"
      }
    }
    </output>
  </example>

  <!-- Example 4: Complex Case -->
  <example type="complex">
    <description>Multi-step operation with relationships</description>
    <input>
    User requests: Create a user and assign to team "engineering"
    </input>
    <output>
    {
      "success": true,
      "data": {
        "user": {
          "id": "user_456",
          "email": "jane@example.com"
        },
        "teamAssignment": {
          "teamId": "team_eng",
          "role": "member"
        }
      }
    }
    </output>
  </example>

  <!-- Example 5: Boundary Case -->
  <example type="boundary">
    <description>Handling system limits</description>
    <input>
    User requests: Create user (max users reached)
    </input>
    <output>
    {
      "success": false,
      "error": {
        "code": "LIMIT_EXCEEDED",
        "message": "Maximum user limit reached",
        "limit": 1000,
        "current": 1000
      }
    }
    </output>
  </example>
</examples>
```

### Example Diversity Checklist

- [ ] Typical/happy path case
- [ ] Edge case (ambiguous input)
- [ ] Error case (invalid input)
- [ ] Complex case (multi-step)
- [ ] Boundary case (limits)

---

## CHAIN OF THOUGHT (CoT)

### When to Use CoT

| Scenario | Use CoT? | Reason |
|----------|----------|--------|
| Complex reasoning | **Yes** | Improves accuracy significantly |
| Multi-step tasks | **Yes** | Tracks progress and state |
| Math/logic problems | **Yes** | Shows work, catches errors |
| Simple retrieval | No | Adds unnecessary tokens |
| Factual questions | No | Direct answer sufficient |
| Creative writing | Optional | Can help with planning |

### CoT Structure with Tags

```xml
<thinking>
Step 1: Understand the problem
- What is being asked? Generate a TypeScript function
- What constraints exist? Must be type-safe, <30 lines

Step 2: Analyze the inputs
- Input type: User object with optional fields
- Output type: Validated user or error

Step 3: Plan the approach
- Use Zod for validation
- Return Result<T,E> pattern
- Handle all edge cases

Step 4: Execute
- Define schema first
- Create validation function
- Add error handling
</thinking>

<answer>
```typescript
import { z } from 'zod';
import { Result, ok, err } from 'neverthrow';

const UserSchema = z.object({
  email: z.string().email(),
  name: z.string().min(1).max(100),
});

type User = z.infer<typeof UserSchema>;
type ValidationError = { field: string; message: string };

export function validateUser(input: unknown): Result<User, ValidationError[]> {
  const result = UserSchema.safeParse(input);
  if (!result.success) {
    const errors = result.error.issues.map(i => ({
      field: i.path.join('.'),
      message: i.message,
    }));
    return err(errors);
  }
  return ok(result.data);
}
```
</answer>
```

---

## PREFILLING TECHNIQUE

### JSON Output Forcing

Start Claude's response with `{` to force valid JSON:

```xml
<user>
Extract entities from: "Apple released iPhone 15 in California"
Return as JSON with: company, product, location fields.
</user>

<assistant>{</assistant>  <!-- Prefill forces JSON output -->
```

**Result**: Claude continues from `{` producing valid JSON.

### Structured Output Forcing

Start with markdown structure to enforce format:

```xml
<user>
Analyze this code and provide feedback.
</user>

<assistant>## Code Analysis

### Strengths
- </assistant>  <!-- Prefill enforces structure -->
```

### Format Specification Tag

```xml
<format>
Respond in this exact JSON structure:
{
  "summary": "string - one sentence overview",
  "issues": ["array of identified problems"],
  "suggestions": ["array of improvement recommendations"],
  "severity": "low | medium | high | critical"
}
</format>
```

---

## PROMPT CACHING (90% COST SAVINGS)

### Caching Strategy

**Rule**: Static content FIRST (cached), dynamic content LAST (not cached).

| Content Type | Position | Cached | Savings |
|--------------|----------|--------|---------|
| System prompt | First | Yes | ~90% |
| Reference docs | Early | Yes | ~90% |
| Examples | Middle | Yes | ~90% |
| User query | Last | No | 0% |

### Cache-Optimized Structure

```xml
<!-- CACHED SECTION (static - same across requests) -->
<system>
You are an expert code reviewer specializing in TypeScript.
</system>

<documents>
  <!-- Large reference documentation - CACHED -->
  <document index="1">
    <source>style-guide.md</source>
    <document_content>[500+ lines of style guide]</document_content>
  </document>
</documents>

<examples>
  <!-- Standard examples - CACHED -->
  <example type="typical">[Example content]</example>
  <example type="error">[Example content]</example>
</examples>

<!-- NON-CACHED SECTION (dynamic - changes per request) -->
<user_code>
// User's specific code to review - NOT CACHED
function processData(data: any) {
  return data.map(x => x.value);
}
</user_code>

<query>
Review this code for type safety issues.
</query>
```

### Cache Optimization Tips

- Keep static content **identical** across requests
- Place all variable content at the **end**
- Use consistent formatting in static sections
- Anthropic caches at natural breakpoints

---

## COMPLETE PROMPT STRUCTURE TEMPLATE

### Production-Ready Template

```xml
<system>
[ROLE ONLY - Identity and expertise]
You are an expert [domain] specialist with [N] years of experience.
You specialize in [technologies/domains].
You communicate [style: precisely/concisely/thoroughly].
</system>

<user>
<!-- SECTION 1: Context (TOP - for caching) -->
<documents>
  <document index="1">
    <source>[filename]</source>
    <document_content>
    [Reference content placed at TOP for optimal context loading]
    </document_content>
  </document>
</documents>

<!-- SECTION 2: Instructions (MIDDLE) -->
<instructions>
  <objective>[Primary goal - single clear sentence]</objective>

  <steps>
    <step number="1">[First action]</step>
    <step number="2">[Second action]</step>
    <step number="3">[Third action]</step>
  </steps>

  <constraints>
    <constraint>[Limitation 1]</constraint>
    <constraint>[Limitation 2]</constraint>
  </constraints>
</instructions>

<!-- SECTION 3: Examples (MIDDLE) -->
<examples>
  <example type="typical">
    <input>[Typical input]</input>
    <output>[Expected output]</output>
  </example>
  <example type="edge-case">
    <input>[Edge case input]</input>
    <output>[Expected output]</output>
  </example>
  <example type="error">
    <input>[Error case input]</input>
    <output>[Expected output]</output>
  </example>
</examples>

<!-- SECTION 4: Format Specification -->
<format>
[Exact output format specification]
</format>

<!-- SECTION 5: Query (BOTTOM - fresh in attention) -->
[Specific task/question to perform NOW]
</user>

<!-- Optional: Prefill for format control -->
<assistant>[Starting characters to force format]</assistant>
```

---

## VERIFICATION CHECKLIST

Before deploying any prompt:

- [ ] Documents positioned at **TOP**
- [ ] Query positioned at **BOTTOM**
- [ ] System prompt contains **ROLE ONLY**
- [ ] Instructions in **user message**
- [ ] **3-5 diverse examples** included
- [ ] XML tags **properly closed**
- [ ] Static content **first** for caching
- [ ] Format specification **explicit**
- [ ] Prefill used if format-critical

---

<!-- ANTHROPIC_PATTERNS v23.0.0 | Updated: 2026-01-27 -->
