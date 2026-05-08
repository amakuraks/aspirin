---
description: "Create a comprehensive implementation plan. Always detailed. Detects messy code and includes restructuring."
---

# /plan — Comprehensive Implementation Plan

## Process

### 1. Gather Context

- Read recent brainstorm doc from `docs/brainstorms/` (if exists).
- Scan existing codebase for patterns, conventions, and file structure.
- If library/API docs needed → use `context7-docs` skill (before web search).
- Check `docs/solutions/` for previously solved related problems.

### 2. Structure Check (Existing Codebases)

Detect AI-generated or poorly structured code:
- Giant monolithic components (500+ lines)
- Inline API calls in JSX / template files
- No separation of concerns
- Duplicated logic across files
- Business logic in controllers (Laravel) or components (React)
- Missing TypeScript types / interfaces

If detected:
> "⚠️ The current code structure needs cleanup before adding features. Phase 1 of this plan restructures the existing code. Building on a broken foundation guarantees future headaches."

Include restructuring as **Phase 1** of the plan before any feature work.

### 3. Write Plan

**Always comprehensive depth.** No quick/standard selector.

Structure the plan in phases:
- **Phase 1** (if needed): Restructure existing code
- **Phase 2**: Database layer (migrations, models, relationships)
- **Phase 3**: Backend logic (services, controllers, routes, form requests)
- **Phase 4**: Frontend (types, API client, hooks, components, pages)
- **Phase 5**: Testing and verification

Each task must include:
- **Exact file paths** — where to create or modify
- **Complete code** — not placeholders or pseudocode
- **Verification** — command to run + expected output
- **Single layer** — each task touches ONE layer (DB, backend, or frontend)

**Laravel task order:** Migration → Model → Service → Controller → Route → FormRequest
**React task order:** Type/Interface → API Client → Hook → Component → Page → Route

### 4. Acceptance Criteria

End the plan with clear acceptance criteria:

```
## Acceptance Criteria
- [ ] [Specific, verifiable outcome 1]
- [ ] [Specific, verifiable outcome 2]
- [ ] [Specific, verifiable outcome 3]
```

### 5. Save

Write to `docs/plans/YYYY-MM-DD-<feature>-plan.md`

### ⛔ STOP

> "Plan ready. Run `/gate` to review before coding."

**DO NOT proceed to `/gate`.** Wait for user to say CONTINUE or invoke `/gate`.
