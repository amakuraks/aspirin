---
description: "Initialize project context. Stack-agnostic. Scans codebase, auto-fills config."
---

# /init — Project Initialization

## Process

### 1. Scan Package Files

Look for and read (if they exist):
- `composer.json` → PHP/Laravel deps, scripts
- `package.json` → Node.js deps, scripts, framework hints
- `requirements.txt` / `pyproject.toml` → Python deps
- `go.mod` → Go modules
- `Cargo.toml` → Rust crates

### 2. Scan Framework Markers

Check for existence of:
- `artisan` → Laravel
- `next.config.*` → Next.js
- `vite.config.*` → Vite
- `manage.py` → Django
- `angular.json` → Angular
- `tsconfig.json` → TypeScript project

### 3. Scan Config Files

- `.env.example` → environment variables (**never read `.env` itself**)
- `docker-compose.yml` / `Dockerfile` → container setup
- `tailwind.config.*` → Tailwind CSS
- Framework-specific configs

### 4. Scan Directory Structure

List top 3 levels of the project tree. Identify:
- Source directories (src/, app/, resources/, components/)
- Test directories (tests/, __tests__/)
- Config/infra (.github/, docker/)
- Documentation (docs/, README)

### 5. Detect Tooling

- Run `rtk --version` (or `where rtk` / `which rtk`).
- Found → propose `rtk_available: true`. Not found → `rtk_available: false`.
- Detection only proposes — the value is written after user confirms in step 7 (PATH hit ≠ user chose to install it).
- Never fail /init over this — rtk is optional.

### 6. Auto-Fill Config

Update `.agents/rules/project-config.md` with detected values.
- Only fill empty fields
- Never overwrite user-set values
- Present the detected config to the user before applying

### 7. Present Results

```
## Project Detected

| Field | Value |
|-------|-------|
| Type | fullstack |
| Backend | Laravel (PHP 8.x) |
| Frontend | React (TypeScript) |
| Styling | Tailwind CSS |
| Database | MySQL |
| Auth | Sanctum |
| rtk | available / not found |

Config changes: [show diff]

Does this look correct? Any corrections?
```

### ⛔ STOP

> "Project initialized. What would you like to work on?"

Wait for user instruction.