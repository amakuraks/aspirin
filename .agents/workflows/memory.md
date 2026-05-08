---
description: "Browse and search stored knowledge from docs/solutions/. Read-only."
---

# /memory — Browse Stored Knowledge

## Usage

```
/memory              → List all knowledge, grouped by category
/memory search <q>   → Search for a keyword across all knowledge
/memory <category>   → List knowledge in a specific category
```

## Process

### /memory (list all)

Scan `docs/solutions/` and present:

```
📁 docs/solutions/
├── runtime-errors/ (3 entries)
│   ├── 2026-05-01-lazy-load-issue.md        [high]
│   ├── 2026-04-28-null-relationship.md      [medium]
│   └── 2026-04-15-session-expired.md        [low]
├── build-errors/ (1 entry)
│   └── 2026-04-20-vite-config-conflict.md   [medium]
├── security-issues/ (2 entries)
│   ├── 2026-05-05-csrf-token-mismatch.md    [high]
│   └── 2026-04-22-cors-misconfiguration.md  [medium]
└── database-issues/ (0 entries)

Total: 6 entries across 4 categories
```

### /memory search \<query\>

Search file contents in `docs/solutions/` for the query.

```
🔍 Search: "lazy load"

Results:
1. runtime-errors/2026-05-01-lazy-load-issue.md
   → "LazyLoadViolation in User model when accessing transactions
      without eager loading. Fixed with ->with('transactions')."

2. performance-issues/2026-04-10-n-plus-one.md
   → "Related to lazy loading — N+1 query detected on dashboard..."
```

### /memory \<category\>

List all entries in a specific category with summaries.

```
📁 runtime-errors/ (3 entries)

1. lazy-load-issue.md (2026-05-01) [high]
   Symptoms: LazyLoadViolation exception on User::transactions
   Root cause: Missing eager load in query
   Fix: Add ->with('transactions') to query

2. null-relationship.md (2026-04-28) [medium]
   Symptoms: Trying to get property of null
   Root cause: belongsTo relationship not defined
   Fix: Add belongsTo() in Transaction model
```

## Rules

- **Read-only.** Never modify knowledge files.
- If no `docs/solutions/` directory exists: "No knowledge stored yet. Knowledge is auto-saved when solving non-trivial issues via `/debug`, `/work`, or `/review`."

## Categories

Valid categories: `build-errors`, `test-failures`, `runtime-errors`, `performance-issues`, `database-issues`, `security-issues`, `ui-bugs`, `integration-issues`, `logic-errors`, `config-issues`
