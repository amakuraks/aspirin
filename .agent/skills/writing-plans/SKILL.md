---
name: writing-plans
description: "Create comprehensive implementation plans with bite-sized tasks. Used by /plan workflow."
---

# Writing Plans Skill

## Purpose

Create detailed, executable implementation plans. Every task should be small enough to complete in one step and independently verifiable.

## Plan Structure

```markdown
# Plan: [Feature Name]

> Created: YYYY-MM-DD
> Source: docs/brainstorms/[brainstorm-file].md (if applicable)

## Overview
[1-2 sentences: what this plan builds]

## Phase 1: [Phase Name]

### Task 1.1: [Description]
**File:** `path/to/file.ext`
**Action:** create | modify
**Code:**
[Complete code — not placeholders]
**Verify:** [command to run + expected output]

### Task 1.2: [Description]
...

## Phase 2: [Phase Name]
...

## Acceptance Criteria
- [ ] [Specific verifiable outcome 1]
- [ ] [Specific verifiable outcome 2]
```

## Task Rules

| Rule | Description |
|------|-------------|
| **Bite-sized** | Each task describable in 2-3 sentences |
| **Single layer** | Each task touches ONE layer (DB, backend, or frontend) |
| **Complete code** | Full implementation, not pseudocode or "add validation here" |
| **Verifiable** | Each task has a command to prove it works |
| **Ordered** | Dependencies respected — DB before backend, backend before frontend |

## Laravel Task Order

1. Migration (create table / add columns)
2. Model (relationships, scopes, casts)
3. Service (business logic)
4. FormRequest (validation rules)
5. Controller (thin — delegates to service)
6. Route (register endpoint)

## React Task Order

1. TypeScript interface (API response/request shapes)
2. API client function (axios/fetch wrapper)
3. Custom hook (data fetching + state management)
4. Component (UI rendering)
5. Page (composition of components)
6. Route (register in router)

## Structure Detection

Before writing the plan, check for existing code issues:

- **Monolithic components** (500+ lines) → include restructuring as Phase 1
- **Inline API calls** in JSX → include API client extraction
- **Business logic in controllers** → include service extraction
- **Missing TypeScript types** → include type definition tasks
- **No test files** → include test setup task

If issues found, the plan starts with restructuring before feature work.

## Saving

Write to: `docs/plans/YYYY-MM-DD-<feature-name>-plan.md`
