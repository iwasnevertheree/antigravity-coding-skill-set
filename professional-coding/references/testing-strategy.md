# Testing Strategy Guidance

Principles and decision frameworks for automated testing, risk-based verification, and failure recovery across any codebase.

---

## 1. Testing Tiers & Decision Framework

| Tier | Scope | Speed | Best For |
|---|---|---|---|
| **Unit Tests** | Isolated functions, classes, modules | Milliseconds | Business logic, calculations, transforms, boundary conditions |
| **Integration Tests** | Collaborating components, persistence | Seconds | Component boundaries, database queries, middleware, contracts |
| **End-to-End (E2E)** | Complete system workflow | Seconds/minutes | Critical user journeys, smoke testing deployments |

### Decision Rules
- Prefer lowest tier providing high confidence: unit tests for logic, integration tests for boundaries, E2E for critical paths.

---

## 2. Risk-Based Verification Matrix

Scale verification depth to assessed risk (not merely domain presence):

| Assessed Risk | Typical Scope | Minimum Verification Scope |
|---|---|---|
| **Low Risk** | Local helper, typo/rename, comment | Unit tests covering changed logic; existing test suite pass; linter/type check. |
| **Medium Risk** | Feature edit, API update, refactor | Unit tests + integration tests + direct caller checks; linter and type check. |
| **High Risk** | Schema migration, breaking contract, auth | Broadest relevant verification feasible; regression tests; boundary checks; diff review. |

---

## 3. Verification Feasibility & Confidence Limitations

Execute broadest relevant verification available and feasible in current environment:
- If tests cannot run due to missing daemons, credentials, network isolation, or pre-existing unrelated failures:
  1. **Do not silently ignore omission**.
  2. **Explicitly document** what checks could not run.
  3. **Explain resulting confidence limitations** (e.g., unit tests pass, but live database integration requires staging verification).
  4. **Apply a Stop Condition** only if remaining unverified risk is consequential.

---

## 4. Failure Recovery Loop & Circuit Breaker

### The 4-Step Recovery Cycle
$$\text{Diagnose} \longrightarrow \text{Identify Root Cause} \longrightarrow \text{Apply One Minimal Correction} \longrightarrow \text{Re-run Verification}$$
1. **Diagnose**: Inspect error logs, compiler messages, stack traces, and failure outputs.
2. **Identify Root Cause**: Determine underlying defect rather than patching symptoms.
3. **Apply One Minimal Correction**: Make a single targeted edit aimed directly at root cause.
4. **Re-run**: Execute previously failing check to confirm resolution.

### Anti-Loop Circuit Breaker
- **Maximum 3 cycles for the same failure**.
- If 3 cycles fail to resolve defect:
  - **STOP speculative patching immediately**.
  - Reassess approach from first principles.
  - Report diagnostic findings, attempts made, and specific blockers to user.
- **Objective Early Stop**: Stop before 3 cycles *only* when clear, verifiable evidence proves approach is fundamentally invalid, requires unsafe/destructive operation, or is blocked by external prerequisites outside authorized scope. Never stop early based on subjective difficulty.

---

## 5. Test-First, Mocking & Regression Design

- **Test-First**: Write tests first when fixing bugs (reproduce failure first) or implementing contract-heavy logic.
- **Mocking**: Mock at system boundaries (network, external APIs, clock, disk); do not mock internal domain logic. Assert on observable outcomes rather than private call sequences.
- **AAA Pattern**: Arrange inputs → Act on target → Assert observable outcomes. Describe behavior in test names (e.g., `returns_zero_when_discount_is_100_percent`).
- **Regression Tests**: Reproduce failure against unpatched code; confirm fix passes; keep permanently in test suite.
- **Adaptation**: Follow repository test runners, assertion libraries, directory structure, and fixtures.
