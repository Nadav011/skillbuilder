# THE 9 DIMENSIONS OF SYNTHESIS

> **Aligned with MASTER_COORDINATE v21.0.0-SLIM-SLIM**
> **Laws:** 6 APEX Laws | **Gates:** 70-Gate Matrix | **Dimensions:** 9D Framework

## Overview

Every skill MUST be validated against all 9 dimensions.
Dimensions map to the 6 APEX Laws and ensure holistic quality.
Combined with the 70-Gate Matrix, this provides complete skill validation.

---

## Dimension Matrix

| #   | Dimension | Metaphor       | Focus                         | APEX Law          |
| --- | --------- | -------------- | ----------------------------- | ----------------- |
| 1   | STRUCTURE | Skeleton       | Types, Zod, Pure Functions    | #1 Zero Trust     |
| 2   | SENSATION | Nervous System | UX Flow, Feedback, Clarity    | #3 Immortality    |
| 3   | DESIGN    | Aesthetic      | Visual Hierarchy, Formatting  | #3 Immortality    |
| 4   | INTEGRITY | Immune System  | Error Handling, Edge Cases    | #1 Zero Trust     |
| 5   | SCALE     | Muscles        | Performance, Token Efficiency | #2 Infinite Scale |
| 6   | ORACLE    | Senses         | Domain Logic, Business Rules  | #2 Infinite Scale |
| 7   | ALCHEMIST | Metabolism     | State Management, Caching     | #4 ROI            |
| 8   | NEURAL    | Brain          | AI Optimization, Context      | #1 Zero Trust     |
| 9   | EXORCIST  | Soul           | Cleanup, Dead Code, Debt      | #3 Immortality    |

**Cross-Cutting Laws (Apply to ALL Dimensions):**
- **Law #5 RTL-FIRST (COMPASS)**: All UI dimensions (D2, D3) must use logical properties
- **Law #6 RESPONSIVE (WATER)**: All UI dimensions must ensure 44x44px touch targets, 8pt grid

---

## Detailed Dimensions

### 1. STRUCTURE (Skeleton)

**Focus:** Atomic purity of code structure

**Checklist:**

- [ ] Types explicitly defined
- [ ] Zod validation for inputs
- [ ] Pure functions where possible
- [ ] Result<T,E> pattern for errors
- [ ] Max 250 lines per file (skills: 500)

**Anti-Patterns:**

- `any` type usage
- Implicit type coercion
- Side effects in pure functions

---

### 2. SENSATION (Nervous System)

**Focus:** User experience and feedback

**Checklist:**

- [ ] Clear input expectations
- [ ] Progress indicators
- [ ] Actionable error messages
- [ ] Smooth reading flow
- [ ] Logical step progression

**Anti-Patterns:**

- Ambiguous instructions
- No feedback on completion
- Confusing terminology

---

### 3. DESIGN (Aesthetic)

**Focus:** Visual consistency and hierarchy

**Checklist:**

- [ ] Consistent heading structure
- [ ] Proper markdown formatting
- [ ] Tables for structured data
- [ ] Code blocks with language tags
- [ ] Semantic sectioning

**Anti-Patterns:**

- Inconsistent formatting
- Wall of text
- Missing visual breaks

---

### 4. INTEGRITY (Immune System)

**Focus:** Robustness and error handling

**Checklist:**

- [ ] All edge cases covered
- [ ] Empty input handling
- [ ] Invalid input handling
- [ ] Idempotent operations
- [ ] Graceful degradation

**Anti-Patterns:**

- Assuming valid input
- Silent failures
- Unhandled edge cases

---

### 5. SCALE (Muscles)

**Focus:** Performance and efficiency

**Checklist:**

- [ ] O(n) or better algorithms
- [ ] No N+1 patterns
- [ ] Token-efficient design
- [ ] Progressive disclosure
- [ ] Under 500 lines (Rule 500)

**Anti-Patterns:**

- Redundant operations
- Excessive verbosity
- Monolithic files

---

### 6. ORACLE (Senses)

**Focus:** Domain logic isolation

**Checklist:**

- [ ] Domain knowledge in references/
- [ ] Clear business rules
- [ ] Separation of concerns
- [ ] Testable logic
- [ ] Observable outcomes

**Anti-Patterns:**

- Mixed concerns
- Hidden business logic
- Unclear success criteria

---

### 7. ALCHEMIST (Metabolism)

**Focus:** State and resource optimization

**Checklist:**

- [ ] Minimal state required
- [ ] Lazy loading applied
- [ ] Caching where beneficial
- [ ] Resource cleanup
- [ ] Right tool for job

**Anti-Patterns:**

- Unnecessary state
- Eager loading everything
- Resource leaks

---

### 8. NEURAL (Brain)

**Focus:** AI/Claude optimization

**Checklist:**

- [ ] Context-efficient prompts
- [ ] Clear trigger conditions
- [ ] Appropriate tool permissions
- [ ] Progressive context loading
- [ ] Anti-hallucination anchors

**Anti-Patterns:**

- Context bloat
- Vague triggers
- Over-permissive tools

---

### 9. EXORCIST (Soul)

**Focus:** Code health and cleanup

**Checklist:**

- [ ] No dead code paths
- [ ] No TODO markers
- [ ] No commented code
- [ ] No redundancy
- [ ] Zero technical debt

**Anti-Patterns:**

- Ghost logic
- Leftover debug code
- Copy-paste redundancy

---

## Full Audit Checklist

```
[ ] D1: Types explicit, Zod validation?
[ ] D2: UX clear, feedback present?
[ ] D3: Formatting consistent?
[ ] D4: Edge cases covered?
[ ] D5: Under 500 lines, efficient?
[ ] D6: Domain logic isolated?
[ ] D7: Minimal state?
[ ] D8: Context optimized?
[ ] D9: No dead code?
```

---

## Dimension-to-Phase Mapping

| Dimension    | Genesis Phase          |
| ------------ | ---------------------- |
| D1 STRUCTURE | Phase 1: Blueprint     |
| D2 SENSATION | Phase 3: Sensation     |
| D3 DESIGN    | Phase 3: Sensation     |
| D4 INTEGRITY | Phase 1: Blueprint     |
| D5 SCALE     | Phase 4: Optimization  |
| D6 ORACLE    | Phase 2: Wiring        |
| D7 ALCHEMIST | Phase 4: Optimization  |
| D8 NEURAL    | Phase 2: Wiring        |
| D9 EXORCIST  | Phase 5: Fortification |

---

<!-- 9_DIMENSIONS v23.0.0 | Updated: 2026-01-27 -->
