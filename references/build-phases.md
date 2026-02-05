# Skill Build Phases

## 9-Phase Build Pipeline

All skill generation follows this exact sequence. Each phase is mandatory.

---

## Phase 0: EXTERNAL RESEARCH (Timeout: 60s)

**Objective:** Research external sources before internal context gathering.

**Actions:**
1. **WebSearch** domain best practices (e.g., "skill design patterns 2026")
2. **Context7** resolve-library-id -> query-docs for relevant frameworks
3. **WebFetch** Anthropic docs if MCP/tool updates suspected
4. **Memory MCP** search_nodes for past learnings on similar skills
5. **Task** spawn research agents for complex domains

**Research Queries:**

| Domain | Search Query |
|--------|-------------|
| General | "[domain] best practices [YEAR]" |
| Anthropic | "Anthropic Claude updates [YEAR]" |
| Patterns | "LLM skill design patterns [YEAR]" |
| Security | "[domain] security guidelines" |

**Success Criteria:**
- [ ] Domain best practices gathered
- [ ] Context7 docs queried (if applicable library)
- [ ] Memory checked for past learnings
- [ ] Research findings documented

**Common Mistakes:**
- Skipping external research (stale patterns)
- Not using Context7 for library docs
- Ignoring Memory MCP past learnings

---

## Phase 1: RESEARCH (Timeout: 30s)

**Objective:** Gather context and constraints before generating anything.

**Actions:**
1. Read `~/.claude/MASTER_COORDINATE.md` for 6 APEX Laws and 70-Gate matrix
2. Scan existing skills in `~/.claude/skills/` for proven patterns
3. Check Anthropic docs via WebSearch if MCP or tool usage unclear

**Success Criteria:**
- [ ] Understand APEX Laws #1-6
- [ ] Know which gates apply to this skill type
- [ ] Have reference examples loaded

**Common Mistakes:**
- Skipping MASTER_COORDINATE.md (results in non-compliant skills)
- Not checking existing skills (reinventing patterns)
- Generating before understanding constraints

---

## Phase 2: INTENT_PARSING

**Objective:** Classify skill type and determine structural requirements.

**Determine:**

| Question | Options | Impact |
|----------|---------|--------|
| **Type** | Slash Command / Agent Skill | File location (commands/ vs skills/) |
| **Complexity** | Simple (<200) / Standard (<400) / Complex (<500) | Directory structure |
| **Freedom** | Low (scripts) / Medium (patterns) / High (heuristics) | Execution constraints |
| **MCP Required** | Yes / No | Add to allowed_tools |
| **Subagents** | Single / Multi-agent | Task tool needed |

**Decision Matrix:**

```
User says "create a /commit command"
  → Type: Command
  → Complexity: Simple (< 100 lines)
  → Structure: commands/commit.md

User says "build skill to refactor code"
  → Type: Agent Skill
  → Complexity: Standard (multi-step)
  → Structure: skills/refactor-expert/SKILL.md + references/

User says "build full-stack scaffolder with MCP"
  → Type: Agent Skill
  → Complexity: Complex (MCP + subagents)
  → Structure: skills/scaffold-pro/SKILL.md + references/ + scripts/
```

**Success Criteria:**
- [ ] Type clearly identified
- [ ] Line limit determined
- [ ] Structure pattern selected
- [ ] Tool requirements known

---

## Phase 3: STRUCTURE_SELECTION

**Objective:** Choose directory layout and file organization.

**Patterns:**

### Simple (< 200 lines)
```
skill-name/
└── SKILL.md
```

### Standard (< 400 lines)
```
skill-name/
├── SKILL.md
└── references/
    ├── patterns.md
    ├── examples.md
    └── checklist.md
```

### Complex (< 500 lines)
```
skill-name/
├── SKILL.md
├── references/
│   ├── patterns.md
│   ├── api-reference.md
│   └── troubleshooting.md
└── scripts/
    ├── validate.sh
    └── setup.sh
```

### Command (< 100 lines)
```
commands/
└── command-name.md
```

**Success Criteria:**
- [ ] Directory structure matches complexity
- [ ] All required directories created
- [ ] File naming follows kebab-case convention

---

## Phase 4: GENERATION

**Objective:** Generate all files in correct order with progressive disclosure.

**Mandatory Order:**

1. **skill.yaml** (metadata first)
   - Name, version, description
   - Triggers, allowed_tools
   - Commands (if applicable)
   - < 150 lines

2. **SKILL.md** (core instructions)
   - Role, capabilities, triggers
   - Structured Chain-of-Thought (SCoT)
   - Primary workflow
   - < 500 lines

