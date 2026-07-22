---
description: "Ask-first feature exploration. Questions before solutions. No wasted tokens on rejected paths."
---

# /brainstorm — Ask-First Exploration

## Process

### Phase 1: UNDERSTAND

1. **Print version** — display: `> 💊 Aspirin v1.1.0`
2. Read the user's request.
2. Scan codebase for relevant existing code, patterns, and conventions (if applicable).
3. **Load use cases** — read `docs/use-cases.md` (if exists) to understand existing system behavior.
4. If ANYTHING is unclear — ask ALL clarifying questions in **ONE message**.
5. **DO NOT generate any solution proposals yet.**
6. Only generate detail that helps the user make a decision (e.g., "Option A means X, Option B means Y — which do you prefer?").
7. Gate: Wait for user to answer.

### Phase 2: PROPOSE

1. Present 2-3 options as a table:

   | Option | Approach | Pros | Cons | Best Practice? |
   |--------|----------|------|------|----------------|
   | A | ... | ... | ... | ✅ Yes |
   | B | ... | ... | ... | ❌ Less common |

2. **Textbook-first** — lead with battle-proven patterns. If the textbook solution exists, it goes in Option A. Novel approaches require justification + source.
3. Flag unconventional options — provide source/article link as evidence.
4. If the user's existing design has structural issues — **say so directly here.**
   - Use severity-appropriate tone (P2 blunt or P1 harsh if it's bad).
5. For each option, list affected use cases:
   ```
   AFFECTED USE CASES: user login, user reset password
   ```
6. Gate: Wait for user to pick an option.

### Phase 3: DESIGN

1. Detail **ONLY the chosen option.** Don't waste tokens on rejected paths.
2. Include:
   - Data flow (how data moves through the system)
   - Key components and their responsibilities
   - API shape (endpoints, request/response)
3. For **frontend:** component hierarchy + state management approach.
4. For **backend:** service layer flow + database changes (migrations, models).
5. Keep it concise — tables and bullet points, not essays.
6. List all affected use cases consolidated:
   ```
   AFFECTED USE CASES (consolidated):
   - MODIFIED: user login, user registration
   - NEW: user two-factor authentication
   - REMOVED: (none)
   ```

### Phase 4: ENHANCEMENT OPPORTUNITIES

Present what COULD be added beyond the core feature:

| # | Enhancement | Benefit | Priority |
|---|-------------|---------|----------|
| 1 | Rate limiting | Prevents API abuse | Recommended |
| 2 | Soft deletes | Recoverable data | Optional |
| 3 | Activity logging | Audit trail | Optional |

Ask: "Include any of these? (list numbers, 'all', or 'none')"

Gate: Wait for user response.

### Save

Write brainstorm output to `docs/brainstorms/YYYY-MM-DD-<topic>.md`

### ⛔ STOP

> "Brainstorm complete. Run `/plan` when ready."

**DO NOT proceed to `/plan`.** Wait for user to say CONTINUE or invoke `/plan`.
