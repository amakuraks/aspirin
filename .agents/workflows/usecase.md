---
description: "Scan and maintain the use case registry. Detects use cases from codebase, groups by domain."
---

# /usecase — Use Case Registry

## Process

### 1. Scan Codebase

Scan for use cases by reading:
- **Routes** (web.php, api.php, React router) → each route implies a use case
- **Controllers** → public methods = user-facing actions
- **Services** → business operations
- **Pages/Views** → user-facing screens
- **Form Requests / Validators** → user input scenarios
- **Tests** → test descriptions often name use cases explicitly

### 2. Build Registry

Group use cases by domain with detailed granularity:

```markdown
## Authentication
- User login
- User logout
- User register
- User forgot password
- User reset password
- Reseller authentication

## Tracks
- Admin create track
- Admin edit track
- Admin delete track
- User view track listing
```

**Granularity rules:**
- Each distinct user action = one use case.
- ✅ "User login" (specific)
- ❌ "User authentication" (too broad)
- Include the actor: "Admin create track", "User view track"

### 3. Cross-Reference

Compare detected use cases against existing `docs/use-cases.md` (if exists):
- **NEW** — detected in code but not in registry
- **MISSING** — in registry but no matching code found
- **UNCHANGED** — matches between code and registry

### 4. Coverage Report

```
## Scan Results

| Domain         | Use Cases | In Registry | In Code | Status     |
|----------------|-----------|-------------|---------|------------|
| Authentication | 5         | 5           | 5       | ✅ Synced   |
| Tracks         | 4         | 3           | 4       | ⚠️ 1 new   |
| Publishing     | 3         | 3           | 2       | ❌ 1 missing |
```

**Coverage honesty:** If the scan could not cover all source files, disclose:
> "Scanned X of Y source files (Z%). Remaining files not reviewed."

### 5. Confirm Changes

For any additions, removals, or modifications to the registry:
- Present the diff to the user.
- **Wait for confirmation before updating `docs/use-cases.md`.**

### 6. Save

Write/update `docs/use-cases.md`

### ⛔ STOP

> "Use case registry updated. Run `/brainstorm` or `/plan` when ready."

**DO NOT proceed to any other workflow.** Wait for user instruction.
