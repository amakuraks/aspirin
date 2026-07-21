# Project Configuration

Customize per project. Leave fields empty for auto-detect via `/init`.

```yaml
# ═══ IDENTITY ═══
project_name: ""
project_type: ""              # fullstack | backend | frontend | cli | library

# ═══ FRONTEND ═══
frontend:
  framework: ""               # react | vue | angular | none
  language: "typescript"      # typescript | javascript
  styling: ""                 # tailwind | bootstrap | vanilla-css
  bundler: ""                 # vite | webpack

# ═══ BACKEND ═══
backend:
  framework: ""               # laravel | express | fastapi | django | gin | none
  language: ""                 # php | typescript | python | go
  orm: ""                      # eloquent | prisma | sqlalchemy | typeorm | gorm

# ═══ DATABASE ═══
database:
  primary: ""                  # mysql | postgresql | sqlite
  cache: ""                    # redis | none

# ═══ AUTH ═══
auth:
  method: ""                   # sanctum | session | jwt | none
  provider: ""                 # custom | laravel-sanctum | none

# ═══ COMMANDS ═══
dev_command: ""
test_command: ""
lint_command: ""
build_command: ""

# ═══ BEHAVIOR ═══
git_workflow: "branch"         # branch | none
default_branch: "main"
tdd_mode: "balanced"           # strict | balanced | relaxed
rtk_available: ""              # true | false — auto-detected by /init
```

## Auto-Detect

When config fields are empty, `/init` scans the project and fills them automatically.

**NEVER start development work with empty critical fields.** If `/init` hasn't been run, ask the user about their stack first.

## Common Stacks

| Stack | Framework | ORM | DB | Auth |
|-------|-----------|-----|----|------|
| Laravel + React | laravel + react | eloquent | mysql/postgresql | sanctum |
| Laravel + Blade | laravel | eloquent | mysql/postgresql | session |
| Express + React | express + react | prisma | postgresql | jwt |
| FastAPI + React | fastapi + react | sqlalchemy | postgresql | jwt |
| Go + React | gin + react | gorm | postgresql | jwt |
