---
description: "Execute implementation plan task by task. Incremental commits. Auto-compounds knowledge on non-trivial solutions."
---

# /work — Execute Plan

## Process

### 1. Load Plan

- Read the plan from `docs/plans/`.
- If multiple plans exist, ask which one to execute.
- Confirm understanding of all tasks before starting.
- If the plan has NOT been explained yet (`/layman`), consider running it first — especially if the user or stakeholders need a plain-English view of what's being built. This is a suggestion, not a requirement: `/work` can proceed without it if the user prefers.

### 2. Setup

```bash
git checkout -b feat/<feature-name>
```

If git is not initialized or user prefers no-git, skip.

### 3. Execute Tasks

For each task in the plan:

1. **Read the task** — understand what to do.
2. **Check `docs/solutions/`** — any known issues related to this task?
3. **Implement** — follow the plan exactly. Write complete code, not stubs.
4. **TDD cycle** (if tdd_mode is strict or balanced):
   - Write failing test first
   - Implement the code
   - Run test → confirm pass
5. **Verify** — run the verification command from the plan.
6. **Commit** — `git add` + `git commit -m "feat(scope): description"`
7. **Mark complete** — check off the task.
8. **Use case update** — if this task adds, removes, or modifies a use case:
   - Update `docs/use-cases.md` with the change.
   - Note: "📋 Updated use case: [use case name]"

### 3.5 Repetitive Refactoring Check

If a task requires the **same change across many files** (e.g., adding a parameter, renaming a method):
1. Identify the mechanical vs context-dependent parts.
2. Offer to split:
   - "This needs the same change in N files. I'll handle [mechanical part]. Can you handle [context-dependent part]? Here's the list: ..."
3. Provide specific find-and-replace patterns or file lists.
4. Wait for user confirmation before proceeding.

### 4. Batch Checkpoints

After every **3 tasks**, pause and show:

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

At each checkpoint, also estimate context fill. If ≥60% → append to the progress block:
`⚠️ Context ~60%+. Run /compact before continuing.`

Wait for user before continuing.

### 5. Quality Check

After ALL tasks are complete:

- Run full test suite (test_command from project config)
- Run linter (lint_command from project config)
- Verify against plan's acceptance criteria
- Show verification output as evidence

### 6. Auto-Compound

Check if any task involved:
- Solving a non-trivial problem
- Working around a tricky issue
- Encountering a recurring pattern

If yes → auto-save to `docs/solutions/<category>/<filename>.md`
Mention: "💊 Saved to `docs/solutions/...` for future reference"

### ⛔ STOP

> "Implementation complete. Run `/review` when ready."

**DO NOT proceed to `/review`.** Wait for explicit user instruction.
