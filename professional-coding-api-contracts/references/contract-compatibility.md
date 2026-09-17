# API Contract Compatibility, Versioning & Evolution

Guidelines for maintaining backward compatibility, evaluating breaking changes, and structuring API versions.

---

## 1. Breaking vs. Non-Breaking Taxonomy

| Change Type | Classification | Impact & Rule |
|---|---|---|
| **Add optional field to response** | Non-Breaking | Permissible (tolerant consumers ignore unknown fields). |
| **Add new optional query parameter** | Non-Breaking | Permissible. |
| **Add new endpoint route** | Non-Breaking | Permissible. |
| **Remove field from response** | **BREAKING** | Forbidden without deprecation window or major API version. |
| **Rename existing field** | **BREAKING** | Forbidden without supporting dual-emission during migration. |
| **Change field data type (e.g., int $\rightarrow$ string)** | **BREAKING** | Forbidden without major API version. |
| **Add mandatory field to request** | **BREAKING** | Breaks existing clients omitting that field. |
| **Change HTTP status code semantic** | **BREAKING** | Breaks client branching logic (e.g. 200 $\rightarrow$ 202). |

---

## 2. API Versioning Strategies

1. **URI Path Versioning** (`/api/v1/users`, `/api/v2/users`):
   - Clear, explicit, browser-friendly, cache-friendly.
   - Recommended for major breaking paradigm shifts.
2. **Header Versioning** (`Accept: application/vnd.company.v2+json` or `X-API-Version: 2026-09`):
   - Keeps URIs clean; supports fine-grained date-based versioning.
3. **Protobuf Field Tags**:
   - Backward-compatible by design: never change existing field tag numbers; mark removed fields as `reserved`.

---

## 3. Deprecation & Sunsetting Protocol

When phasing out an interface:
1. **Announce**: Mark the endpoint/field as `@deprecated` in schemas and OpenAPI docs.
2. **Header Signals**: Return the `Deprecation: @<timestamp>` and `Sunset: <date>` HTTP headers (RFC 8594).
3. **Log & Monitor**: Log caller user-agents accessing deprecated fields to identify stragglers before decommissioning.
