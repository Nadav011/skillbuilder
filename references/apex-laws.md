# THE 6 IMMORTAL APEX LAWS (v21.0.0-SLIM)

> These laws are IMMUTABLE. No skill, agent, or command may violate them.

## Law Matrix

| # | Law | Icon | Mantra | Critical Rule |
|---|-----|------|--------|---------------|
| 1 | ZERO TRUST | SENTINEL | "Trust nothing, validate everything" | Zod validation, Result<T,E>, 0% any |
| 2 | INFINITE SCALE | HORIZON | "Build for billion users from day one" | <100KB bundle, code splitting, RSC+PPR |
| 3 | IMMORTALITY | ECHO | "Code that requires comments is failure" | <150 lines/file, self-documenting |
| 4 | ROI | ALCHEMIST | "Intelligence Density is the only currency" | Query caching, optimistic updates |
| 5 | RTL-FIRST | COMPASS | "Hebrew/Arabic native, LTR is adaptation" | ms-/me-/start-/end- ONLY |
| 6 | RESPONSIVE | WATER | "Fluid like water, adapt to any container" | Mobile-first, 44x44px touch targets |

## Law Details

### Law #1: ZERO TRUST (SENTINEL)
- Validate ALL inputs at system boundaries with Zod
- Use Result<T,E> pattern for error handling
- 0% tolerance for `any` type
- Never trust client-side data

### Law #2: INFINITE SCALE (HORIZON)
- Bundle size < 100KB (gzipped)
- Use code splitting and lazy loading
- RSC (React Server Components) + PPR (Partial Prerendering)
- Edge-first architecture

### Law #3: IMMORTALITY (ECHO)
- Files < 150 lines
- Functions < 30 lines
- Self-documenting code (no comments explaining WHAT)
- Comments only for WHY

### Law #4: ROI (ALCHEMIST)
- TanStack Query for server state caching
- Optimistic updates for instant UX
- Minimize re-renders
- Opus(orchestrate), Sonnet(implement), Haiku(search)

### Law #5: RTL-FIRST (COMPASS)
- Use ms-/me- instead of ml-/mr-
- Use ps-/pe- instead of pl-/pr-
- Use text-start/text-end instead of text-left/text-right
- Use start-/end- instead of left-/right-
- Directional icons: rtl:rotate-180
- Numbers: wrap in dir="ltr"

### Law #6: RESPONSIVE (WATER)
- Mobile-first approach
- Touch targets minimum 44x44px (Tailwind: min-h-11 min-w-11)
- 8pt grid spacing (4, 8, 12, 16, 24, 32, 48, 64 px)
- Fluid widths, avoid fixed pixels
- Container queries for component-level responsiveness

## Enforcement

Every skill, agent, and command MUST:
1. Check Law #5 (RTL) compliance before output
2. Check Law #6 (Responsive) compliance before output
3. Validate all inputs (Law #1)
4. Optimize for performance (Law #2)
5. Keep code clean (Law #3)
6. Maximize value per token (Law #4)
