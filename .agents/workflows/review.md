---
description: "Multi-perspective code review. Scoped re-reviews prevent cascading issue discovery."
---

# /review — Code Review

## Response Style

- **Engaging.** No dry dumps. Lead with the verdict, then the findings. Keep momentum.
- **Separate informing from asking.** Information and questions are distinct blocks. Never bury a question mid-finding. When you ask, ask explicitly with `❓`.
- **One question per block.** If multiple questions, number them: `❓ 1) ... ❓ 2) ...`
- **End with a clear next action** — what the user should do or decide next.

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
- **SONE** — is this a quick, obvious, low-effort fix? Mark `SONE: ✅` or `SONE: ❌` (see SONE Fixes below).

### 4. Tone Matches Severity

- **P3**: Direct → "Rename `data` to `userTransactions`."
- **P2**: Blunt → "This controller method is 200 lines. That's not a controller, that's a service pretending to be one. Extract the logic."
- **P1**: Harsh → "WTF? You're passing raw user input into a shell command. That's a remote code execution vulnerability. Fix this NOW."

### 5. Present Review

Structure: strengths first, then findings by severity. Each finding is ONE combined block — technical + layman together.

```
## Review Results

### Strengths
- Clean service layer separation
- Good use of FormRequests

### Findings

🔴 P1 Critical (must fix):
  #R1: [file:line] — [description]
       SONE: ✅ / ❌
       What's wrong: [technical description]
       Layman issue: [plain translation, why it matters]
       Fix: [specific fix]
       Layman fix: [plain translation of the fix]

🟡 P2 Important (should fix):
  #R2: [file:line] — [description]
       SONE: ✅ / ❌
       What's wrong: [technical description]
       Layman issue: [plain translation, why it matters]
       Fix: [specific fix]
       Layman fix: [plain translation of the fix]

🟢 P3 Suggestion:
  #R3: [file:line] — [description]
       SONE: ✅ / ❌
       What's wrong: [technical description]
       Layman issue: [plain translation, why it matters]
       Fix: [specific fix]
       Layman fix: [plain translation of the fix]

### Use Case Cross-Check
- Do code changes match the declared `AFFECTED USE CASES` from the plan?
- Flag any use case that appears impacted by code but was NOT listed.
```

Each block pairs technical detail with its layman translation:
- **What's wrong / Fix** — technical, for the engineer
- **Layman issue / Layman fix** — same content in plain language: no jargon (no "endpoint", "migration", "DTO", "N+1", "CSRF"), no framework names (say "the backend", "the frontend", not "Laravel", "React"), one plain sentence on why it matters. Tone matches severity but stays plain — a critical issue is still called critical.
- **One-to-one mapping.** Every finding MUST have its layman pair — same ID, same order, nothing merged or dropped.
- **SONE** — mark each finding `SONE: ✅` or `SONE: ❌` (see below).

## SONE Fixes

**SONE** = **S**traightforward, **O**bvious, **N**o-brainer, **E**ffortless. Findings that can be taken out quickly — no design debate, no cross-cutting impact.

### SONE Criteria (all must hold)

| Criterion | Meaning |
|-----------|---------|
| **Straightforward** | Single clear action. No ambiguity about what to change. |
| **Obvious** | The correct fix is self-evident. No alternatives worth weighing. |
| **No-brainer** | No trade-offs, no risk of breaking other things, nobody could reasonably object. |
| **Effortless** | Low effort, small change, no new architecture, no new dependencies. |

### SONE Examples

- **Logs currently store PII** → take PII out of the logs completely; or recommend storing it somewhere else / in a different (anonymized/hashed) form.
- **Missing input validation** → add the validation rule (FormRequest).
- **Missing null check on a nullable input** → add the guard before the value is used.
- **Hardcoded secret / debug flag left on** → remove it, move to env/config.
- **Obvious rename / style issue** → rename it (P3 naming fixes are usually SONE).
- **Use case registry out of sync with code** → update the affected use case in `docs/use-cases.md`. (Exception: if use cases haven't been generated yet, it's NOT SONE — flag, don't create a registry on the spot.)
- Any finding with a single canonical fix that can't reasonably be objected to.

### SONE Handling

1. For SONE findings: present the fix AND offer to apply it immediately: "SONE — want me to take this one out now?"
2. **Wait for explicit user approval. The STOP rule stands: NEVER auto-fix.**
3. SONE fixes can be applied immediately, before the rest of the findings are addressed — the review list shrinks fast.
4. Non-SONE findings keep the normal flow: fix → scoped `/review` re-check.

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

SONE findings approved:
> "Taking out [N] SONE fix(es) now: [list of IDs]. Then re-check with `/review`."

**⛔ STOP after presenting findings. Wait for user action.**
