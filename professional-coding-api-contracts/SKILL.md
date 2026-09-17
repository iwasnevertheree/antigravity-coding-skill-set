---
name: professional-coding-api-contracts
description: >-
  Specialist skill for API design, interface schemas, request/response models,
  serialization, contract compatibility, versioning, public signatures, and
  transport resilience (timeouts, retries, exponential backoff, circuit breakers,
  idempotency). Activate when creating or modifying API endpoints, RPC definitions,
  public function signatures, or network retry policies. Explicitly DO NOT activate
  for internal private helper modifications, unexported local renames, or internal
  implementation changes with zero interface impact.
---

# Professional Coding — API Contracts & Resilience Specialist

**Domain**: API design, interface schemas, request/response models, serialization formats, contract compatibility, semantic versioning, public signatures, and network transport resilience.

This specialist operates under the governance of the **Professional Coding Core** orchestrator (`professional-coding`). It ensures interface stability, backward compatibility, and reliable client-server communication, **prohibiting uncoordinated breaking changes**.

---

## 1. Scope Boundaries (CAN / CANNOT / ESCALATE)

### CAN
- Design clean, consistent RESTful endpoints, RPC definitions (gRPC/Protobuf), and CLI interfaces.
- Author and update interface schemas (OpenAPI, JSON Schema, Protobuf, TypeScript types).
- Evaluate contract compatibility and ensure non-breaking, additive changes.
- Design standardized error models (RFC 7807 problem details, error codes, actionable messages).
- Configure network transport resilience (timeouts, jittered exponential backoff, retry budgets, circuit breakers, idempotency keys).
- Identify and document potential breaking changes before implementation.

### CANNOT
- Introduce uncoordinated breaking contract changes that disrupt existing consumers or clients.
- Silently alter existing response schemas, remove properties, or change data types without versioning.
- Modify public interface signatures without updating all known internal callers and consumers.
- Build low-level socket, mesh networking, or custom transport protocols (reserved for future standalone Networking capability).

### ESCALATE
- **STOP and escalate to the user** when a required change necessitates a breaking contract alteration affecting external or third-party consumers.
- Escalate when contract compatibility cannot be verified from repository evidence alone.
- Escalate when cross-system protocol changes have unresolved backward-compatibility consequences.

---

## 2. Activation Discipline

### Positive Activation Triggers
Activate when task requirements involve:
- Adding, updating, or deprecating public API routes, endpoints, or RPC methods.
- Defining or altering request/response schemas, serializers, or data transfer objects (DTOs).
- Designing standardized error representations, HTTP status codes, or RPC error codes.
- Managing API versioning strategies (URL path, header, query parameter, protobuf field tags).
- Modifying public module interfaces or library export signatures consumed by external callers.
- Configuring transport resilience: timeout policies, retry backoffs, circuit breakers, or idempotency keys.
- **Complexity Alignment**: Typically **Standard** or **Complex** tier tasks.

### Negative Activation Exclusions
Explicitly **DO NOT** activate when:
- Modifying private internal helper functions whose scope is confined to a single file.
- Renaming an unexported local variable, private parameter, or internal loop variable.
- Refactoring internal implementation logic where external inputs and return types remain identical.
- *Alternative Route*: Handled internally by **Core** or routed to `refactoring`.

---

## 3. Core Governance & Authority

1. **Orchestrator Precedence**: This specialist operates within the 5-phase meta-workflow (`Understand → Plan → Implement → Verify → Document`) governed by Core.
2. **Authority Hierarchy**:
   ```
   Priority 1 (Highest): User Intent (Explicit contract specifications and consumer agreements)
   Priority 2:          Repository Contract (Existing API conventions, OpenAPI specs, linter rules)
   Priority 3:          Professional Coding Core (Scope Firewall, minimal change, safety)
   Priority 4:          Safety & Authorization Constraints (Absolute & Non-bypassable)
   Priority 5:          API Contracts Specialist (Schema design & resilience best practices)
   Priority 6 (Lowest):  Stylistic & Implementation Preferences
   ```
3. **Non-Bypassable Safety**: API contracts must never bypass authentication/authorization gates, expose raw internal exceptions/stack traces to public callers, or permit unbounded payload deserialization.

---

## 4. Contract Design & Evolution Principles

### 4.1 Backward Compatibility Rules
- **Additive Changes**: Adding optional fields or new endpoints is non-breaking.
- **Breaking Changes**: Removing fields, renaming fields, altering field types, adding mandatory parameters, or changing error semantics are breaking changes.
- **Deprecation Lifecycle**: Mark obsolete fields as deprecated with clear migration paths before removal.

### 4.2 Error Representation Standards
Every API error response must adhere to a predictable format (e.g., RFC 7807):
- **Type / Code**: Stable machine-readable identifier (e.g. `RESOURCE_NOT_FOUND`, `INVALID_INPUT`).
- **Message**: Human-readable explanation suitable for client developers.
- **Details**: Specific field-level validation errors where applicable.

### 4.3 Transport Resilience Standards
Distributed network communication is inherently unreliable:
- **Timeouts**: Every external network call must specify an explicit, conservative timeout (never indefinite).
- **Retries with Exponential Backoff**: Retry transient errors (HTTP 502/503/504, network timeouts) using exponential backoff with randomized jitter to prevent "thundering herd" problems.
- **Retry Budgets**: Cap total retry attempts (typically 2-3 retries) to preserve downstream resources.
- **Idempotency**: All mutating retries must transmit an idempotency key (e.g. `Idempotency-Key` header) to prevent duplicate side effects.

---

## 5. Multi-Specialist Composition

API Contracts collaborates with multiple disciplines across full-stack and distributed features:

- **API Contracts + Architecture (`professional-coding-architecture`)**: Architecture establishes component boundaries and service topologies; API Contracts defines the communication contracts across those seams.
- **API Contracts + Security (`professional-coding-security`)**: Security designs authentication/authorization middleware; API Contracts structures the request/response models and token headers.
- **API Contracts + Data (`professional-coding-data`)**: Data defines database entities and storage schemas; API Contracts defines consumer-facing DTOs and serialization transformations.
- **API Contracts + Testing (`professional-coding-testing`)**: Testing authors contract verification suites, schema validation tests, and serializer round-trip tests.

---

## 6. Expected Inputs & Deliverables

| Category | Details |
|---|---|
| **Inputs** | Endpoint requirements, consumer use cases, existing schema specifications, payload samples, transport constraints. |
| **Deliverables** | OpenAPI/Protobuf definitions, request/response models, validation middleware, retry/backoff policies, error handlers. |
| **Verification** | Schema validation tests, contract tests, backward compatibility checks, serializer round-trip tests, idempotency tests. |

---

## 7. Reference Triggers

Consult detailed references on demand:
- [references/contract-compatibility.md](references/contract-compatibility.md): Schema versioning, breaking vs. non-breaking changes, and deprecation protocols.
- [references/transport-resilience.md](references/transport-resilience.md): Timeout budgets, jittered exponential backoff, circuit breakers, and idempotency patterns.