3. **references/** (detailed docs)
   - Patterns, examples, checklists
   - API references, troubleshooting
   - On-demand loading

4. **scripts/** (if needed)
   - Validation scripts
   - Setup/teardown scripts
   - Testing utilities

**Content Rules:**

| File | Purpose | Max Size | Format |
|------|---------|----------|--------|
| skill.yaml | Metadata only | 150 lines | YAML |
| SKILL.md | Core logic | 500 lines | Markdown |
| references/*.md | Details | 1000 lines/file | Markdown |
| scripts/*.sh | Automation | No limit | Bash |

**Success Criteria:**
- [ ] skill.yaml valid YAML, < 150 lines
- [ ] SKILL.md has all core sections, < 500 lines
- [ ] References split logically
- [ ] No duplicate content across files

---

## Phase 5: VERIFICATION

**Objective:** Validate against 70-Gate matrix and 6 APEX Laws.

**Checklist:**

### File Validation
- [ ] skill.yaml is valid YAML
- [ ] SKILL.md < 500 lines
- [ ] skill.yaml < 150 lines
- [ ] All references/ files exist
- [ ] No README.md or CHANGELOG.md

### Gate Validation
- [ ] Cluster 1 (Genesis): Identity, version, role, name
- [ ] Cluster 2 (Law): Zero Trust, types, token ROI
- [ ] Cluster 3 (Cognitive): Precision, state, errors
- [ ] Cluster 4 (XML): Tags, isolation, compression
- [ ] Cluster 5 (Exorcist): No ghost logic, DRY
- [ ] Cluster 6 (Sensation): Fluidity, clarity, flow
- [ ] Cluster 7 (Neural): Token efficiency, context
- [ ] Cluster 8 (Scale): 8pt spacing, 44x44px touch

### APEX Laws
- [ ] Law #1 (Zero Trust): Zod validation, Result<T,E>
- [ ] Law #2 (N+1): Code splitting, lazy loading
- [ ] Law #3 (Immortality): < 500 lines, self-documenting
- [ ] Law #4 (ROI): Token efficiency, caching
- [ ] Law #5 (RTL): ms-/me-/start-/end- only
- [ ] Law #6 (Responsive): Mobile-first, 44x44px touch

**Success Criteria:**
- [ ] All gates pass (70/70)
- [ ] All laws compliant (6/6)
- [ ] No validation errors

---

## Phase 6: REGISTRATION

**Objective:** Finalize and register skill in Claude Code.

**Actions:**

1. **Run validation script:**
   ```bash
   bash ~/.claude/skills/singularity-forge/scripts/validate-skill.sh <skill-path>
   ```

2. **Verify file structure:**
   ```bash
   tree ~/.claude/skills/<skill-name>/
   ```

3. **Test skill trigger:**
   - Restart Claude Code (if needed)
   - Test trigger phrases
   - Verify skill activates

4. **Document in skill registry** (if exists)

**Success Criteria:**
- [ ] validate-skill.sh passes
- [ ] All files created successfully
- [ ] Skill responds to triggers
- [ ] No errors in Claude Code logs

---

## Phase Transition Gates

Between phases, verify:

```
Phase 1 → 2: MASTER_COORDINATE.md read, examples scanned
Phase 2 → 3: Type/complexity determined, structure known
Phase 3 → 4: Directories created, file list ready
Phase 4 → 5: All files generated, content complete
Phase 5 → 6: Validation passes, gates clear
Phase 6 → DONE: Skill registered, triggers tested
```

**If any gate fails: HALT and fix before proceeding.**

---

## Error Recovery

| Phase | Common Error | Recovery |
|-------|--------------|----------|
| 1 | Can't find MASTER_COORDINATE.md | Search ~/.claude/ recursively |
| 2 | Ambiguous intent | ASK user for clarification |
| 3 | Wrong structure chosen | Re-parse intent, adjust |
| 4 | File too large | Split into references/ |
| 5 | Gate validation fails | Refactor to comply |
| 6 | Trigger not working | Check skill.yaml triggers list |

**Prime Directive: ASK if uncertain. Never guess.**

---

## Phase 7: TEST GENERATION (Timeout: 45s)

### Purpose
Generate comprehensive test suite for the skill.

### Actions
1. **Generate Golden Set**
   - Create 5+ test cases per category
   - Categories: Happy Path, Edge Cases, Error Cases, Security

2. **Create Test Template**
   ```typescript
   describe('SkillName', () => {
     it('should handle valid input', () => { ... });
     it('should reject invalid input', () => { ... });
     it('should block injection attempts', () => { ... });
   });
   ```

3. **LLM-as-Judge Setup**
   - Configure evaluation criteria
   - Set quality thresholds

### Output
- `tests/skill-name.test.ts`
- Golden set JSON
- Evaluation config

### Gate
| Check | Threshold |
|-------|-----------|
| Test coverage | > 80% |
| Golden set size | >= 5 per category |
| Security tests | >= 3 |

> See: `references/test-generation.md`

---

## Phase 8: BENCHMARK VALIDATION (Timeout: 30s)

### Purpose
Compare skill against industry standards and external benchmarks.

### Actions
1. **Gather Benchmarks**
   - Anthropic best practices
   - OpenAI patterns
   - Google guidelines
   - Academic research

2. **Score Against 5 Dimensions**
   | Dimension | Weight | Threshold |
   |-----------|--------|-----------|
   | Correctness | 25% | > 0.9 |
   | Efficiency | 20% | > 0.8 |
   | Safety | 25% | > 0.95 |
   | Clarity | 15% | > 0.85 |
   | Robustness | 15% | > 0.8 |

3. **Gap Analysis**
   - Identify deviations from standards
   - Generate improvement recommendations

### Output
- Benchmark score card
- Gap analysis report
- Improvement recommendations

### Gate
| Check | Threshold |
|-------|-----------|
| Overall score | > 0.85 |
| Safety score | > 0.95 |
| No critical gaps | 0 |

> See: `references/external-benchmarks.md`

---

## Phase Transition Gates (Updated)

Between phases, verify:

```
Phase 0 -> 1: External research complete, domain context gathered
Phase 1 -> 2: MASTER_COORDINATE.md read, examples scanned
Phase 2 -> 3: Type/complexity determined, structure known
Phase 3 -> 4: Directories created, file list ready
Phase 4 -> 5: All files generated, content complete
Phase 5 -> 6: Validation passes, gates clear
Phase 6 -> 7: Skill registered, triggers tested
Phase 7 -> 8: Tests generated, coverage adequate
Phase 8 -> DONE: Benchmarks pass, GOD-TIER certified
```

**If any gate fails: HALT and fix before proceeding.**
