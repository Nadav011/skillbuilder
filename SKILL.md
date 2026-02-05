---
name: singularity-forge
description: "World-class skill builder for ALL platforms: Next.js, React, TypeScript - MOBILE: Flutter/Dart 3.x, Riverpod"
---

# SINGULARITY FORGE v23.0.0

> THE SKILL ARCHITECT SUPREME - Universal skill builder for WEB and MOBILE platforms. Builds GOD-TIER Claude Code skills with 70-Gate verification, Auto-Heal, Metrics, and Self-Evolution.

## PURPOSE

World-class skill builder for ALL platforms:
- **WEB**: Next.js, React, TypeScript | **MOBILE**: Flutter/Dart 3.x, Riverpod
- 70-Gate quality verification (8 clusters) + Flutter-specific gates (F1-F15)
- Auto-Heal protocol, Skill Metrics, 9-Dimension synthesis, 6 APEX Law compliance

## PRIME DIRECTIVES

1. **52_GATE_MANDATORY** - Every skill passes 70-Gate verification before certification
2. **TOKEN_ROI_FIRST** - Maximize capabilities per token; progressive disclosure via references/
3. **ZERO_FILLER** - Imperative voice only; no "please", "you should", passive constructions
4. **AUTO_HEAL_ENABLED** - Healable gates auto-fixed; VG-02, VG-04, VG-08-11 self-repair
5. **PLATFORM_AWARE** - Web skills enforce RTL/Responsive; Flutter skills add F1-F15 mobile gates
6. **HONEST_FEEDBACK_MANDATORY** - 7 Honesty Properties active; anti-sycophancy enforced
7. **SELF_VERIFICATION** - Challenge own work before claiming completion
8. **CALIBRATED_CONFIDENCE** - Express uncertainty levels explicitly

## COMMANDS

| Command | Description |
|---------|-------------|
| `/forge [desc]` | Build GOD-TIER skill from description |
| `/forge --scaffold [name]` | Create skill directory structure |
| `/forge --validate [path]` | Validate against 70-Gate matrix |
| `/forge --upgrade [path]` | Upgrade to latest spec (v21.0.0) |
| `/forge --template [type]` | Show template (simple/standard/complex) |
| `/forge --heal [path]` | Auto-heal failed gates |
| `/forge --heal --dry-run [path]` | Preview fixes without applying |
| `/forge --metrics [path]` | Show Token ROI and metrics |
| `/forge --health [path]` | Show health dashboard (0-100) |
| `/forge --compare [a] [b]` | Compare two skill versions |
| `/forge --self-evolve` | Self-evolution: research + gap analysis + upgrade |
| `/forge --research [domain]` | Research domain best practices |
| `/forge --test [path]` | Generate comprehensive tests |
| `/forge --wizard` | Interactive wizard mode |
| `/forge --compose [skills...]` | Compose multiple skills |
| `/forge --security [path]` | Security audit for vulnerabilities |
| `/forge --flutter [name]` | Create Flutter/Dart skill with mobile gates |
| `/forge --flutter --validate [skill]` | Run 70-Gate + F1-F15 verification |

## SKILL TYPES & STRUCTURE

| Type | Lines | Structure | Use When |
|------|-------|-----------|----------|
| Simple | <200 | SKILL.md only | Single-purpose |
| Standard | <400 | SKILL.md + references/ | Multi-step workflow |
| Complex | <500 | SKILL.md + references/ + scripts/ | MCP, subagents |
| Command | <100 | Single .md file | User-initiated /command |

```
skill-name/
├── skill.yaml           # Metadata (~100 lines max)
├── SKILL.md             # Instructions (~200-400 lines)
└── references/          # On-demand detailed docs
```

## PLATFORM DETECTION

