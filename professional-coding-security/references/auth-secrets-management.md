# Authentication, Authorization & Secrets Management

Guidelines for access control, cryptographic hygiene, and credential isolation.

---

## 1. Authentication Standards

- **Password Storage**: Always use adaptive salted hashing functions (Argon2id, bcrypt, PBKDF2). Never use fast general-purpose hashes (MD5, SHA-1, SHA-256) for passwords.
- **Session & Token Verification**:
  - JWTs: Strictly verify signatures, issuer (`iss`), audience (`aud`), expiration (`exp`), and reject the `alg: none` header.
  - Cookies: Always enforce `HttpOnly`, `Secure`, and `SameSite=Lax` or `Strict`.
- **Timing Attack Prevention**: Use constant-time comparison (`hmac.compare_digest` or equivalent) when validating tokens, signatures, or password hashes.

---

## 2. Authorization Patterns (RBAC / ABAC)

- **Role-Based Access Control (RBAC)**: Assign permissions to roles; assign roles to users. Check permissions at the service or handler level before executing business logic.
- **Attribute-Based Access Control (ABAC)**: Check contextual attributes (e.g. `resource.owner_id == current_user.id`).
- **Fail Closed**: Default to denying access unless an explicit permission or rule grants it.
- **Avoid Client-Side Authorization**: Client-side UI checks are purely for user experience; server/backend logic must independently verify authorization for every operation.

---

## 3. Secrets Management & Least Privilege

- **Zero Secrets in Code**: No API keys, database credentials, or private certificates hardcoded in source files, tests, or commit history.
- **Configuration Isolation**: Load secrets at runtime from environment variables, secret files, or cloud key-vault providers.
- **Least Privilege**:
  - Microservices and database connections should use roles with the minimal required permissions (e.g. read-only replicas for analytics, dedicated tables for specific services).
  - Do not use administrative / root accounts for routine application connections.
