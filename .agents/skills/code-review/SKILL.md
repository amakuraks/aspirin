---
name: code-review
description: "Multi-perspective code review with scoped re-review protocol. Used by /review workflow."
---

# Code Review Skill

## Purpose

Review code from 7 perspectives with severity classification. Re-reviews are SCOPED to prevent cascading issue discovery.

## First Review: Full Scan

### Perspectives

| # | Perspective | What to Check |
|---|-------------|---------------|
| 1 | **Correctness** | Logic errors, null handling, off-by-one, error paths, edge cases |
| 2 | **Design** | SRP violations, DRY violations, tight coupling, God classes/components, naming |
| 3 | **Security** | SQL injection, XSS, CSRF, auth bypass, secrets in code, mass assignment |
| 4 | **Performance** | N+1 queries, missing eager loads, unnecessary loops, missing DB indexes, large payloads |
| 5 | **Readability** | Unclear naming, deep nesting, unnecessary complexity, missing types |
| 6 | **Testing** | Missing tests for critical paths, unreliable tests, missing edge case coverage |
| 7 | **Architecture** | Business logic in controllers, API calls in components, layer violations |

### Classification

Every finding gets:
- **ID**: `#R1`, `#R2`, `#R3`, ...
- **Severity**: P1 (critical) / P2 (important) / P3 (suggestion)
- **File and line**: exact location
- **Fix**: specific recommendation

### Tone

Match severity:
- **P3**: Direct → "Rename this variable."
- **P2**: Blunt → "This is doing too much. Split it."
- **P1**: Harsh + swearing → "WTF? This is an SQL injection waiting to happen."

### Output Format

```
## Strengths
- [What's done well]

## Findings

🔴 P1 Critical:
  #R1: [file:line] — [description]
       Fix: [what to do]

🟡 P2 Important:
  #R2: [file:line] — [description]
       Fix: [what to do]

🟢 P3 Suggestion:
  #R3: [file:line] — [description]
       Fix: [what to do]
```

## Re-Review: SCOPED Protocol

When user fixes findings and asks for re-review:

### Rules

1. **ONLY check previously failed finding IDs** (`#R1`, `#R2`, etc.)
2. For each:
   - Is the specific finding fixed? ✅ / ❌
   - Did the fix introduce a regression in the **same file/function**?
3. **DO NOT discover new issues** on code that already passed.
4. **DO NOT re-scan the entire feature.**

### Output

```
## Re-Review Results

✅ #R1: Fixed — auth check added
✅ #R2: Fixed — logic moved to service
❌ #R3: Not fixed — still using raw SQL
```

### Exception

`/review full` → run a complete full scan again. Default re-review is always scoped.

## Why Scoped Re-Reviews Matter

**The cascade problem:**
1. First review finds 5 issues → user fixes them
2. Re-review scans everything again → finds 3 NEW issues
3. User fixes those → re-review finds 2 MORE
4. Infinite loop. User loses trust.

**The fix:** First review must be comprehensive and thorough. Re-reviews are surgical — verify fixes only.
