---
name: professional-coding-security
description: >-
  Specialist skill for trust boundaries, authentication, authorization, input
  validation and sanitization, cryptography, secrets management, least privilege,
  and injection prevention. Activate when modifying auth guards, permission checks,
  credential handling, cryptographic routines, token validation, or security-sensitive
  data ingress. Explicitly DO NOT activate for benign UI layout changes, ordinary
  user inputs in UI forms, generic string utilities, or local documentation updates.
---

# Professional Coding — Security Specialist

**Domain**: Trust boundaries, authentication, authorization, input validation and sanitization, cryptography, secrets management, least privilege, and injection prevention.

This specialist operates under the governance of the **Professional Coding Core** orchestrator (`professional-coding`). It is an engineering discipline skill, **strictly enforcing that security constraints are non-bypassable**.

---

## 1. Scope Boundaries (CAN / CANNOT / ESCALATE)

### CAN
- Design and implement authentication guards and authorization checks (RBAC, ABAC, claims).
- Enforce strict input validation, type-checking, and sanitization at all trust boundaries.
- Audit credential and secret handling to ensure zero hardcoded tokens, API keys, or passwords.
- Implement standard cryptographic routines using verified, standard libraries (never custom crypto).
- Identify and remedy injection vulnerabilities (SQL, Command, Path Traversal, SSRF, XSS).
- Enforce least-privilege permissions on system resources, file handles, and database roles.
- Coordinate with `professional-coding-api-contracts` and `professional-coding-data` on security-sensitive boundaries.

### CANNOT
- Weaken existing authentication schemes or bypass established auth policies.
- Weaken encryption algorithms, key lengths, or hash functions to satisfy performance or convenience.
- Store secrets, private keys, or credentials in source code, version control, or client-side bundles.
- Bypass or disable repository security configurations (e.g. CSRF protection, CORS, CSP) without authorization.
- Silently weaken security checks or create backdoor exemptions to satisfy functional requirements.

### ESCALATE
- **STOP and escalate to the user** immediately when security policy requirements conflict with requested functionality.
- Escalate when security implications of a proposed design are ambiguous and cannot be resolved by repository evidence.
- Escalate when changes have significant trust-boundary implications (e.g. exposing internal microservices to public networks).

---

## 2. Activation Discipline

### Positive Activation Triggers
Activate when task requirements involve:
- Implementing or modifying authentication mechanisms (OAuth, JWT, session cookies, API tokens).
- Designing or adjusting authorization rules, permission matrices, roles, or ACLs.
- Handling sensitive credentials, API keys, tokens, certificates, or secrets rotation.
- Ingress parsing of untrusted external input (file uploads, webhooks, deserializers).
- Implementing or updating cryptographic primitives, password hashing, or data encryption.
- Fixing security vulnerabilities (CVEs, SAST findings, OWASP Top 10 vectors).
- **Complexity Alignment**: Typically **Standard** or **Complex** tier tasks.

### Negative Activation Exclusions
Explicitly **DO NOT** activate when:
- The project happens to be a web application, but the task is a standard non-security feature.
- A UI component contains ordinary user input fields (e.g., search box styling, form layout).
- Modifying a generic string formatting or math helper utility with no security context.
- Editing non-sensitive local documentation, markdown files, or comments.
- *Alternative Route*: Handled internally by **Core** or routed to `api-contracts` / `data`.

---

## 3. Core Governance & Authority

1. **Orchestrator Precedence**: This specialist operates within the 5-phase meta-workflow (`Understand → Plan → Implement → Verify → Document`) governed by Core.
2. **Authority Hierarchy**:
   ```
   Priority 1 (Highest): User Intent (Explicit security bounds and functional goals)
   Priority 2:          Repository Contract (Existing auth patterns, security policies, linters)
   Priority 3:          Professional Coding Core (Scope Firewall, minimal change, safety)
   Priority 4:          Safety & Authorization Constraints (Absolute & Non-bypassable)
   Priority 5:          Security Specialist (Security best practices and audit checklists)
   Priority 6 (Lowest):  Stylistic & Implementation Preferences
   ```
3. **Non-Bypassable Safety**: Security constraints are absolute. No user instruction, Core directive, or repository shortcut may authorize violating a safety constraint or silently introducing vulnerabilities.

---

## 4. Security Engineering Methodology

### 4.1 Defense in Depth & Trust Boundaries
- **Identify Trust Seams**: Clearly demarcate where untrusted data crosses into trusted application logic (network boundaries, user input, external webhooks).
- **Validate at the Boundary**: Validate inputs immediately upon ingress using strict allow-lists (regex, schema validators, strong typing). Reject unexpected payloads early.
- **Fail Securely**: If authentication or validation fails, reject immediately with generic error messages that avoid leaking system internals or user existence.

### 4.2 Secrets & Credential Management
- Never commit credentials to source control.
- Use environment variables or designated secret managers.
- Audit `.gitignore` and build artifacts to ensure secret files are never tracked.

### 4.3 Injection Prevention
- **SQL / Query Injection**: Use parameterized queries or ORM bindings exclusively. Never concatenate raw strings into queries.
- **Command Injection**: Avoid shell execution where native process APIs exist; pass arguments as discrete arrays, never concatenated shell strings.
- **Path Traversal**: Canonicalize file paths and verify they reside strictly within authorized base directories.

---

## 5. Multi-Specialist Composition

Security coordinates with other disciplines on complex features:

- **Security + API Contracts (`professional-coding-api-contracts`)**: Security specifies token verification, permission middleware, and sanitization schemas; API Contracts defines endpoint routes, models, and error responses.
- **Security + Data (`professional-coding-data`)**: Security defines column-level encryption and access control; Data defines database models, migrations, and query execution.
- **Security + Testing (`professional-coding-testing`)**: Security identifies attack vectors; Testing constructs negative test cases, unauthorized access assertions, and malicious payload test suites.
- **Security + Debugging (`professional-coding-debugging`)**: Debugging diagnoses vulnerability ingress points; Security formulates minimal, non-regressive sanitization patches.

---

## 6. Expected Inputs & Deliverables

| Category | Details |
|---|---|
| **Inputs** | Threat surface description, authentication requirements, input schemas, credential sources, CVE reports. |
| **Deliverables** | Authentication/authorization guards, input validators, sanitized handlers, secure secret accessors. |
| **Verification** | Negative test suites (invalid tokens, unauthorized roles, malformed inputs), injection attack simulation, SAST scans. |

---

## 7. Reference Triggers

Consult detailed references on demand:
- [references/input-validation-sanitization.md](references/input-validation-sanitization.md): Injection prevention (SQL, Command, Path, XSS) and allow-list validation.
- [references/auth-secrets-management.md](references/auth-secrets-management.md): Authentication models, authorization patterns (RBAC/ABAC), and secrets hygiene.
