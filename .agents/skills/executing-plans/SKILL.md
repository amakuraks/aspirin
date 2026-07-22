---
name: executing-plans
description: "Execute implementation plans task by task with verification. Used by /work workflow."
---

# Executing Plans Skill

## Purpose

Systematically execute a plan document. Each task is implemented, verified, and committed before moving to the next.

## Execution Loop

For each task in the plan:

```
1. READ    — Understand the task fully
2. CHECK   — Search docs/solutions/ for related known issues
3. CODE    — Implement exactly as planned (complete code, not stubs)
4. TEST    — TDD cycle if tdd_mode is strict/balanced:
             a. Write failing test
             b. Implement code
             c. Run test → pass
5. VERIFY  — Run verification command from the plan
6. COMMIT  — git add + git commit -m "feat(scope): description"
7. MARK    — Check off the task
```

## TDD Mode Behavior

| Mode | Behavior |
|------|----------|
| `strict` | Every task MUST have a test written first |
| `balanced` | Tests for logic/services. Skip for pure config/routing. |
| `relaxed` | Tests optional. Write if useful. |

## Batch Checkpoints

After every **3 tasks**, pause:

```
## Progress: 3/12 tasks complete

✅ Task 1: [description]
✅ Task 2: [description]
✅ Task 3: [description]
⬜ Task 4: [description] ← next
⬜ Task 5: [description]
⬜ Task 6: [description]
... (list ALL remaining tasks)

Ready for next batch, or any feedback?
```

Always show **every task** in the checklist (completed + remaining). Use the actual task count for progress (e.g., `3/12`, `6/12`).

**Wait for user before continuing.**

## Deviation Handling

If the plan doesn't match reality during execution:

1. **Minor deviation** (typo, slightly different path): Fix and continue. Note it.
2. **Significant deviation** (missing dependency, wrong assumption): STOP. Tell the user. Suggest plan amendment.
3. **Never silently deviate** from the plan without informing the user.

## Commit Convention

Format: `<type>(<scope>): <description>`

Types: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`

Examples:
- `feat(users): add user service with CRUD operations`
- `feat(dashboard): create transaction list component`
- `test(users): add unit tests for user service`
