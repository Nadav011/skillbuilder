# Interactive Wizard Reference

> **SINGULARITY FORGE v21.0.0** | Guided Skill Creation for All Levels
> Step-by-step wizard for building GOD-TIER skills

---

## WIZARD OVERVIEW

```
+------------------------------------------------------------------+
|                    INTERACTIVE WIZARD MODE                        |
+------------------------------------------------------------------+
|  Guided skill creation through structured questions               |
|  Perfect for: Beginners, complex skills, team collaboration       |
|  Outcome: Complete, validated skill ready for deployment          |
+------------------------------------------------------------------+
```

### Wizard Benefits

| Benefit | Description |
|---------|-------------|
| Guided Process | No guessing, clear next steps |
| Validation | Each step verified before proceeding |
| Best Practices | Defaults based on proven patterns |
| Documentation | Auto-generated docs from answers |
| Consistency | Same quality every time |

---

## WIZARD FLOW ARCHITECTURE

```
                    START WIZARD
                         |
                         v
        +----------------+----------------+
        |                                 |
        v                                 v
   [Quick Mode]                    [Full Mode]
   (5 questions)                   (15+ questions)
        |                                 |
        v                                 v
+-------+-------+              +----------+----------+
| Phase 1: Core |              | Phase 1: Research   |
| (Name, Type)  |              | Phase 2: Core       |
+-------+-------+              | Phase 3: Structure  |
        |                      | Phase 4: Patterns   |
        v                      | Phase 5: Testing    |
+-------+-------+              | Phase 6: Security   |
| Phase 2: Gen  |              | Phase 7: Deploy     |
| (Auto-fill)   |              +----------+----------+
+-------+-------+                        |
        |                                v
        +----------------+---------------+
                         |
                         v
                  VALIDATION GATE
                         |
                         v
                    GENERATION
                         |
                         v
                      OUTPUT
```

---

## QUICK MODE (5 Questions)

### For Simple Skills (<150 lines)

```markdown
## Quick Mode Questions

### Q1: Skill Name
"What should this skill be called?"
- Format: lowercase-kebab-case
- Examples: "git-commit", "deploy-vercel", "rtl-check"
- Validation: ^[a-z][a-z0-9-]*[a-z0-9]$

### Q2: Skill Type
"What type of skill is this?"
Options:
  1. Automation - Runs commands/scripts
  2. Analysis - Examines code/data
  3. Generation - Creates files/content
  4. Workflow - Multi-step process
  5. Integration - Connects services

### Q3: Primary Trigger
"What command or phrase activates this skill?"
- Format: /command-name or natural language
- Examples: "/deploy", "review this PR", "/test"

### Q4: Core Action
"In one sentence, what does this skill do?"
- Format: Active voice, specific action
- Example: "Commits staged changes with conventional commit format"

### Q5: Output Type
"What does the skill produce?"
Options:
  1. Terminal output - Commands, logs
  2. File(s) - Code, configs, docs
  3. Report - Analysis, audit
  4. Action - External service call
  5. Hybrid - Multiple outputs
```

### Quick Mode Defaults

| Setting | Default Value | Override Available |
|---------|---------------|-------------------|
| Lines limit | 150 | Yes |
| Token budget | Light | Yes |
| Testing | Basic | Yes |
| Security | Standard | Yes |
| Documentation | Auto-gen | Yes |

---

## FULL MODE (15+ Questions)

### Phase 1: Research Setup (2 Questions)

```markdown
## Phase 1: Research Setup

### Q1.1: Domain Classification
"What domain does this skill belong to?"
Options:
  1. AI/LLM - Prompt engineering, model interaction
  2. DevOps - CI/CD, deployment, infrastructure
  3. Security - Auditing, scanning, compliance
  4. Frontend - UI, components, styling
  5. Backend - API, database, services
  6. Mobile - Flutter, React Native, Capacitor
  7. Testing - Unit, E2E, visual
  8. Documentation - Docs, comments, READMEs
  9. Custom - Specify your domain

### Q1.2: Research Depth
"How much research is needed?"
Options:
  1. Light - Well-established patterns (15 min)
  2. Standard - Recent best practices needed (30 min)
  3. Deep - Cutting-edge or security-critical (60+ min)

Default: Based on domain (AI/Security = Deep)
```

