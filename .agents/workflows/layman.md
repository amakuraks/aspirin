---
description: "Explain the gated plan in layman's terms. Plain English, no jargon. Bridges /gate and /work."
---

# /layman — Plain-English Plan Explanation

Explains the gated, approved implementation plan in **layman's terms** — no jargon, no framework names, no acronyms. Anyone (product owner, stakeholder, non-technical teammate, future self) can read this and understand what is about to be built, why, and what it will change.

## Response Style

- **Engaging.** No dry dumps. Lead with the one-sentence what-are-we-building, then the details.
- **Separate informing from asking.** Information and questions are distinct blocks. Never bury a question mid-explanation. When you ask, ask explicitly with `❓`.
- **One question per block.** If multiple questions, number them: `❓ 1) ... ❓ 2) ...`
- **End with a clear next action** — what the user should do or decide next.

## When

Run **after `/gate` passes** and **before `/work`**. The plan is already reviewed and gated — this step translates it, it does not re-review it.

## Input

Read the **gated plan** from `docs/plans/` (the one that passed `/gate`).

## Process

### 1. Load the Gated Plan

- Read the plan document from `docs/plans/`.
- If multiple plans exist, ask which one to explain (or use the one that was just gated).
- Confirm it has passed `/gate`. If not gated yet: "Plan is not gated yet. Run `/gate` first — I don't translate unapproved plans."

### 2. Translate to Layman's Terms

Explain the plan as if to a smart non-developer. Use the **7 rules of plain language**:

1. **No jargon** — replace every technical term with plain words. No "ORM", "endpoint", "middleware", "DTO", "migration".
2. **No acronyms** — spell out or drop OWASP, ISO, CRUD, API, SQL, TDD. Say "a checklist for common security problems" instead of "OWASP Top 10".
3. **Analogies over definitions** — "A migration is like a blueprint that tells the database how to change its storage room." Use at most one analogy per concept.
4. **Short sentences** — one idea per sentence. If a sentence needs a comma plus a dash, split it.
5. **Concrete examples** — show what the user will actually see or do after this is built. "After this, when a user clicks 'Delete', a popup asks 'Are you sure?' before anything is removed."
6. **No framework names** — no "Laravel", "React", "TypeScript", "PHP". Say "the backend", "the frontend", "the code that runs on the server".
7. **Explain WHY, not just WHAT** — each major piece gets a one-line reason. "We add a confirmation popup so people don't accidentally delete their work."

### 3. Output Format

Present the explanation in this structure:

```
## What We're Building (Layman's Terms)

One or two plain sentences: what this feature does, end to end.

## What Changes for the User

- What the user will SEE (new screens, buttons, messages)
- What the user can DO that they couldn't before
- What the user should NOT expect (deliberately out of scope)

## What Changes Under the Hood (Plain English)

- "The database stores a new piece of info: ..."
- "The server checks ... before allowing ..."
- "The frontend now shows ... when ..."
- Each bullet: what + why, in plain words.

## How It's Built (No Jargon)

- Step 1: ... (plain description of each phase/task group)
- Step 2: ...
- Step 3: ...

## Risks & Things We Checked

- Plain-language summary of what `/gate` reviewed and what it found.
- Any accepted risks or open questions — in plain words.
- "The plan was checked against security best practices and common pitfalls. It passed. These are the remaining things to watch: ..."

## Questions This Plan Answers

- Q: "Will this break existing features?" → A: "No. Existing screens stay the same; new stuff is added on top."
- (Write 2–3 real Q&A the user/stakeholder is likely to ask.)
```

### 4. Keep It Aligned

- The explanation must match the gated plan **exactly**. No additions, no omissions, no new suggestions.
- If the plan has open questions, surface them in "Risks & Things We Checked" — do NOT invent answers.
- If something in the plan is genuinely confusing, ask the user for clarification instead of guessing.

### 5. Save (Optional)

If the user wants a copy, save to `docs/plans/<plan-name>-layman.md`. Otherwise present in chat only.

### ⛔ STOP

> "Plain-English explanation ready. Run `/work` when ready to start building."

**DO NOT proceed to `/work`. Wait for explicit user instruction.**