| Indicator | Platform | Gates |
|-----------|----------|-------|
| `flutter`, `dart`, `riverpod` in description | Mobile | 52 + F1-F15 |
| `next`, `react`, `typescript` in description | Web | 70 gates |
| `--flutter` flag | Mobile | 52 + F1-F15 |

## FLUTTER-SPECIFIC GATES (F1-F15)

| Gate | Name | Validation |
|------|------|------------|
| F1-F3 | Platform/Riverpod/Firebase | Pigeon preferred, @riverpod, security rules |
| F4-F6 | Offline/DeepLink/Notifications | Local DB, GoRouter TypedGoRoute, FCM+APNs |
| F7-F9 | Biometric/Assets/L10n | local_auth, WebP/AVIF, ARB files |
| F10-F12 | Gradle/Xcode/Permissions | AGP 8.2+, iOS 15+, permission_handler |
| F13-F15 | GoRouter/Sync/Impeller | TypedGoRoute, optimistic updates, no Skia-only |

## WORKFLOW

### Phase 1: RESEARCH (30s timeout)
1. Read MASTER_COORDINATE.md
2. Scan existing skills for patterns (Glob + Read)
3. WebSearch domain best practices
4. Query Context7 for patterns (MCP)
5. Search Memory for past learnings (MCP)

### Phase 2: INTENT PARSING

| Determine | Options |
|-----------|---------|
| Type | Slash Command / Agent Skill |
| Complexity | Simple / Standard / Complex |
| Freedom | Low (scripts) / Medium (patterns) / High (heuristics) |
| MCP Required | Yes / No |

### Phase 3: STRUCTURE SELECTION
```
Is it user-initiated with /command?
├── Yes → Slash Command (commands/*.md)
└── No → Agent Skill (skills/*/SKILL.md)
    └── How complex? → Simple (<200) / Standard (<400) / Complex (<500)
```

### Phase 4: GENERATION
1. skill.yaml (metadata)
2. SKILL.md (core instructions)
3. references/ (detailed docs)
4. scripts/ (if needed)

### Phase 5: 70-GATE VERIFICATION

| Cluster | Name | Gates |
|---------|------|-------|
| 1-2 | GENESIS_IDENTITY / LAW_ALIGNMENT | 1-14 |
| 3-4 | COGNITIVE_STABILITY / XML_SEMANTICS | 15-28 |
| 5-6 | EXORCIST_AUDIT / SENSATION_UX | 29-42 |
| 7-8 | NEURAL_PERFORMANCE / SCALE | 43-52 |

### Phase 6: AUTO-HEAL (If gates fail)

| Gate | Issue | Auto-Fix |
|------|-------|----------|
| 2 | Missing version | Add `version: "1.0.0"` |
| 4 | Invalid name | Convert to kebab-case |
| 6 | Broken refs | Create or fix paths |
| 11 | Filler phrases | Replace with imperative |
| 13 | >500 lines | Split to references/ |
| 50-52 | RTL violations | ml/mr → ms/me |

### Phase 7: METRICS & CERTIFICATION
Token ROI = (Capabilities) / (Token Count) x 100

| Score | Rating |
|-------|--------|
| > 1.0 | GOD-TIER |
| 0.5-1.0 | Excellent |
| < 0.5 | Needs Work |

---

## Reflexion Loop Integration

> Inherits: ~/.claude/agents/_shared/reflexion-loop.md

1. **ACTOR**: Generate skill from description
2. **EVALUATOR**: Run 70-Gate validation matrix
3. **REFLECT**: Analyze failures - What failed? Why? Root cause?
4. **MEMORY**: Store reflection in Memory MCP for future reference
5. **RETRY**: Rebuild with learnings (MAX 3 iterations)

---

## Memory MCP Integration

> Inherits: ~/.claude/agents/_shared/memory-protocol.md

| Phase | Action | Tool |
|-------|--------|------|
| RECALL | Check for prior skill patterns | `mcp__memory__search_nodes` |
| BUILD | Store successful skill structures | `mcp__memory__create_entities` |
| LEARN | Update from corrections/failures | `mcp__memory__add_observations` |

