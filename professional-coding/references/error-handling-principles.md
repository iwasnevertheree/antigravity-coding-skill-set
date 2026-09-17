# Error Handling & Boundary Observability

Principles for designing robust, transparent, and resilient error handling across any architecture.

---

## 1. Core Principles

1. **Never swallow errors silently**: Always log, propagate, or transform errors into domain-appropriate responses. Never use empty catch or except blocks.
2. **Fail fast and explicitly**: Validate inputs, invariants, and prerequisites at the boundary before executing logic.
3. **Use domain-specific error types**: Group errors into meaningful domain concepts (e.g., NotFound, Unauthorized, Validation, Conflict) rather than generic exceptions.
4. **Preserve error context**: When wrapping or rethrowing errors, preserve the original cause and stack trace.
5. **Clean up resources**: Ensure file handles, database connections, and locks are released via standard idioms (`finally`, context managers, `defer`, RAII).

---

## 2. Error Categorization Matrix

| Category | Description | Strategy | Example |
|---|---|---|---|
| **Client / Input Error** | Caller provided invalid data | Reject immediately with descriptive message | Malformed payload, missing field, out-of-range value |
| **Operational / Transient** | Temporary external failure | Retry with exponential backoff or degrade gracefully | Network timeout, database connection drop |
| **Programming / Defect** | Bug in application code | Fail fast, log with stack trace, alert | Null/nil dereference, index out of bounds |
| **System / Environmental** | Resource exhaustion or infrastructure failure | Log critical alert, shut down gracefully if unrecoverable | Out of disk space, missing configuration |

---

## 3. Boundary Observability

At system boundaries (HTTP endpoints, RPC handlers, queue consumers, CLI commands):
- **Structured Error Responses**: Return machine-readable error codes and human-readable messages (e.g., `{"error": {"code": "USER_NOT_FOUND", "message": "User 42 does not exist"}}`).
- **Operational Logging**: Log errors at the boundary with contextual metadata (request ID, user ID, operation name). Avoid duplicate logging in inner layers.
- **Sanitize Sensitive Data**: Never expose stack traces, database queries, internal IP addresses, or secrets in client-facing error responses.
