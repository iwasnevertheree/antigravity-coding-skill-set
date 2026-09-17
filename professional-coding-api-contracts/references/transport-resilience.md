# Network Transport Resilience: Timeouts, Retries & Circuit Breakers

Patterns for building reliable client and server communication across unreliable networks.

---

## 1. Timeout Strategies

- **Never rely on default / infinite timeouts**: Every HTTP client or RPC stub must specify explicit connect, read, and write timeouts.
- **Fail Fast**: Set connect timeouts aggressively (e.g. 1-2 seconds) to avoid tying up worker threads on dead endpoints.
- **Deadline Propagation**: In multi-tier distributed calls, propagate the remaining request deadline (e.g. `grpc-timeout` or `Request-Timeout`) so downstream services cancel work if the client has already given up.

---

## 2. Exponential Backoff with Jitter

When retrying transient network errors (HTTP 502, 503, 504, connection reset):
- Compute backoff duration:
  $$\text{delay} = \min(\text{max\_delay}, \text{initial\_delay} \times 2^{\text{attempt}})$$
- **Add Full Jitter**: Randomize delay between $0$ and $\text{delay}$ to prevent synchronized retries from overwhelming downstream servers:
  $$\text{actual\_sleep} = \text{random}(0, \text{delay})$$
- **Never Retry Non-Transient Errors**: Do **not** retry HTTP 400 (Bad Request), 401 (Unauthorized), 403 (Forbidden), 404 (Not Found), or 422 (Unprocessable Entity).

---

## 3. Idempotency & Circuit Breakers

### Idempotency Keys
- For unsafe HTTP methods (`POST`, `PATCH`), supply an `Idempotency-Key: <uuid>` header.
- The server stores the processed key and returns the cached response if a duplicate request arrives, preventing duplicate charges or creation.

### Circuit Breakers
- Track consecutive failures over a rolling window.
- **Closed State**: Requests pass normally.
- **Open State**: If failure threshold (e.g. 50% errors over 10s) is breached, trip breaker immediately, returning fast failure without hitting the struggling downstream service.
- **Half-Open State**: After a cooldown period, allow a single probe request to test if the dependency has recovered.