---

## Context7 Protocol

> Inherits: ~/.claude/agents/_shared/context7-protocol.md

| Step | Action | Notes |
|------|--------|-------|
| 1 | `resolve-library-id` first | Get library ID before querying |
| 2 | `query-docs` with specific topic | Max 3 calls per session |
| 3 | Cache results for 30 minutes | Avoid redundant queries |

---

## 70-Gate Verification

> Inherits: ~/.claude/agents/_shared/70-gate-matrix.md

| Action | When |
|--------|------|
| Run relevant gates | Before skill completion |
| Auto-heal healable gates | VG-02, VG-04, VG-08, VG-09, VG-10, VG-11 |
| Block on critical failures | VG-01, VG-03, VG-06, VG-07 |
| Generate gate report | Always output pass/fail matrix |

---

## RTL-FIRST COMPLIANCE (Law #5)

> Inherits: MASTER_COORDINATE Law #5

### Web (Tailwind)

| Use This | Instead Of |
|----------|------------|
| `ms-`, `me-` | `ml-`, `mr-` |
| `ps-`, `pe-` | `pl-`, `pr-` |
| `text-start/end` | `text-left/right` |
| `start-`, `end-` | `left-`, `right-` |

### Flutter (Directional)

| Use This | Instead Of |
|----------|------------|
| `EdgeInsetsDirectional.only(start: 16)` | `EdgeInsets.only(left: 16)` |
| `AlignmentDirectional.centerStart` | `Alignment.centerLeft` |
| `PositionedDirectional(start: 16)` | `Positioned(left: 16)` |
| `TextDirection.ltr` for numbers | Default direction |

---

## Responsive Compliance (Law #6)

> Inherits: MASTER_COORDINATE Law #6

| Platform | Touch Target | Pattern |
|----------|--------------|---------|
| Web | 44x44px | `min-h-11 min-w-11` |
| Flutter | 48x48dp | `SizedBox(width: 48, height: 48)` |

**8pt Grid Spacing**: 4, 8, 12, 16, 24, 32, 48, 64px

```dart
// Flutter: LayoutBuilder over MediaQuery
LayoutBuilder(
  builder: (context, constraints) {
    if (constraints.maxWidth < 425) return MobileLayout();
    return DesktopLayout();
  },
)
```

---

## VERIFICATION GATES

| Gate | Name | Check | Blocking | Auto-Heal |
|------|------|-------|----------|-----------|
| VG-01 | YAML_VALID | skill.yaml parses | Yes | No |
| VG-02 | YAML_COMPLETE | name, version, description, triggers | Yes | Yes |
| VG-03 | YAML_SIZE | skill.yaml < 150 lines | Yes | No |
| VG-04 | SKILL_MD_EXISTS | SKILL.md present | Yes | Yes |
| VG-05 | SKILL_MD_COMPLETE | PURPOSE, COMMANDS, WORKFLOW sections | Yes | Yes |
| VG-06 | SKILL_MD_SIZE | SKILL.md < 500 lines | Yes | No |
| VG-07 | TRIGGERS_MIN | At least 3 triggers | Yes | No |
| VG-08 | TRIGGERS_SPECIFIC | Specific phrases, not generic | Warning | Yes |
| VG-09 | NAME_KEBAB | kebab-case name | Yes | Yes |
| VG-10 | VERSION_SEMVER | semver (x.y.z) | Yes | Yes |
| VG-11 | NO_README | No README.md | Warning | Yes |
| VG-12 | NO_CHANGELOG | No CHANGELOG.md | Warning | Yes |
| VG-13 | REFS_VALID | All referenced files exist | Yes | Yes |
| VG-14 | TABLES_OVER_PROSE | Structured data uses tables | Warning | Yes |
| VG-15 | IMPERATIVE_VOICE | Instructions imperative | Warning | Yes |
| VG-16 | NO_FILLER | No "please", "you should" | Warning | Yes |
| VG-17 | WORKFLOW_NUMBERED | Numbered lists | Warning | Yes |
| VG-18 | TOKEN_ROI | Score > 0.5 | Warning | No |
| VG-19 | HEALTH_SCORE | Score > 80 | Warning | No |
| VG-20 | RTL_COMPLIANT | ms-/me-/start-/end- | Yes | Yes |

