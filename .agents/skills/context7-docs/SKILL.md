---
name: context7-docs
description: "Fetch up-to-date library/API documentation. Use before web search."
---

# Context7 Docs Skill

## Purpose

Get up-to-date documentation for libraries and frameworks. Use this BEFORE falling back to web search.

## Priority Order

```
1. Context7 MCP   → resolve library ID + query docs
   ↓ if unavailable or library not found
2. Official docs  → read_url_content on documentation URL
   ↓ if inaccessible
3. Web search     → search_web as last resort
```

## Usage

### Step 1: Resolve Library ID

Use `mcp_context7_resolve-library-id` with the library name.

Example: "laravel" → gets the Context7-compatible library ID.

### Step 2: Query Documentation

Use `mcp_context7_query-docs` with the library ID and your specific question.

Example: query "sanctum SPA authentication setup" for the Laravel library.

## When to Use

- Before implementing features using a library's API
- When unsure about a framework convention
- When checking version-specific behavior
- When the user asks "how does X work in [library]?"

## Fallback

If Context7 is unavailable:
1. Try `read_url_content` on the official documentation URL
2. If that fails, use `search_web`
3. Always cite the source in your recommendation
