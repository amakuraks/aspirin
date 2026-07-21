# Aspirin 💊

> **"Relief from engineering headaches."**
> **Version 1.1.0**

Aspirin is a lean, token-efficient AI development framework for **Antigravity IDE**. Built for developers who build Laravel + React dashboards (and beyond), it enforces security compliance, best-practice architecture, and honest feedback — without the bloat.

Forked and reshaped from [Super Compound](https://github.com/aultramen/super-compound) to cut the ceremony and focus on what matters.

---

## Features

| Feature | Description |
|---------|-------------|
| 💊 **Ask-First Brainstorming** | Questions before solutions. No wasted tokens on rejected paths. |
| 📋 **Comprehensive Planning** | Always detailed. Detects messy code and includes restructuring. |
| 🛡️ **Pre-Code Security Gate** | Review plans through OWASP Top 10, ISO 27001, gap analysis, and layered architecture — BEFORE writing code |
| 🔒 **Scoped Re-Reviews** | First review is full scan. Re-reviews check ONLY fixed items — no cascading discovery |
| 🧠 **Auto-Compound Knowledge** | Solved problems auto-save to `docs/solutions/` — no manual trigger needed |
| 🗣️ **Direct Engineer Personality** | No sugarcoating. Bad design gets called out with engineering reasoning. |
| 🎯 **Token Efficiency** | No preamble, no echo, tables over prose, ask don't guess |
| ⛔ **CONTINUE Gate** | Every workflow stops at the end. No auto-advancing without permission. |
| 📖 **TypeScript Helper** | Explains TS patterns for developers learning TypeScript |
| 📚 **Source Citations** | Best-practice recommendations by default. Sources linked when uncertain. |
| ⚠️ **Context Hygiene** | Nudges /compact when estimated context passes ~60% (checked at workflow gates) |
| 🦴 **Caveman Output** | Terse output everywhere — filler dropped, technical terms exact |
| 🗜️ **rtk Integration** | Wraps shell commands with rtk when installed — compressed tool output |
| 🧭 **Clarity Over Cleverness** | Reusability, readability, uniformity > runtime efficiency |

---

## Installation

Aspirin lives inside the `.agents/` directory of your project.

### Option A: Git Clone

```bash
git clone https://github.com/amakuraks/aspirin.git
cd /path/to/your-project
cp -r /path/to/aspirin/.agents ./.agents
```

**Windows (PowerShell):**

```powershell
git clone https://github.com/amakuraks/aspirin.git
cd C:\path\to\your-project
Copy-Item -Recurse "path\to\aspirin\.agents" -Destination ".\.agents" -Force
```

### Option B: Manual Copy

Copy the `.agents/` folder into your project root.

### Project Structure After Installation

```
your-project/
├── .agents/
│   ├── rules/                    ← Always-on constraints (2 files)
│   │   ├── aspirin.md            ← Core philosophy, personality, token rules
│   │   └── project-config.md    ← Tech stack config (auto-filled by /init)
│   ├── workflows/                ← Slash commands (8 workflows)
│   │   ├── brainstorm.md        ← Ask-first exploration
│   │   ├── plan.md              ← Comprehensive implementation plan
│   │   ├── gate.md              ← ⭐ Plan review: gaps + OWASP + ISO + architecture
│   │   ├── work.md              ← Execute plan task by task
│   │   ├── review.md            ← Code review with scoped re-reviews
│   │   ├── init.md              ← Project initialization (multi-stack)
│   │   ├── debug.md             ← Standalone error diagnosis
│   │   └── memory.md            ← Browse / search stored knowledge
│   └── skills/                   ← Reusable capabilities (11 skills)
│       ├── brainstorming/        ← Ask-first brainstorming
│       ├── writing-plans/        ← Comprehensive plan writing
│       ├── executing-plans/      ← Plan execution with TDD
│       ├── code-review/          ← Scoped re-review protocol
│       ├── security-gate/        ← OWASP + ISO 27001 checklists
│       ├── architecture-check/   ← Laravel + React layer validation
│       ├── auto-compound/        ← Auto-detect + save knowledge
│       ├── systematic-debugging/ ← Root-cause debugging
│       ├── verification/         ← Evidence before claims
│       ├── tdd/                  ← Test-driven development
│       └── context7-docs/        ← Library docs lookup
└── docs/
    ├── brainstorms/              ← Brainstorm outputs
    ├── plans/                    ← Implementation plans
    └── solutions/                ← Auto-compounded knowledge
```

---

## Quick Start

### Step 1: Initialize Your Project

```
/init
```

Scans your codebase, detects the tech stack, and auto-fills `project-config.md`.

### Step 2: Start Building

```
/brainstorm    → Explore a feature idea (questions first, solutions later)
/plan          → Create comprehensive implementation plan
/gate          → Review plan for gaps, OWASP, ISO 27001, architecture
/work          → Execute plan task by task with TDD
/review        → Code review with scoped re-reviews
```

**Utilities:**

```
/debug         → Diagnose and fix errors (standalone)
/memory        → Browse stored knowledge from docs/solutions/
/init          → Initialize or re-scan project
```

---

## Core Pipeline

```
/brainstorm → /plan → /gate → /work → /review
```

Every step stops and waits for you. No auto-advancing.

| Phase | Command | What Happens |
|-------|---------|-------------|
| 💡 **Brainstorm** | `/brainstorm` | Ask-first exploration. Questions → options table → design chosen option → enhancement opportunities |
| 📋 **Plan** | `/plan` | Comprehensive plan with exact file paths, complete code, and verification commands. Detects messy code and restructures first. |
| 🛡️ **Gate** | `/gate` | Reviews plan through 4 lenses: gap analysis, OWASP Top 10, ISO 27001 Annex A, layered architecture. Presents findings — never auto-patches. |
| ⚡ **Work** | `/work` | Executes plan task by task. TDD cycle, incremental commits, batch checkpoints every 3 tasks. |
| 🔍 **Review** | `/review` | 7-perspective code review. P1/P2/P3 severity. Re-reviews are scoped to fixed items only. |

---

## Key Behaviors

### Ask-First Brainstorming

The agent asks all clarifying questions FIRST before generating any solution. No wasted tokens on rejected paths.

```
Phase 1: UNDERSTAND  → Questions only. No solutions.
Phase 2: PROPOSE     → 2-3 options table. Best practice first.
Phase 3: DESIGN      → Detail chosen option only.
Phase 4: ENHANCE     → Optional additions with benefits.
```

### Pre-Code Security Gate (`/gate`)

Reviews the plan through **4 lenses** before any code is written:

| Lens | What It Checks |
|------|---------------|
| **Gap Analysis** | Missing CRUD ops, validations, error handling, edge cases, migrations |
| **OWASP Top 10** | A01–A10 applied to the plan (access control, injection, auth, CSRF, etc.) |
| **ISO 27001** | Annex A controls: access control, cryptography, secure dev, logging, compliance |
| **Architecture** | Laravel layers (Route→Controller→Service→Model) + React layers (Page→Component→Hook→API) |

Each finding gets an ID (`#G1`, `#O3`, `#I8`). After patching, `/gate` only re-checks failed IDs — no cascading.

### Scoped Re-Reviews

| Review Type | Behavior |
|-------------|----------|
| **First `/review`** | Full comprehensive scan — all 7 perspectives |
| **Re-review after fixes** | ONLY checks: are the specific findings fixed? Any regressions in changed files? |
| **`/review full`** | Force a complete re-scan (explicit opt-in) |

### Direct Engineer Personality

No sugarcoating. Tone matches severity:

| Severity | Tone |
|----------|------|
| 🟢 P3 | Direct, professional |
| 🟡 P2 | Blunt, explains what's wrong |
| 🔴 P1 | Harsh + swearing to convey criticality |

If the design is wrong, the agent explains WHY and gives you a choice: fix properly or proceed with documented trade-offs.

### Auto-Compound Knowledge

Knowledge auto-saves when non-trivial problems are solved in `/debug`, `/work`, or `/review`. No manual trigger.

```
💊 Saved to docs/solutions/runtime-errors/2026-05-08-lazy-load-violation.md
```

Browse with `/memory`. Search with `/memory search <query>`.

### Inline Mode (No Workflow)

Ask questions directly without invoking any workflow. The agent answers concisely, follows the same personality, and **never modifies files** unless you explicitly ask.

---

## Skills

| Skill | Purpose |
|-------|---------|
| **brainstorming** | Ask-first exploration with best-practice bias |
| **writing-plans** | Comprehensive plans with structure detection |
| **executing-plans** | Plan execution with TDD and batch checkpoints |
| **code-review** | 7-perspective review with scoped re-review protocol |
| **security-gate** | OWASP Top 10 + ISO 27001 Annex A checklists |
| **architecture-check** | Laravel + React layered architecture validation |
| **auto-compound** | Auto-detect and save knowledge when issues are solved |
| **systematic-debugging** | 4-phase root-cause investigation |
| **verification** | Evidence-based completion — no claims without proof |
| **tdd** | Test-driven development (strict / balanced / relaxed) |
| **context7-docs** | Up-to-date library docs via Context7 MCP |

---

## Architecture Enforcement

### Laravel Layers

```
Route → Controller (thin) → FormRequest (validation) → Service (logic) → Model (data)
```

| Layer | Must NOT Contain |
|-------|-----------------|
| Controller | Business logic, direct DB queries |
| Service | HTTP concerns (`request()`, `response()`) |
| Model | Business logic, HTTP concerns |

### React Layers

```
Page → Component (UI) → Hook (logic) → API Client (HTTP) → Interface (types)
```

| Layer | Must NOT Contain |
|-------|-----------------|
| Component | API calls, complex business logic |
| Hook | UI rendering, direct HTTP calls |

Violations are **P1 Critical** in code review.

---

## Philosophy

| Principle | Description |
|-----------|-------------|
| **Ask Before Generate** | If info is missing, ask. Don't speculate. |
| **Best Practice First** | Recommend commonly-used solutions. Cite sources when uncertain. |
| **Evidence > Claims** | Run verification, then claim success. |
| **Fix Root Cause** | If A integrates with B because C is broken — fix C. |
| **Plan Before Code** | Brainstorm → Plan → Gate → then code. |
| **Knowledge Compounds** | Solved problems auto-save for the future. |
| **No Sugarcoating** | Bad design gets called out with engineering reasoning. |

---

## Comparison with Super Compound

| Metric | Super Compound | Aspirin |
|--------|---------------|---------|
| Rule files | 3 | **2** |
| Workflows | 22 (15 + 7 redirects) | **8** (all unique) |
| Skills | 27 | **11** |
| Agents | 5 | **0** |
| Hooks | 3 | **0** |
| Security review | Post-code only | **Pre-code** (`/gate`) |
| Re-review behavior | Full re-scan (cascading) | **Scoped** to findings |
| Knowledge capture | Manual (forgettable) | **Automatic** |
| Workflow advancement | Sometimes auto-continues | **CONTINUE gate** on every step |
| Personality | Neutral/helpful | **Direct + severity-tiered** |
| Token efficiency | Standard | **Optimized** (ask-first, no fluff) |

---

## Origin

Aspirin is reshaped from [Super Compound](https://github.com/aultramen/super-compound), which synthesized ideas from:

- **[Superpowers](https://github.com/obra/superpowers)** — TDD discipline, systematic debugging, verification rigor
- **[Compound Engineering](https://github.com/EveryInc/compound-engineering-plugin)** — Knowledge compounding, multi-depth planning

Aspirin strips the ceremony, adds pre-code security gates, and speaks directly.

---

## License

MIT
