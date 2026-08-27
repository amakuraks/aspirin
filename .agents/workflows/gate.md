---
description: "Review plan before coding. 5 lenses: gap analysis, OWASP Top 10, ISO 27001, layered architecture, use case impact. Never auto-patches."
---

# /gate — Plan Review

Reviews the implementation plan through 5 security and quality lenses BEFORE any code is written. Catches issues in the plan — not in the code.

## Input

Read the plan document from `docs/plans/`. If multiple plans exist, ask which one to review.

## Process

Review through all 4 lenses, then present a consolidated report.

### Lens 1: Gap Analysis

Check the plan for completeness:

- [ ] All CRUD operations covered (create, read, update, delete — if applicable)
- [ ] Input validation on every user-facing endpoint
- [ ] Error handling for every operation (success + failure paths)
- [ ] Edge cases identified (empty states, duplicates, limits, concurrency)
- [ ] Database migrations included (with rollback)
- [ ] API endpoints complete (request shape + response shape + status codes)
- [ ] Frontend states covered (loading, error, empty, success)
- [ ] Authorization checks on every protected operation
- [ ] Rollback/undo scenarios considered (soft delete, restore)
- [ ] Pagination for list endpoints

### Lens 2: OWASP Top 10

For each planned endpoint/feature, verify the plan addresses:

- [ ] **A01 Broken Access Control** — Role/permission checks? Route protection? Insecure direct object references?
- [ ] **A02 Cryptographic Failures** — Sensitive data encrypted at rest and transit? HTTPS enforced? Proper hashing?
- [ ] **A03 Injection** — Parameterized queries? Input sanitization? No raw SQL with user input?
- [ ] **A04 Insecure Design** — Business logic abuse scenarios? Rate limiting? Account enumeration prevention?
- [ ] **A05 Security Misconfiguration** — CORS properly restricted? Debug mode off? Security headers?
- [ ] **A06 Vulnerable Components** — Dependencies up-to-date? Known CVEs in used packages?
- [ ] **A07 Auth Failures** — Session management? Password policy? Brute force protection? Token rotation?
- [ ] **A08 Data Integrity** — CSRF protection? Signed/verified data? Mass assignment protection?
- [ ] **A09 Logging Failures** — Security events logged? Failed logins tracked? No PII in logs?
- [ ] **A10 SSRF** — External URL handling sanitized? No user-controlled URLs in server requests?

### Lens 3: ISO 27001 Annex A

Check the plan addresses information security controls:

- [ ] **A.5 Information Security Policies** — Security requirements documented or referenced?
- [ ] **A.6 Organization** — Responsibilities clear? Separation of duties where needed?
- [ ] **A.8 Asset Management** — Data classification defined? (public, internal, confidential, restricted)
- [ ] **A.9 Access Control** — Least privilege enforced? Role-based access? No hardcoded admin bypasses?
- [ ] **A.10 Cryptography** — Approved algorithms used? Key management addressed? (AES-256, bcrypt/Argon2)
- [ ] **A.12 Operations Security** — Logging and monitoring planned? Backup strategy? Change management?
- [ ] **A.14 Secure Development** — Secure coding practices in the plan? Input validation? Output encoding?
- [ ] **A.16 Incident Management** — Error reporting? Alerting for critical failures? Audit trail?
- [ ] **A.18 Compliance** — Regulatory requirements addressed? Data retention policies?

### Lens 4: Layered Architecture

**Laravel layers:**
- [ ] Routes → Controllers (thin controllers, no business logic)
- [ ] FormRequests for validation (not inline in controllers)
- [ ] Services for business logic (not in models or controllers)
- [ ] Models for relationships, scopes, and accessors only
- [ ] No direct DB queries in controllers (`DB::` calls)
- [ ] No HTTP concerns (`request()`, `response()`) in services
- [ ] Repository pattern used for complex queries (optional for simple Eloquent)

**React layers:**
- [ ] Pages → Components (proper decomposition, no 500-line God components)
- [ ] Custom hooks for reusable stateful logic
- [ ] API client layer (axios/fetch abstracted, not inline in components)
- [ ] TypeScript interfaces for all API request/response shapes
- [ ] State management appropriate (local state vs context vs global store)
- [ ] No business logic in JSX (compute in hook, render in component)
- [ ] No direct API calls in components (use hooks or API client)

### Lens 5: Use Case Impact

Cross-reference the plan against `docs/use-cases.md`:

