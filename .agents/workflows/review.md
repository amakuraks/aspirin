---
description: "Multi-perspective code review. Scoped re-reviews prevent cascading issue discovery."
---

# /review — Code Review

## First Review (Full Scan)

### 1. Determine Scope

- What files were changed? (check git diff or ask user)
- Is this a full feature review or specific file review?

### 2. Review Perspectives

Analyze from 8 angles:

| Perspective | Focus |
|-------------|-------|
| ✅ **Correctness** | Logic errors, error handling, edge cases, off-by-one, null handling |
| 🏗️ **Design** | SRP, YAGNI, DRY, coupling, cohesion, naming |
| 🔒 **Security** | Input validation, auth checks, injection, secrets exposure, CSRF |
| ⚡ **Performance** | N+1 queries, unnecessary loops, missing indexes, memory leaks |
| 📖 **Readability** | Naming clarity, code structure, unnecessary complexity |
| 🧪 **Testing** | Coverage gaps, missing edge case tests, test reliability |
| 🏛️ **Architecture** | Layer violations, dependency direction, separation of concerns |
| 📋 **Use Cases** | Changes match declared affected use cases, no undeclared use case regressions |

### 3. Classify Findings

Each finding gets:
- **Unique ID**: `#R1`, `#R2`, `#R3`, ...
- **Severity**:
  - 🔴 **P1 Critical** — Must fix. Bugs, security holes, data loss risks.
  - 🟡 **P2 Important** — Should fix. Design issues, missing edge cases, performance.
  - 🟢 **P3 Suggestion** — Nice to have. Style, naming, minor improvements.

### 4. Tone Matches Severity

- **P3**: Direct → "Rename `data` to `userTransactions`."
- **P2**: Blunt → "This controller method is 200 lines. That's not a controller, that's a service pretending to be one. Extract the logic."
- **P1**: Harsh → "WTF? You're passing raw user input into a shell command. That's a remote code execution vulnerability. Fix this NOW."

### 5. Present Review

Structure: strengths first, then findings by severity.

```
## Review Results

### Strengths
- Clean service layer separation
- Good use of FormRequests

### Findings

🔴 P1 Critical (must fix):
  #R1: [file:line] — [description]
       Fix: [specific fix]

🟡 P2 Important (should fix):
  #R2: [file:line] — [description]
       Fix: [specific fix]

🟢 P3 Suggestion:
  #R3: [file:line] — [description]
       Fix: [specific fix]

### Use Case Cross-Check
- Do code changes match the declared `AFFECTED USE CASES` from the plan?
- Flag any use case that appears impacted by code but was NOT listed.
```

---

## Re-Review (After Fixes) — SCOPED

When the user fixes findings and asks for re-review:

### Rules

1. **ONLY check the specific finding IDs** (`#R1`, `#R2`, etc.)
2. For each finding:
   - Is it actually fixed? ✅ / ❌
   - Did the fix introduce a regression **in the same file/function**? Check only the changed area.
3. **DO NOT scan for new issues** on code that already passed.
4. **DO NOT re-review the entire feature** — only the patched areas.

### Exception

If the user explicitly says `/review full` — run a complete full scan again. But the default re-review is always scoped.

### Why This Matters

Without scoped re-reviews, each pass finds new issues → fixes → new issues → infinite loop. The first full scan must be comprehensive. Re-reviews must be surgical.

---

## Auto-Compound

If P1 critical issues were found and fixed:
- Auto-save the pattern to `docs/solutions/<category>/<filename>.md`
- Include: what was wrong, why it's dangerous, how it was fixed
- Mention: "💊 Saved to `docs/solutions/...` for future reference"

## Handoff

All P1/P2 fixed:
> "Review passed ✅. Feature ready."

Has outstanding P1:
> "Review has [N] critical issues remaining. Fix them and run `/review` again."

**⛔ STOP after presenting findings. Wait for user action.**