### Phase 2: Core Identity (4 Questions)

```markdown
## Phase 2: Core Identity

### Q2.1: Skill Name
"What should this skill be called?"
[Same as Quick Mode Q1]

### Q2.2: Version
"What version should this be?"
- Default: 1.0.0
- Format: SemVer (major.minor.patch)
- Tip: Use 0.x.x for experimental skills

### Q2.3: Description
"Describe this skill in 2-3 sentences."
Prompts:
  - What problem does it solve?
  - Who is the target user?
  - What makes it unique?

### Q2.4: Category Tags
"Select all applicable tags:"
[ ] automation
[ ] analysis
[ ] security
[ ] performance
[ ] testing
[ ] deployment
[ ] documentation
[ ] refactoring
[ ] monitoring
[ ] integration
```

### Phase 3: Structure Design (3 Questions)

```markdown
## Phase 3: Structure Design

### Q3.1: Skill Complexity
"How complex is this skill?"
Options:
  1. Simple - Single action, <100 lines
  2. Standard - Multiple steps, 100-300 lines
  3. Complex - Multi-phase, 300-500 lines
  4. Enterprise - Full system, 500+ lines

### Q3.2: Input Requirements
"What inputs does the skill need?"
For each input, specify:
  - Name: [input_name]
  - Type: string | number | boolean | file | selection
  - Required: yes | no
  - Default: [value or none]
  - Validation: [pattern or none]

Example:
  - Name: project_path
  - Type: string
  - Required: yes
  - Default: current directory
  - Validation: valid directory path

### Q3.3: Output Specification
"What outputs does the skill produce?"
For each output, specify:
  - Type: file | terminal | report | action
  - Format: text | json | markdown | html
  - Location: stdout | file path | memory
```

### Phase 4: Pattern Selection (3 Questions)

```markdown
## Phase 4: Pattern Selection

### Q4.1: Prompt Pattern
"What prompting pattern should be used?"
Options:
  1. Direct Instruction - Clear, specific commands
  2. Chain of Thought - Step-by-step reasoning
  3. Few-Shot - Include examples
  4. Role-Based - Assume expert persona
  5. Structured Output - Enforce format (XML/JSON)
  6. Hybrid - Combine patterns

Default recommendation based on skill type:
  - Automation → Direct Instruction
  - Analysis → Chain of Thought
  - Generation → Few-Shot + Structured
  - Workflow → Role-Based + CoT

### Q4.2: Error Handling Strategy
"How should errors be handled?"
Options:
  1. Fail Fast - Stop on first error
  2. Collect All - Report all errors at end
  3. Auto-Recover - Attempt fixes automatically
  4. Interactive - Ask user for resolution
  5. Graceful Degrade - Continue with warnings

### Q4.3: State Management
"Does this skill need to maintain state?"
Options:
  1. Stateless - Each run independent
  2. Session State - State within conversation
  3. Persistent State - State across sessions (Memory MCP)
  4. File State - State in project files
```

### Phase 5: Testing Configuration (2 Questions)

```markdown
## Phase 5: Testing Configuration

### Q5.1: Test Coverage Level
"What level of testing is required?"
Options:
  1. Basic - Happy path only
  2. Standard - Happy path + common errors
  3. Comprehensive - All paths + edge cases
  4. Exhaustive - Full coverage + fuzzing

Default: Based on complexity
  - Simple → Basic
  - Standard → Standard
  - Complex/Enterprise → Comprehensive

### Q5.2: Golden Set Definition
"Define expected outputs for test cases:"
For each test case:
  - Name: [test_name]
  - Input: [test_input]
  - Expected Output: [expected_result]
  - Validation: exact | contains | regex | semantic

Example:
  - Name: "basic_commit"
  - Input: "commit with message 'fix bug'"
  - Expected: Contains "git commit -m"
  - Validation: contains
```

### Phase 6: Security Requirements (2 Questions)

```markdown
## Phase 6: Security Requirements

### Q6.1: Security Level
"What security level is required?"
Options:
  1. Standard - Basic validation
  2. Enhanced - Input sanitization + output filtering
  3. Strict - Full security audit compliance
  4. Critical - Zero-trust, encryption, audit logging

Default: Based on domain
  - Security domain → Strict minimum
  - User data handling → Enhanced minimum
  - Internal tools → Standard

### Q6.2: Sensitive Data Handling
"Does this skill handle sensitive data?"
Options:
  1. No sensitive data
  2. Credentials/API keys - Never log, mask in output
  3. PII - Anonymization required
  4. Financial data - Encryption + audit trail
  5. Multiple types - Specify each
```