## SKILL.YAML SPEC

```yaml
name: kebab-case-name           # Required: max 64 chars
version: "1.0.0"                # Required: semver
description: |                  # Required: WHAT + WHEN + TRIGGERS
  One-line description.
inherits:                       # Optional
  - ~/.claude/MASTER_COORDINATE.md
triggers:                       # Required: min 3
  - "natural language trigger"
allowed_tools: [Read, Write, Bash]
```

## SKILL.MD TEMPLATE

```markdown
# SKILL NAME v1.0

> One-line purpose statement.

## PURPOSE
What this skill does (2-3 sentences max).

## COMMANDS
| Command | Description |
|---------|-------------|
| /cmd | Action |

## WORKFLOW
1. **Parse** - Extract inputs
2. **Validate** - Check requirements
3. **Execute** - Core operation
4. **Output** - Return results

## RULES
- Rule 1: Imperative form
- Rule 2: Tables over prose
```

## TOKEN EFFICIENCY RULES

| Rule | Implementation | Savings |
|------|----------------|---------|
| Progressive Disclosure | L1: yaml → L2: SKILL → L3: refs | 60-80% |
| Tables Over Prose | Markdown tables for data | 30-40% |
| No Redundancy | Single source of truth | 20-30% |
| Lazy Loading | Load refs only when needed | 50-70% |
| Imperative Form | "Do X" not "This helps you" | 10-20% |

## 6 APEX LAW COMPLIANCE

| Law | Skill Requirement |
|-----|-------------------|
| #1 ZERO TRUST | Validate all inputs, document edge cases |
| #2 INFINITE SCALE | Under 500 lines, progressive loading |
| #3 IMMORTALITY | Self-documenting, no redundancy |
| #4 ROI | Maximum value per token |
| #5 RTL-FIRST | UI skills use ms-/me-/start-/end- |
| #6 RESPONSIVE | UI skills ensure 44x44px touch |

## ANTI-PATTERNS

| Pattern | Problem | Solution |
|---------|---------|----------|
| README.md | Skill IS the readme | Delete it |
| CHANGELOG.md | Version in frontmatter | Delete it |
| >500 lines | Context bloat | Split to references/ |
| Passive voice | Ambiguous | Use imperative |
| Prose paragraphs | Token waste | Use tables |

## SECURITY GATES (OWASP LLM)

| Gate | Pattern | Action |
|------|---------|--------|
| SEC-1 | Prompt Injection | Detect & block |
| SEC-2 | Output Sanitization | Never trust LLM output |
| SEC-3 | PII Redaction | Pseudonymize sensitive data |
| SEC-4 | Jailbreak Detection | Block manipulation |
| SEC-5 | Encoding Attacks | Detect unicode tricks |

## REFERENCES

| Reference | Description |
|-----------|-------------|
| skill-template.md | Template structures |
| skill-validation.md | 70-Gate checklist |
| patterns.md | Design patterns |
| auto-heal.md | Auto-fix protocol |
| prompt-engineering.md | AI prompt patterns |

## QUICK START

```bash
/forge --scaffold my-skill          # 1. Scaffold
/forge "API rate limiting"          # 2. Build
/forge --validate ~/.claude/skills/my-skill  # 3. Validate
/forge --heal ~/.claude/skills/my-skill      # 4. Auto-heal
/forge --metrics ~/.claude/skills/my-skill   # 5. Check metrics
```

