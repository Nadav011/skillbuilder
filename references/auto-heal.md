# Auto-Heal Protocol - SINGULARITY FORGE Reference

## Overview

Automatic healing of failed gates during skill validation. When a gate fails, the system attempts to fix it automatically before requesting human intervention.

---

## 1. AUTO-HEAL ARCHITECTURE

```
┌─────────────────────────────────────────────────────────────┐
│                    AUTO-HEAL PIPELINE                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  VALIDATE           ANALYZE           HEAL           VERIFY  │
│  ┌──────┐          ┌──────┐         ┌──────┐        ┌──────┐│
│  │ Run  │ ──FAIL─► │ Find │ ──────► │ Fix  │ ────► │ Re-  ││
│  │ Gate │          │ Root │         │ Auto │        │ Run  ││
│  └──────┘          │ Cause│         └──────┘        │ Gate ││
│      │             └──────┘             │           └──────┘│
│      ▼                                  ▼               │    │
│    PASS ◄───────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

---

## 2. HEALABLE GATES

### Automatically Healable (No Human Input)

| Gate | Issue | Auto-Fix |
|------|-------|----------|
| 2 | Missing version | Add `version: "1.0.0"` |
| 4 | Invalid name format | Convert to kebab-case |
| 6 | Broken reference paths | Create missing files or fix paths |
| 7 | Missing description | Generate from SKILL.md content |
| 11 | Filler phrases | Replace with imperative form |
| 13 | Too many lines | Split to references/ |
| 16 | Tag mismatch | Close unclosed tags |
| 27 | Deep nesting | Flatten to max 3 levels |
| 34 | TODO markers | Remove or convert to issues |
| 38 | Missing tables | Convert prose to tables |
| 40 | Passive voice | Convert to imperative |
| 50-52 | RTL violations | Replace ml/mr with ms/me |

### Requires Human Input

| Gate | Issue | Why Manual |
|------|-------|------------|
| 1 | Intent alignment | Requires understanding user intent |
| 3 | Role unclear | Needs clarification of purpose |
| 8 | Input validation | Domain-specific validation rules |
| 14 | Security issue | Requires security review |
| 19 | Error handling | Domain-specific error messages |
| 20 | Edge cases | Domain-specific scenarios |

---

## 3. HEAL STRATEGIES

### Strategy 1: Line Count (Gate 13)

```typescript
// Detect: SKILL.md > 500 lines
// Fix: Extract sections to references/

async function healLineCount(skillPath: string) {
  const content = await readFile(`${skillPath}/SKILL.md`);
  const lines = content.split('\n');

  if (lines.length > 500) {
    // Find sections that can be extracted
    const sections = parseSections(content);

    for (const section of sections) {
      if (section.lines > 50 && section.type === 'reference-worthy') {
        // Move to references/
        await writeFile(
          `${skillPath}/references/${section.name}.md`,
          section.content
        );

        // Replace with reference link
        content = content.replace(
          section.content,
          `See [${section.name}](references/${section.name}.md)`
        );
      }
    }
  }

  return content;
}
```

### Strategy 2: RTL Violations (Gates 50-52)

```typescript
// Detect: ml-, mr-, pl-, pr-, left-, right-, text-left, text-right
// Fix: Replace with logical properties

const RTL_REPLACEMENTS = {
  'ml-': 'ms-',
  'mr-': 'me-',
  'pl-': 'ps-',
  'pr-': 'pe-',
  'left-': 'start-',
  'right-': 'end-',
  'text-left': 'text-start',
  'text-right': 'text-end',
  'rounded-l': 'rounded-s',
  'rounded-r': 'rounded-e',
  'border-l': 'border-s',
  'border-r': 'border-e',
};

function healRTL(content: string): string {
  let healed = content;
  for (const [bad, good] of Object.entries(RTL_REPLACEMENTS)) {
    healed = healed.replace(new RegExp(bad, 'g'), good);
  }
  return healed;
}
```

### Strategy 3: Filler Phrases (Gate 11)

```typescript
const FILLER_PATTERNS = [
  { pattern: /This skill will help you/gi, replacement: '' },
  { pattern: /is designed to/gi, replacement: '' },
  { pattern: /in order to/gi, replacement: 'to' },
  { pattern: /It is important to note that/gi, replacement: '' },
  { pattern: /Please note that/gi, replacement: '' },
  { pattern: /As you can see/gi, replacement: '' },
];