- [ ] All affected use cases explicitly listed in the plan
- [ ] No unlisted use cases are silently modified by the planned changes
- [ ] New use cases have clear acceptance criteria
- [ ] Removed use cases are intentional (not accidental side effects)
- [ ] Test coverage plan addresses all affected use cases (unit + UAT)

## Output Format

Present a consolidated report:

```
┌───────────┬──────┬──────────────────────────────────┐
│ Lens      │ Pass │ Findings                         │
├───────────┼──────┼──────────────────────────────────┤
│ Gaps      │ 8/10 │ ❌ #G1: ... · ❌ #G2: ...       │
│ OWASP     │ 9/10 │ ❌ #O4: ...                      │
│ ISO 27001 │ 7/9  │ ❌ #I8: ... · ❌ #I9: ...       │
│ Arch      │ 6/6  │ ✅ All passed                    │
│ Use Cases │ 4/5  │ ❌ #U2: ...                      │
└───────────┴──────┴──────────────────────────────────┘
```

For each ❌ finding, present ONE combined block — technical + layman together:

```
❌ #O4 — A04 Insecure Design: [short title]
SONE: ✅ / ❌

What's missing: [technical description]
Layman issue: [plain translation of what's missing, and why it matters]

Suggested fix: [technical fix]
Layman fix: [plain translation of the fix]
```

Each block pairs technical detail with its layman translation:
- **What's missing / Suggested fix** — technical, for the engineer
- **Layman issue / Layman fix** — same content in plain language: no jargon, no framework names (say "the backend", "the frontend", not "Laravel", "React"), one plain sentence on why it matters. Written for a smart non-developer (product owner, stakeholder, future self).
- **One-to-one mapping.** Every finding MUST have its layman pair — same ID, same order, nothing merged or dropped.
- **SONE** — mark each finding `SONE: ✅` or `SONE: ❌` (see below).

## SONE Fixes

**SONE** = **S**traightforward, **O**bvious, **N**o-brainer, **E**ffortless. Findings that can be taken out quickly — no design debate, no cross-cutting impact.

### SONE Criteria (all must hold)

| Criterion | Meaning |
|-----------|---------|
| **Straightforward** | Single clear action. No ambiguity about what to change. |
| **Obvious** | The correct fix is self-evident. No alternatives worth weighing. |
| **No-brainer** | No trade-offs, no risk of breaking other things, nobody could reasonably object. |
| **Effortless** | Low effort, small change, no new architecture, no new dependencies. |

### SONE Examples

- **Use case needs updating** → update the affected use case in `docs/use-cases.md`. (Exception: if use cases haven't been generated yet, it's NOT SONE — flag, don't create a registry on the spot.)
- **Logs currently store PII** → take PII out of the logs completely; or recommend storing it somewhere else / in a different (anonymized/hashed) form.
- **Missing input validation on a simple endpoint** → add the validation rule (FormRequest).
- **Missing migration rollback** → add the `down()` step.
- **Missing test for a listed use case** → add the test that the plan already implies.
- Any checklist item from the lenses with a single canonical fix.

### SONE Handling

1. For SONE findings: present the fix AND offer to apply it immediately: "SONE — want me to take this one out now?"
2. **Wait for explicit user approval. Rule 1 stands: NEVER auto-patch.**
3. SONE fixes can be applied immediately, before the rest of the plan is patched — gate report shrinks fast.
4. Non-SONE findings keep the normal flow: patch plan → re-run `/gate` (scoped).

## Rules

1. **NEVER auto-patch the plan** — including SONE fixes. Present findings and wait for user decision.
2. **User decides** what to fix, what to accept as risk, what to skip. SONE findings can be taken out immediately on approval — before the rest of the plan is patched.
3. After user patches the plan, re-run `/gate` — but **SCOPED**:
   - Only re-check the previously failed finding IDs
   - Do NOT discover new issues on items that already passed
   - User can force full re-scan with: `/gate full`
4. Each finding's layman pair must match its technical block exactly — no new findings, no softened language, no omissions. Translate, don't reinterpret.

## Handoff

All passed:
> "Gate passed ✅. Run `/layman` for a plain-English explanation of what's planned, then `/work` when ready."

Has failures:
> "Gate found [N] issues. Review the findings above, patch the plan, then run `/gate` again."

**⛔ DO NOT proceed to `/work`. Wait for explicit user instruction.**