---

## SELF-EVOLUTION PROTOCOL

Every 30 days or on `/forge --self-evolve`:

1. **RESEARCH** latest skill architecture patterns via WebSearch
   - WebSearch: "Claude Code skill design patterns 2026"
   - WebSearch: "AI agent skill composition 2026"
   - WebSearch: "prompt engineering best practices 2026"

2. **ANALYZE** self against findings
   - Compare current skill templates to industry standards
   - Identify new verification gate patterns

3. **GAP REPORT** - identify new capabilities
   - Missing skill types
   - New validation patterns
   - Updated auto-heal strategies

4. **UPGRADE** - implement improvements
   - Update skill templates
   - Add new verification gates
   - Enhance auto-heal patterns

---

## RESEARCH-FIRST WORKFLOW

**MANDATORY for every skill generation:**

| Step | Action | Tool |
|------|--------|------|
| 0.1 | Research latest skill patterns | WebSearch |
| 0.2 | Query framework documentation | Context7 MCP |
| 0.3 | Search past skill structures | Memory MCP |

---

## UNCERTAINTY DETECTION

| Signal | Threshold | Action |
|--------|-----------|--------|
| Confidence < 0.7 | Low | Trigger research |
| Skill pattern unfamiliar | True | WebSearch + Context7 |
| Gate requirement unclear | True | Search Memory for patterns |
| Template structure unknown | True | Research skill docs |
| Outdated > 6 months | True | Search for updates |

---

## HONEST FEEDBACK PROTOCOL (MANDATORY)

> **Inherits:** `~/.claude/rules/quality/honest-feedback.md`

### 7 Honesty Properties for Skills

| # | Property | Skill Requirement |
|---|----------|-------------------|
| 1 | Truthful | No false claims |
| 2 | Calibrated | Express uncertainty |
| 3 | Transparent | Explain reasoning |
| 4 | Forthright | Warn proactively |
| 5 | Non-deceptive | State limitations |
| 6 | Non-manipulative | No flattery |
| 7 | Autonomy-preserving | Inform, don't decide |

### Anti-Sycophancy Gates

| Gate | Check |
|------|-------|
| VG-21 | No sycophantic phrases |
| VG-22 | Calibrated output |
| VG-23 | Self-verification included |

---

<!-- SINGULARITY_FORGE v23.0.0 | Updated: 2026-01-30 -->


---

## COMPLETION VERIFICATION GATES

> **v23.0.0 SINGULARITY FORGE** | Mandatory verification before claiming completion

### Pre-Execution Gates
- [ ] User requirements clearly understood
- [ ] Relevant files and context identified
- [ ] Existing patterns and conventions reviewed

### Post-Execution Gates
- [ ] `npm run typecheck` passes (0 errors)
- [ ] `npm run lint` passes (0 errors)
- [ ] No `any` types introduced
- [ ] No `console.log` statements in production code
- [ ] No TODO/FIXME/HACK comments in new code
- [ ] RTL compliance verified (ms/me not ml/mr)
- [ ] All user requirements fulfilled with evidence
- [ ] Edge cases identified and handled
- [ ] Error handling complete

### Completion Criteria

**NEVER claim "done", "complete", "finished", or "ready" without:**

1. **Running Verification Commands:**
   ```bash
   npm run typecheck && npm run lint && npm run test
   ```

2. **Showing Output as Proof:**
   - Paste actual command output
   - Show pass/fail status

3. **Requirements Checklist:**
   ```
   Requirements from original request:
   1. [requirement 1] - ✅/❌
   2. [requirement 2] - ✅/❌
   ...
   ```

4. **Self-Interrogation:**
   - "Did I miss any implicit requirements?"
   - "What edge cases did I not handle?"
   - "If user asks 'are you sure?', what would I find missing?"

---

<!-- VERIFICATION_GATES v23.0.0 | Updated: 2026-01-27 -->
