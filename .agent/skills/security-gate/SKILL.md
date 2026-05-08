---
name: security-gate
description: "OWASP Top 10 and ISO 27001 Annex A checklists for plan review. Used by /gate workflow."
---

# Security Gate Skill

## Purpose

Review implementation plans for security and compliance gaps BEFORE any code is written. Catching issues in the plan is cheaper than catching them in code.

## OWASP Top 10 Checklist

For each planned endpoint, feature, or data flow:

### A01: Broken Access Control
- [ ] Role/permission checks on every protected endpoint?
- [ ] Insecure Direct Object Reference (IDOR) protection? (e.g., user can only access own resources)
- [ ] Function-level access control? (admin routes protected)
- [ ] No forced browsing to unauthorized pages?

### A02: Cryptographic Failures
- [ ] Sensitive data encrypted at rest? (passwords, tokens, PII)
- [ ] HTTPS enforced for all data in transit?
- [ ] Proper hashing for passwords? (bcrypt / Argon2, NOT MD5/SHA1)
- [ ] No sensitive data in URLs or query parameters?

### A03: Injection
- [ ] Parameterized queries / Eloquent ORM for all DB access?
- [ ] No raw SQL with user input (`DB::raw()` with variables)?
- [ ] Input sanitization for dynamic content?
- [ ] No `eval()`, `exec()`, or similar with user input?

### A04: Insecure Design
- [ ] Business logic abuse scenarios considered?
- [ ] Rate limiting on sensitive operations? (login, registration, password reset)
- [ ] Account enumeration prevention? (same response for valid/invalid accounts)
- [ ] Proper error messages? (no stack traces in production)

### A05: Security Misconfiguration
- [ ] CORS properly restricted? (not `*` in production)
- [ ] Debug mode disabled in production?
- [ ] Security headers planned? (X-Frame-Options, X-Content-Type-Options, CSP)
- [ ] Default credentials removed?

### A06: Vulnerable Components
- [ ] Dependencies reasonably up-to-date?
- [ ] No known CVEs in used packages?
- [ ] Unused dependencies removed?

### A07: Authentication Failures
- [ ] Session management secure? (httpOnly, Secure, SameSite cookies)
- [ ] Password policy enforced? (min length, complexity)
- [ ] Brute force protection? (lockout, rate limiting)
- [ ] Token rotation on privilege change?

### A08: Data Integrity
- [ ] CSRF protection enabled? (Laravel's `@csrf` / Sanctum)
- [ ] Mass assignment protection? (`$fillable` / `$guarded` in models)
- [ ] Data validation server-side? (never trust client-only validation)

### A09: Logging Failures
- [ ] Security events logged? (failed logins, permission denials, data changes)
- [ ] No PII in log files? (passwords, tokens, personal data)
- [ ] Log injection prevention? (sanitize user input before logging)

### A10: SSRF
- [ ] No user-controlled URLs used in server-side requests?
- [ ] URL allowlisting for external service calls?
- [ ] No internal network access via user input?

---

## ISO 27001 Annex A Checklist

### A.5 — Information Security Policies
- [ ] Security requirements documented or referenced in the plan?

### A.6 — Organization of Information Security
- [ ] Responsibilities clear? Who maintains what?
- [ ] Separation of duties where needed? (e.g., admin vs regular user)

### A.8 — Asset Management
- [ ] Data handled by this feature classified? (public / internal / confidential / restricted)
- [ ] Data retention considered? (how long to keep, when to delete)

### A.9 — Access Control
- [ ] Least privilege principle? (users get minimum required access)
- [ ] Role-based access control (RBAC) used?
- [ ] No hardcoded admin bypasses or backdoors?
- [ ] Access revocation handled? (what happens when roles change)

### A.10 — Cryptography
- [ ] Approved encryption algorithms? (AES-256 for data, bcrypt/Argon2 for passwords)
- [ ] Key management addressed? (where keys are stored, rotation)
- [ ] No custom cryptography implementations?

### A.12 — Operations Security
- [ ] Logging and monitoring planned?
- [ ] Backup strategy for new data?
- [ ] Change management process? (migrations, rollback plan)

### A.14 — Secure Development
- [ ] Secure coding practices followed in the plan?
- [ ] Input validation at every boundary?
- [ ] Output encoding for displayed data?
- [ ] Test cases include security scenarios?

### A.16 — Incident Management
- [ ] Error reporting planned? (how errors surface to admins)
- [ ] Alerting for critical failures?
- [ ] Audit trail for sensitive operations? (who did what, when)

### A.18 — Compliance
- [ ] Regulatory requirements addressed? (GDPR, local data laws)
- [ ] Data subject rights considered? (access, delete, export — if PII)

---

## Output

For each checklist item that fails:

```
❌ #O3: A03 Injection — Task 3.2 uses DB::raw() with $request->input('search')
   without parameterization. This is a SQL injection vector.
   Fix: Use Eloquent query builder with ->where('name', 'like', '%'.$search.'%')
   or parameterized DB::raw('... WHERE name LIKE ?', [$search])
```

## Re-Gate Protocol

When re-checking after patches:
1. Only re-check previously **failed** finding IDs
2. Do NOT discover new issues on passing items
3. `/gate full` forces a complete re-scan