### Phase 7: Deployment Settings (2 Questions)

```markdown
## Phase 7: Deployment Settings

### Q7.1: Deployment Target
"Where will this skill run?"
Options:
  1. Local only - Developer machine
  2. CI/CD - Pipeline integration
  3. Server - API/service deployment
  4. Multi-environment - All of above

### Q7.2: Dependencies
"List external dependencies:"
For each dependency:
  - Name: [package/tool]
  - Version: [version constraint]
  - Required: yes | no (with fallback)

Example:
  - Name: gh (GitHub CLI)
  - Version: >=2.0.0
  - Required: yes (for PR creation)
```

---

## QUESTION TEMPLATES

### Text Input Template

```yaml
question:
  id: "q_[phase]_[number]"
  type: "text"
  prompt: "[Question text]"
  help: "[Additional context]"
  validation:
    pattern: "[regex]"
    min_length: [N]
    max_length: [N]
  default: "[value]"
  examples:
    - "[example 1]"
    - "[example 2]"
```

### Selection Template

```yaml
question:
  id: "q_[phase]_[number]"
  type: "selection"
  prompt: "[Question text]"
  options:
    - value: "[option_value]"
      label: "[Display text]"
      description: "[Help text]"
      recommended: [true|false]
  multi_select: [true|false]
  default: "[value]"
```

### Confirmation Template

```yaml
question:
  id: "q_[phase]_[number]"
  type: "confirm"
  prompt: "[Yes/No question]"
  default: [true|false]
  warning: "[Warning if choosing non-default]"
```

### Complex Input Template

```yaml
question:
  id: "q_[phase]_[number]"
  type: "complex"
  prompt: "[Question text]"
  fields:
    - name: "[field_name]"
      type: "text|number|select"
      required: [true|false]
  repeatable: [true|false]
  min_items: [N]
  max_items: [N]
```

---

## PROGRESS INDICATORS

### Visual Progress Bar

```
SKILL CREATION WIZARD
======================

Phase: [2/7] Core Identity
Progress: [████████░░░░░░░░░░░░] 40%

Current Question: 2.3 of 4
[Description]

Phase Completion:
  ✓ Phase 1: Research Setup
  → Phase 2: Core Identity (in progress)
  ○ Phase 3: Structure Design
  ○ Phase 4: Pattern Selection
  ○ Phase 5: Testing Configuration
  ○ Phase 6: Security Requirements
  ○ Phase 7: Deployment Settings
```

### Phase Status Indicators

| Symbol | Meaning |
|--------|---------|
| ✓ | Phase complete, validated |
| → | Current phase |
| ○ | Not started |
| ⚠ | Complete with warnings |
| ✗ | Failed validation |

---

## VALIDATION AT EACH STEP

### Real-Time Validation

```typescript
interface ValidationResult {
  valid: boolean;
  errors: ValidationError[];
  warnings: ValidationWarning[];
  suggestions: string[];
}

interface ValidationError {
  field: string;
  message: string;
  fix_suggestion?: string;
}

// Validation flow
async function validateStep(
  questionId: string,
  answer: unknown
): Promise<ValidationResult> {
  const rules = getValidationRules(questionId);
  const errors: ValidationError[] = [];
  const warnings: ValidationWarning[] = [];

  // Run all validators
  for (const rule of rules) {
    const result = await rule.validate(answer);
    if (!result.valid) {
      errors.push(result.error);
    }
    if (result.warning) {
      warnings.push(result.warning);
    }
  }

  return {
    valid: errors.length === 0,
    errors,
    warnings,
    suggestions: generateSuggestions(answer, questionId)
  };
}
```

### Validation Rules by Question Type

| Type | Validations |
|------|-------------|
| Skill Name | Format, uniqueness, reserved words |
| Version | SemVer format |
| Description | Min length, clarity check |
| Inputs | Type compatibility, required fields |
| Outputs | Valid formats, path validity |
| Dependencies | Existence check, version validity |

### Phase-End Validation

