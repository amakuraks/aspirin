---
name: brainstorming
description: "Ask-first feature exploration. Questions before solutions. Used by /brainstorm workflow."
---

# Brainstorming Skill

## Purpose

Explore feature ideas with **minimal token waste**. Ask questions first, generate solutions only after understanding the full picture.

## Process

### Phase 1: UNDERSTAND (No Solutions)

1. **Read the user's request** — identify what they want to build.
2. **Scan codebase** — look for:
   - Existing implementations of similar features
   - Current patterns and conventions
   - Potential conflicts with existing code
3. **Identify unknowns** — what's missing from the request?
   - Scope: How much of this needs to be built?
   - Data: What data model changes are needed?
   - Auth: Who can access this feature?
   - UX: What does the user flow look like?
   - Integration: How does this connect to existing features?
4. **Ask ALL questions in ONE message** — batch them.
5. **DO NOT propose solutions.** Only provide context that helps the user decide.

### Phase 2: PROPOSE (Brief Options)

After user answers:

1. Present **2-3 options** in a comparison table.
2. **Lead with best practice.** The commonly-used approach goes first.
3. For each option: approach, pros, cons, whether it's best practice.
4. If an option is unconventional → web search for a source/article and link it.
5. If the user's existing code has issues → flag them directly:
   - P2 (design issue): Blunt explanation.
   - P1 (critical mess): Harsh call-out.

### Phase 3: DESIGN (Chosen Option Only)

After user picks an option:

1. Detail **ONLY the chosen option.** No wasted tokens on rejected paths.
2. For **backend (Laravel)**:
   - Database changes (new tables, columns, relationships)
   - Service layer flow (what the service does, inputs, outputs)
   - API endpoints (method, path, request/response shape)
3. For **frontend (React)**:
   - Component hierarchy (parent → child breakdown)
   - State management (local, context, or global)
   - Data flow (how data moves from API to UI)
4. Keep it concise — tables and bullets, not paragraphs.

### Phase 4: ENHANCEMENT OPPORTUNITIES

Before handoff, present optional additions:

| # | Enhancement | Benefit | Priority |
|---|-------------|---------|----------|
| 1 | [enhancement] | [benefit] | Recommended / Optional / Nice to have |

Ask: "Include any of these in the plan? (numbers, 'all', or 'none')"

## Rules

- Never generate solutions before Phase 2.
- Always lead with best practice / commonly-used approach.
- Cite sources when recommending less common solutions.
- Call out bad existing design directly.
- Batch all questions — don't ask one at a time.
