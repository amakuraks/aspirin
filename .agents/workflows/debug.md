---
description: "Standalone error diagnosis. Systematic root-cause investigation with auto-compound."
---

# /debug — Error Diagnosis

## Response Style

- **Engaging.** No dry dumps. Lead with the diagnosis, then the evidence. Keep momentum toward the fix.
- **Separate informing from asking.** Information and questions are distinct blocks. Never bury a question mid-investigation. When you ask, ask explicitly with `❓`.
- **One question per block.** If multiple questions, number them: `❓ 1) ... ❓ 2) ...`
- **End with a clear next action** — what the user should do or decide next.

## Process

### 1. Check Existing Knowledge

**Before investigating**, search `docs/solutions/` for matching symptoms.
- If match found: "This matches a known issue: [link]. Applying known fix."
- If no match: proceed with investigation.

### 2. Understand the Error

- Read the full error message and stack trace.
- If anything is unclear → ask. Don't guess.
- Identify:
  - What is the expected behavior?
  - What is the actual behavior?
  - When did it start? Any recent changes?

### 3. Reproduce

- Run the failing command or scenario.
- Confirm the error is reproducible.
- If not reproducible → gather more data, don't guess.

### 4. Investigate

- Read error messages and stack traces **completely** (don't skim).
- Check recent changes: `git diff`, `git log --oneline -10`
- Isolate the layer:
  - Database? (migration, query, relationship)
  - Backend? (service logic, controller, validation)
  - Frontend? (component, API call, state)
  - Network? (CORS, timeout, auth header)
- Trace data flow with logging at boundaries.

### 5. Hypothesize

State clearly: "The bug is caused by **[X]** because **[evidence]**"

- Max 3 hypotheses.
- Test each with the smallest possible change.
- If all 3 fail → question assumptions from step 4.

### 6. Fix

1. Write a failing test that reproduces the bug.
2. Run test → confirm it **FAILS** (proves the bug exists).
3. Implement the fix (root cause, NOT symptoms).
4. Run test → confirm it **PASSES**.
5. Run full test suite → no regressions.

### 7. Direct Feedback

If the bug exists because of poor design:

> "This null pointer happens because the Eloquent relationship isn't defined. The real fix is adding `hasMany()` in the User model, not null-checking everywhere. Null checks here are band-aids — the wound is in the model."

### 8. Auto-Compound

If root cause was non-obvious or this is a recurring pattern:
- Save to `docs/solutions/<category>/<filename>.md`
- Include: symptoms, root cause, solution, prevention
- "💊 Saved to `docs/solutions/...` for future reference"

### ⛔ STOP

> "Bug fixed and verified. Anything else?"