```markdown
## Phase Validation Gate

Before proceeding to next phase:

### Completeness Check
- [ ] All required questions answered
- [ ] No validation errors
- [ ] Warnings acknowledged

### Consistency Check
- [ ] Inputs match declared complexity
- [ ] Security level matches data handling
- [ ] Test coverage matches risk level

### Proceed?
  [Continue to Phase X] [Go Back] [Save & Exit]
```

---

## DEFAULT VALUES AND RECOMMENDATIONS

### Smart Defaults Engine

```typescript
interface SmartDefaults {
  // Based on previous answers
  inferFromContext(answers: Record<string, unknown>): Defaults;

  // Based on skill type
  getTypeDefaults(skillType: SkillType): Defaults;

  // Based on domain
  getDomainDefaults(domain: Domain): Defaults;
}

// Example inference
function inferDefaults(answers: WizardAnswers): Defaults {
  const defaults: Defaults = {};

  // If AI domain, default to deep research
  if (answers.domain === 'ai') {
    defaults.researchDepth = 'deep';
    defaults.promptPattern = 'structured';
  }

  // If security-related, increase defaults
  if (answers.tags?.includes('security')) {
    defaults.securityLevel = 'strict';
    defaults.testCoverage = 'comprehensive';
  }

  // If simple complexity, reduce requirements
  if (answers.complexity === 'simple') {
    defaults.testCoverage = 'basic';
    defaults.documentation = 'minimal';
  }

  return defaults;
}
```

### Recommendation Display

```
RECOMMENDATION

Based on your answers:
  - Domain: Security
  - Complexity: Complex
  - Handles: API Keys

We recommend:
  ┌─────────────────────────────────────────────┐
  │ Security Level: STRICT (elevated from Standard) │
  │ Reason: Handles sensitive credentials       │
  │                                             │
  │ Test Coverage: COMPREHENSIVE                │
  │ Reason: Security-critical functionality     │
  │                                             │
  │ Research Depth: DEEP                        │
  │ Reason: Security domain requires current    │
  │         knowledge of vulnerabilities        │
  └─────────────────────────────────────────────┘

Accept recommendations? [Yes] [Customize]
```

---

## EXAMPLE WIZARD SESSION

### Complete Session Transcript

