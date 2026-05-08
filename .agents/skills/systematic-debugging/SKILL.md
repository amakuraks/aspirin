---
name: systematic-debugging
description: "Root-cause investigation before fixes. Used by /debug workflow."
---

# Systematic Debugging Skill

## Process

1. **Check knowledge first** — Search `docs/solutions/` for matching symptoms.
2. **Read the full error** — Don't skim. Read the complete stack trace.
3. **Ask if unclear** — Don't guess. Batch all questions in one message.
4. **Reproduce** — Confirm the error is reproducible. If not, gather more data.
5. **Isolate the layer** — DB? Backend? Frontend? Network?
6. **Trace data flow** — Follow data from entry point to error location.
7. **Check recent changes** — `git diff`, `git log --oneline -10`.
8. **Hypothesize** — "Bug caused by [X] because [evidence]." Max 3 hypotheses.
9. **Test smallest change** — Confirm/deny each hypothesis with minimal modification.
10. **Fix root cause** — Not symptoms. If design is bad, say so.

## Rules

| Rule | Description |
|------|-------------|
| **Never guess** | If uncertain, ask or add logging |
| **Root cause only** | Don't patch symptoms. Fix the real problem. |
| **Max 3 hypotheses** | If all fail, question your assumptions |
| **Direct feedback** | If the bug exists because of bad design, say so bluntly |
| **Auto-compound** | If root cause was non-obvious, save to `docs/solutions/` |