function healFillers(content: string): string {
  let healed = content;
  for (const { pattern, replacement } of FILLER_PATTERNS) {
    healed = healed.replace(pattern, replacement);
  }
  return healed.replace(/\s+/g, ' ').trim();
}
```

### Strategy 4: Passive to Imperative (Gate 40)

```typescript
const PASSIVE_TO_IMPERATIVE = [
  { pattern: /should be (\w+)ed/gi, replacement: '$1' },
  { pattern: /will be (\w+)ed/gi, replacement: '$1' },
  { pattern: /can be (\w+)ed/gi, replacement: '$1' },
  { pattern: /is (\w+)ed by/gi, replacement: '$1' },
];

function healPassive(content: string): string {
  let healed = content;
  for (const { pattern, replacement } of PASSIVE_TO_IMPERATIVE) {
    healed = healed.replace(pattern, replacement);
  }
  return healed;
}
```

### Strategy 5: Prose to Tables (Gate 38)

```typescript
// Detect: Lists that could be tables
// Fix: Convert to markdown tables

function healProseToTable(content: string): string {
  // Find bullet lists with consistent structure
  const listPattern = /^[-*]\s+\*\*(.+?)\*\*:\s+(.+)$/gm;
  const matches = content.matchAll(listPattern);

  const items = [...matches];
  if (items.length >= 3) {
    // Convert to table
    const table = [
      '| Item | Description |',
      '|------|-------------|',
      ...items.map(m => `| ${m[1]} | ${m[2]} |`)
    ].join('\n');

    // Replace original list
    content = content.replace(/* original list */, table);
  }

  return content;
}
```

---

## 4. HEAL WORKFLOW

### Phase 1: Collect Failures

```bash
# Run validation and capture failures
validate-skill.sh $PATH 2>&1 | grep -E "^\[FAIL\]" > failures.txt
```

### Phase 2: Classify Failures

| Category | Action |
|----------|--------|
| Auto-healable | Apply fix immediately |
| Needs context | Ask one clarifying question |
| Manual only | Report to user with guidance |

### Phase 3: Apply Fixes

```typescript
async function healSkill(path: string, failures: Gate[]) {
  for (const gate of failures) {
    if (HEALABLE_GATES.includes(gate.number)) {
      await applyHeal(path, gate);
      console.log(`✅ Auto-healed Gate ${gate.number}: ${gate.name}`);
    } else {
      console.log(`⚠️ Gate ${gate.number} requires manual fix: ${gate.guidance}`);
    }
  }
}
```

### Phase 4: Re-validate

```bash
# Run validation again after healing
validate-skill.sh $PATH

# If still failing, report remaining issues
```

---

## 5. HEAL COMMANDS

| Command | Description |
|---------|-------------|
| `/forge --heal [path]` | Auto-heal all fixable gates |
| `/forge --heal --dry-run [path]` | Preview fixes without applying |
| `/forge --heal --gate [N] [path]` | Heal specific gate only |
| `/forge --heal --report [path]` | Generate heal report |

---

## 6. HEAL REPORT FORMAT

```markdown
## Auto-Heal Report

**Skill:** my-skill
**Date:** 2026-01-26

### Healed (5)
- ✅ Gate 11: Removed 3 filler phrases
- ✅ Gate 13: Extracted 2 sections to references/
- ✅ Gate 38: Converted 1 list to table
- ✅ Gate 40: Fixed 4 passive voice instances
- ✅ Gate 50: Fixed 2 RTL violations

### Manual Required (2)
- ⚠️ Gate 3: Role unclear - add PURPOSE section
- ⚠️ Gate 19: Add specific error messages

### Unchanged (45)
- Already passing
```

---

## 7. HEAL LIMITS

| Limit | Value | Reason |
|-------|-------|--------|
| Max heal attempts | 3 | Prevent infinite loops |
| Max file changes | 10 | Limit scope |
| Max line changes | 100 | Preserve original intent |
| Backup required | Yes | Always backup before heal |

---

## 8. IMPLEMENTATION CHECKLIST

```markdown
## Auto-Heal Implementation

- [ ] Backup original files
- [ ] Run validation to collect failures
- [ ] Classify failures by healability
- [ ] Apply auto-fixes in order (low-risk first)
- [ ] Re-validate after each fix
- [ ] Generate heal report
- [ ] Restore backup if healing makes it worse
- [ ] Report remaining manual fixes to user
```

---

_SINGULARITY FORGE v19.0.0 - Auto-Heal Protocol_
