---
name: auto-compound
description: "Automatically detect and save knowledge when non-trivial problems are solved. Embedded in /debug, /work, /review."
---

# Auto-Compound Skill

## Purpose

Automatically capture solved problems as searchable docs. No manual trigger — embedded in `/debug`, `/work`, `/review`.

## Before Solving: Check First

**ALWAYS** search `docs/solutions/` before investigating any error. If match found, apply known fix.

## Trigger Conditions

Auto-save when:
- Multiple investigation steps needed
- Root cause was non-obvious
- Same pattern seen before (recurring)
- User says "save this" / "remember this"

**Don't save:** Typos, missing imports, simple syntax errors.

## Save To

`docs/solutions/<category>/YYYY-MM-DD-<description>.md`

Categories: `build-errors`, `test-failures`, `runtime-errors`, `performance-issues`, `database-issues`, `security-issues`, `ui-bugs`, `integration-issues`, `logic-errors`, `config-issues`

## Template

```markdown
---
date: YYYY-MM-DD
category: <category>
severity: critical | high | medium | low
tags: [tag1, tag2]
---
# [Problem Title]
## Symptoms
## Root Cause
## Solution
## What Didn't Work
## Prevention
```

## Rules

1. Don't ask permission — save and mention: "💊 Saved to `docs/solutions/...`"
2. Always check existing knowledge first before investigating.
3. Cross-reference related entries.
