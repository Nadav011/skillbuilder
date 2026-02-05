# XML Tag Structure Patterns

> **Purpose**: Semantic XML tags for optimal prompt structure
> **Version**: SINGULARITY FORGE v21.0.0

---

## TAG HIERARCHY BEST PRACTICES

### Hierarchy Levels

```
Level 0: Root containers (documents, instructions, examples)
Level 1: Primary elements (document, step, example)
Level 2: Content elements (source, input, output)
Level 3: Detail elements (rarely needed)
```

### Maximum Nesting Rule

**Recommended maximum: 3 levels** (4 in complex cases)

```xml
<!-- GOOD: Clean 3-level nesting -->
<documents>                           <!-- Level 0 -->
  <document index="1">                <!-- Level 1 -->
    <document_content>Content</document_content>  <!-- Level 2 -->
  </document>
</documents>

<!-- AVOID: Excessive nesting -->
<a><b><c><d><e>Content</e></d></c></b></a>  <!-- Too deep -->
```

---

## DOCUMENT WRAPPING PATTERNS

### Single Document

```xml
<document>
  <source>filename.ts</source>
  <document_content>
  // File content here
  export function example(): void {}
  </document_content>
</document>
```

### Multiple Documents (Indexed)

```xml
<documents>
  <document index="1">
    <source>api.ts</source>
    <document_content>
    export async function fetchUser(id: string) {
      return await db.user.findUnique({ where: { id } });
    }
    </document_content>
  </document>

  <document index="2">
    <source>types.ts</source>
    <document_content>
    export interface User {
      id: string;
      email: string;
      name: string;
    }
    </document_content>
  </document>
</documents>
```

### Document Referencing

Reference documents by index in instructions:

```xml
<instructions>
Analyze the API in document 1 and ensure it matches types in document 2.
</instructions>
```

---

## EXAMPLE TAG STRUCTURES

### Minimal Example

```xml
<example>
  <input>User input text</input>
  <output>Expected response</output>
</example>
```

### Complete Example (Recommended)

```xml
<example type="typical">
  <description>What this example demonstrates</description>
  <input>
  Detailed input that mimics real usage
  </input>
  <output>
  Expected output in exact format required
  </output>
</example>
```

### Example Type Taxonomy

| Type | Purpose | When to Use |
|------|---------|-------------|
| `typical` | Happy path, common case | Always include |
| `edge-case` | Boundary conditions | Include 1-2 |
| `error` | Invalid input handling | Include 1 |
| `complex` | Multi-step scenarios | If applicable |
| `minimal` | Simplest valid input | Optional |

### Full Examples Block

```xml
<examples>
  <example type="typical">
    <description>Standard user creation</description>
    <input>Create user: email="test@example.com", name="Test"</input>
    <output>{"success": true, "userId": "user_123"}</output>
  </example>

  <example type="edge-case">
    <description>Handling optional fields</description>
    <input>Create user: email="test@example.com" (no name)</input>
    <output>{"success": true, "userId": "user_124", "name": null}</output>
  </example>

  <example type="error">
    <description>Invalid email format</description>
    <input>Create user: email="invalid", name="Test"</input>
    <output>{"success": false, "error": "Invalid email format"}</output>
  </example>
</examples>
```

---

## INSTRUCTIONS VS CONTENT SEPARATION

### Clear Separation Rule

**Instructions**: What to do (imperative)
**Content**: What to work with (data)

```xml
<!-- CORRECT: Clear separation -->
<document>
  [Content to analyze - DATA]
</document>

<instructions>
  [What to do with the content - IMPERATIVE]
</instructions>

<query>
  [Specific question - ACTION]
</query>
```

### Anti-Pattern: Mixed Content

```xml
<!-- WRONG: Instructions mixed with content -->
<document>
  Here is the code. Please analyze it for bugs.  <!-- Mixed! -->
  function example() { return null; }
</document>
```

### Instruction Tag Structure

```xml
<instructions>
  <objective>Primary goal in one sentence</objective>

  <steps>
    <step number="1">First action to take</step>
    <step number="2">Second action to take</step>
    <step number="3">Third action to take</step>
  </steps>

  <constraints>
    <constraint>Must use TypeScript</constraint>
    <constraint>Maximum 50 lines</constraint>
    <constraint>No external dependencies</constraint>
  </constraints>

  <output_requirements>
    <format>JSON</format>
    <structure>{"result": "...", "confidence": 0.0}</structure>
  </output_requirements>
</instructions>
```

---

## NESTED TAG GUIDELINES

### When to Nest

| Nest When | Example |
|-----------|---------|
| Grouping related items | `<steps>` contains `<step>` |
| Adding metadata | `<document index="1">` |
| Hierarchical relationships | `<phase>` contains `<task>` |

### When NOT to Nest

| Avoid Nesting When | Do Instead |
|-------------------|------------|
| Single content block | Use flat tag |
| Simple key-value | Use attributes |
| Readability suffers | Flatten structure |