```
╔══════════════════════════════════════════════════════════════════╗
║           SINGULARITY FORGE - SKILL CREATION WIZARD              ║
╚══════════════════════════════════════════════════════════════════╝

Welcome! Let's create a GOD-TIER skill together.

? Select wizard mode:
  ❯ Full Mode (Comprehensive - 15+ questions)
    Quick Mode (Fast - 5 questions)

[User selects: Full Mode]

═══════════════════════════════════════════════════════════════════
PHASE 1: RESEARCH SETUP [1/7]
═══════════════════════════════════════════════════════════════════

? Q1.1: What domain does this skill belong to?
  1. AI/LLM
  2. DevOps
  ❯ 3. Security
  4. Frontend
  5. Backend
  6. Mobile
  7. Testing
  8. Documentation
  9. Custom

[User selects: 3. Security]

? Q1.2: How much research is needed?
  Based on Security domain, we recommend: DEEP

  1. Light (15 min)
  2. Standard (30 min)
  ❯ 3. Deep (60+ min) [RECOMMENDED]

[User selects: 3. Deep]

✓ Phase 1 Complete
  - Domain: Security
  - Research: Deep

═══════════════════════════════════════════════════════════════════
PHASE 2: CORE IDENTITY [2/7]
═══════════════════════════════════════════════════════════════════

? Q2.1: What should this skill be called?
  Format: lowercase-kebab-case
  Example: security-audit, rls-check

> rls-validator

✓ Valid name: rls-validator

? Q2.2: What version should this be?
  Default: 1.0.0

> [Enter to accept default]

Using: 1.0.0

? Q2.3: Describe this skill in 2-3 sentences.
> Validates Supabase Row Level Security policies against common
> vulnerabilities and misconfigurations. Generates a detailed report
> with remediation steps for any issues found.

✓ Description accepted (42 words)

? Q2.4: Select all applicable tags:
  ❯◉ security
   ◉ analysis
   ◯ automation
   ◉ database
   ◯ performance
   ◯ testing
   ◯ deployment
   ◯ documentation
   ◯ refactoring
   ◯ monitoring
   ◯ integration

[User selects: security, analysis, database]

✓ Phase 2 Complete
  - Name: rls-validator
  - Version: 1.0.0
  - Tags: security, analysis, database

═══════════════════════════════════════════════════════════════════
PHASE 3: STRUCTURE DESIGN [3/7]
═══════════════════════════════════════════════════════════════════

? Q3.1: How complex is this skill?
  1. Simple (<100 lines)
  2. Standard (100-300 lines)
  ❯ 3. Complex (300-500 lines)
  4. Enterprise (500+ lines)

[User selects: 3. Complex]

? Q3.2: What inputs does the skill need?

  Input 1:
  ? Name: project_id
  ? Type: string
  ? Required: yes
  ? Default: (none)
  ? Validation: Supabase project ID format

  ? Add another input? [Y/n] Y

  Input 2:
  ? Name: tables
  ? Type: selection (multi)
  ? Required: no
  ? Default: all tables
  ? Options: [Fetched from schema]

  ? Add another input? [Y/n] n

? Q3.3: What outputs does the skill produce?
  ❯◉ Report (Markdown)
   ◉ Terminal output
   ◯ File changes
   ◯ External action

✓ Phase 3 Complete
  - Complexity: Complex
  - Inputs: 2 (project_id, tables)
  - Outputs: Report + Terminal

[... Phases 4-7 continue similarly ...]

═══════════════════════════════════════════════════════════════════
FINAL VALIDATION
═══════════════════════════════════════════════════════════════════

Validating skill configuration...

✓ Name format valid
✓ All required fields complete
✓ Security level appropriate for domain
✓ Test coverage matches complexity
✓ No conflicting settings

SKILL SUMMARY
┌─────────────────────────────────────────────────┐
│ Name:        rls-validator                      │
│ Version:     1.0.0                              │
│ Domain:      Security                           │
│ Complexity:  Complex                            │
│ Security:    Strict                             │
│ Testing:     Comprehensive                      │
│                                                 │
│ Estimated Lines: 350-400                        │
│ Estimated Build Time: 45-60 minutes             │
└─────────────────────────────────────────────────┘

? Ready to generate skill? [Y/n] Y

Generating skill...
████████████████████████████ 100%

✓ SKILL GENERATED SUCCESSFULLY

Output files:
  - ~/.claude/skills/rls-validator/SKILL.md
  - ~/.claude/skills/rls-validator/skill.yaml
  - ~/.claude/skills/rls-validator/tests/golden-set.yaml
  - ~/.claude/skills/rls-validator/README.md

Next steps:
  1. Review generated files
  2. Run: /rls-validator --test (to verify)
  3. Customize as needed
  4. Deploy with: /skill deploy rls-validator
```

---

## WIZARD COMMANDS

### Navigation Commands

| Command | Action |
|---------|--------|
| `next` / `n` | Proceed to next question |
| `back` / `b` | Go to previous question |
| `skip` | Skip optional question |
| `help` / `?` | Show help for current question |
| `examples` | Show more examples |
| `save` | Save progress and exit |
| `load` | Load saved progress |
| `restart` | Start wizard over |
| `quit` / `q` | Exit without saving |

### Special Commands

| Command | Action |
|---------|--------|
| `preview` | Preview current skill state |
| `validate` | Run validation on all answers |
| `defaults` | Show/apply recommended defaults |
| `summary` | Show answers summary |
| `export` | Export answers to YAML |
| `import [file]` | Import answers from file |

---

## ERROR RECOVERY

### Common Errors and Recovery

| Error | Recovery |
|-------|----------|
| Invalid format | Show format requirements, examples |
| Missing required | Highlight field, prevent proceed |
| Conflict detected | Show conflicting values, suggest fix |
| Validation failed | Show specific error, allow edit |

### Auto-Save and Recovery

```yaml
# Auto-save location
path: ~/.claude/skills/.wizard-state/[skill-name].yaml

# Recovery prompt on restart
recovery:
  prompt: "Found incomplete wizard session for '[skill-name]'. Resume?"
  options:
    - "Resume from question [X]"
    - "Start fresh"
    - "Load and review answers"
```

---

<!-- INTERACTIVE_WIZARD v23.0.0 | Updated: 2026-01-27 -->
