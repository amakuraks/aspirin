---
name: verification
description: "Verify work is complete before claiming done. Used in /work and /review."
---

# Verification Skill

## The Iron Law

**NO COMPLETION CLAIMS WITHOUT FRESH VERIFICATION EVIDENCE.**

## Process

Before claiming ANY work is complete:

1. **IDENTIFY** — What command proves this claim? (test suite, linter, build)
2. **RUN** — Execute the full command (fresh, not cached)
3. **READ** — Check full output, exit code, failure count
4. **VERIFY** — Does the output confirm the claim?
5. **SHOW** — Include the verification output as evidence

## What Counts as Verification

| Claim | Required Evidence |
|-------|------------------|
| "Tests pass" | Full test suite output showing 0 failures |
| "Feature works" | Run the feature, show the result |
| "Bug is fixed" | Run the reproducing test, show it passes |
| "No regressions" | Full test suite passes |
| "Code is clean" | Linter output with 0 errors |

## Red Flags — STOP

- Using "should work", "probably fine", "seems correct"
- Expressing satisfaction before verification ("Done!", "All good!")
- About to commit without running tests
- Claiming success based on reasoning alone (no command output)