### Flat vs Nested Comparison

```xml
<!-- FLAT: Simple tasks -->
<instructions>
Analyze the code for type safety issues.
Return findings as a numbered list.
</instructions>

<!-- NESTED: Complex workflows -->
<instructions>
  <phase name="analysis">
    <step number="1">Parse the code structure</step>
    <step number="2">Identify type annotations</step>
  </phase>
  <phase name="reporting">
    <step number="3">Format findings</step>
    <step number="4">Prioritize by severity</step>
  </phase>
</instructions>
```

---

## COMMON MISTAKES TO AVOID

### 1. Unclosed Tags

```xml
<!-- WRONG -->
<document>
Content here
<source>file.md

<!-- CORRECT -->
<document>
Content here
</document>
<source>file.md</source>
```

### 2. Inconsistent Attributes

```xml
<!-- WRONG: Different naming -->
<step num="1">First</step>
<step number="2">Second</step>
<step n="3">Third</step>

<!-- CORRECT: Consistent naming -->
<step number="1">First</step>
<step number="2">Second</step>
<step number="3">Third</step>
```

### 3. Over-Nesting

```xml
<!-- WRONG: Excessive depth -->
<wrapper>
  <container>
    <group>
      <item>
        <content>Value</content>
      </item>
    </group>
  </container>
</wrapper>

<!-- CORRECT: Flattened with attributes -->
<item group="main" container="primary">Value</item>
```

### 4. Mixing Inline and Block Content

```xml
<!-- WRONG: Mixed inline -->
<task>Do this <important>critical</important> thing</task>

<!-- CORRECT: Separated -->
<task>
  <action>Do this thing</action>
  <priority>critical</priority>
</task>
```

### 5. Missing Index Attributes

```xml
<!-- WRONG: No way to reference -->
<document>First doc</document>
<document>Second doc</document>

<!-- CORRECT: Indexed for reference -->
<document index="1">First doc</document>
<document index="2">Second doc</document>
```

---

## TEMPLATE EXAMPLES

### Template 1: Simple Analysis

```xml
<document>
[Content to analyze]
</document>

<instructions>
Analyze the document and identify key themes.
</instructions>

<format>
Return a bulleted list of themes with brief explanations.
</format>
```

### Template 2: Code Review

```xml
<documents>
  <document index="1">
    <source>code.ts</source>
    <document_content>[Code to review]</document_content>
  </document>
</documents>

<instructions>
  <objective>Review code for quality issues</objective>
  <focus>Type safety, error handling, performance</focus>
</instructions>

<examples>
  <example type="typical">
    <input>function add(a, b) { return a + b; }</input>
    <output>Issue: Missing type annotations. Fix: function add(a: number, b: number): number</output>
  </example>
</examples>

<query>Review document 1 and provide actionable feedback.</query>
```

### Template 3: Content Generation

```xml
<context>
  <domain>Technical documentation</domain>
  <audience>Senior developers</audience>
  <tone>Professional, concise</tone>
</context>

<instructions>
  <objective>Generate API documentation</objective>
  <steps>
    <step number="1">Analyze function signatures</step>
    <step number="2">Document parameters and returns</step>
    <step number="3">Add usage examples</step>
  </steps>
</instructions>

<examples>
  <example type="typical">
    <input>function getUser(id: string): Promise&lt;User&gt;</input>
    <output>
    ## getUser(id)
    Fetches a user by ID.
    **Parameters:** `id` (string) - User identifier
    **Returns:** Promise&lt;User&gt; - User object
    </output>
  </example>
</examples>

<source_code>
[Code to document]
</source_code>
```

### Template 4: Multi-Phase Workflow

```xml
<workflow>
  <phase name="input" order="1">
    <documents>[Reference materials]</documents>
  </phase>

  <phase name="process" order="2">
    <instructions>
      <step number="1">Parse input</step>
      <step number="2">Transform data</step>
      <step number="3">Validate output</step>
    </instructions>
  </phase>

  <phase name="output" order="3">
    <format>JSON with schema validation</format>
    <examples>[Output examples]</examples>
  </phase>
</workflow>

<query>Execute the workflow on the provided documents.</query>
```

---

## QUICK REFERENCE

### Essential Tags

| Tag | Required Attributes | Purpose |
|-----|---------------------|---------|
| `<document>` | `index` (multi-doc) | Content wrapper |
| `<step>` | `number` | Workflow sequencing |
| `<example>` | `type` (recommended) | I/O demonstration |
| `<constraint>` | None | Limitation rule |

### Attribute Conventions

| Convention | Example |
|------------|---------|
| Lowercase with hyphens | `type="edge-case"` |
| Numbers for sequences | `number="1"` |
| Descriptive status | `status="success"` |

### Tag Closing Rules

1. Every opening tag needs a closing tag
2. Self-closing tags end with `/>` (rare in prompts)
3. Proper nesting: last opened = first closed

---

<!-- XML_STRUCTURE v23.0.0 | Updated: 2026-01-27 -->
