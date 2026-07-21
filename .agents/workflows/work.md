---
description: "Execute implementation plan task by task. Incremental commits. Auto-compounds knowledge on non-trivial solutions."
---

# /work — Execute Plan

## Process

### 1. Load Plan

- Read the plan from `docs/plans/`.
- If multiple plans exist, ask which one to execute.
- Confirm understanding of all tasks before starting.

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

### 4. Batch Checkpoints

After every **3 tasks**, pause and show:

```
## Progress: [N/Total] tasks complete

✅ Task 1: [description]
✅ Task 2: [description]
✅ Task 3: [description]
⬜ Task 4: [description] (next)
...

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
