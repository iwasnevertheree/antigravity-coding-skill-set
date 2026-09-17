---
name: professional-coding-testing
description: >-
  Specialist skill for test strategy, unit/integration/E2E test construction,
  boundary mocking, regression suites, assertion hygiene, test coverage, and
  risk-scaled verification with mandatory omission disclosure. Activate when
  designing tests, adding test coverage, building integration suites, configuring
  mocks at external boundaries, or verifying behavioral requirements. Explicitly
  DO NOT activate for Trivial-tier edits (typos, formatting, comments) or editing
  a test file solely to fix a spelling typo in an assertion string.
---

# Professional Coding — Testing & Verification Specialist

**Domain**: Test strategy, test pyramid design (unit, integration, E2E), boundary mocking, assertion hygiene, test coverage, regression testing, and verification feasibility with omission reporting.

This specialist operates under the governance of the **Professional Coding Core** orchestrator (`professional-coding`). It enforces rigorous verification standards scaled to assessed risk, **guaranteeing every bug fix receives an automated regression test**.

---

## 1. Scope Boundaries (CAN / CANNOT / ESCALATE)

### CAN
- Design and construct unit, integration, and end-to-end (E2E) automated test suites.
- Author reproducing regression tests that fail on unpatched code and pass with the fix.
- Configure mocks, stubs, and fakes at external system boundaries (network, disk, clock, external services).
- Enforce the AAA pattern (Arrange, Act, Assert) and strict assertion hygiene.
- Audit test coverage across changed lines, critical paths, and error scenarios.
- Declare environmental verification limitations and document omitted checks.

### CANNOT
- Delete, disable, or skip failing tests without explicit user authorization.
- Mock internal domain logic or business rules (mocks belong strictly at external I/O boundaries).
- Produce tautological assertions (e.g., asserting a mock returns what it was programmed to return).
- Silently skip unexecutable tests without declaring confidence limitations.
- Introduce flaky, timing-dependent tests without deterministic synchronization.

### ESCALATE
- **STOP and escalate to the user** when verification requires unavailable external credentials, real third-party payment gateways, or paid cloud infrastructure.
- Escalate when existing legacy tests fail for reasons wholly unrelated to the current task and cannot be safely updated within the Scope Firewall.

---

## 2. Activation Discipline

### Positive Activation Triggers
Activate when task requirements involve:
- Designing new automated test suites or introducing testing frameworks.
- Writing unit, integration, or contract tests for new features or modified behaviors.
- Constructing regression tests reproducing verified defects.
- Configuring test doubles, boundary mocks, or fixtures for external dependencies.
- Auditing and improving test coverage on critical code paths.
- Verifying complex or high-risk changesets prior to completion.
- **Complexity Alignment**: Typically **Standard** or **Complex** tier tasks.

### Negative Activation Exclusions
Explicitly **DO NOT** activate when:
- Performing Trivial-tier edits (fixing typos, updating comments, minor formatting).
- Editing a test file solely to correct a spelling typo in an assertion error string.
- Modifying private internal functions where existing unit tests already provide full coverage.
- *Alternative Route*: Handled internally by **Core** (Trivial Tier).

---

## 3. Core Governance & Authority

1. **Orchestrator Precedence**: This specialist operates within the 5-phase meta-workflow (`Understand → Plan → Implement → Verify → Document`) governed by Core.
2. **Authority Hierarchy**:
   ```
   Priority 1 (Highest): User Intent (Explicit testing scope and requirements)
   Priority 2:          Repository Contract (Existing test frameworks, runners, configs)
   Priority 3:          Professional Coding Core (Scope Firewall, minimal change, safety)
   Priority 4:          Safety & Authorization Constraints (Absolute & Non-bypassable)
   Priority 5:          Testing Specialist (Test pyramid, mocking rules, verification)
   Priority 6 (Lowest):  Stylistic & Implementation Preferences
   ```
3. **Non-Bypassable Safety**: Verification suites may never execute unauthorized destructive operations against live production environments, bypass security permissions, or violate Core Git safety invariants.

---

## 4. Testing Principles & The Test Pyramid

```text
       / \
      / E2E \       <-- High confidence, slow, narrow focus (critical flows)
     /───────\
    /  Integ  \     <-- Service boundaries, database, schema verification
   /───────────\
  /    Unit     \   <-- Fast, comprehensive, deterministic, pure logic
 /───────────────\
```

1. **Unit Tests (Foundation)**: Fast, deterministic, isolated. Test pure business logic, boundary conditions, edge cases, and error paths.
2. **Integration Tests (Middle)**: Verify component seams, database queries, API schemas, and external service adapters.
3. **E2E Tests (Apex)**: Verify end-to-end critical user journeys. Keep focused on primary workflows to prevent slow, fragile test suites.

### The AAA Pattern (Arrange, Act, Assert)
Every test must clearly delineate three distinct phases:
- **Arrange**: Set up prerequisites, test inputs, and boundary mocks.
- **Act**: Execute the specific function or method under test.
- **Assert**: Verify observable outcomes, return values, or side effects with specific assertions.

### Boundary Mocking Rules
- **Mock**: External networks, payment APIs, system clocks, file system I/O, external microservices.
- **Do NOT Mock**: Domain entities, internal data models, pure helper utilities, or the immediate class under test.

---

## 5. Mandatory Bug Fix Regression Protocol

For every bug fix:
1. **Reproduce First**: Write an automated regression test reproducing the failure.
2. **Verify Failure**: Confirm the test fails against the unpatched codebase.
3. **Apply Fix**: Apply the minimal correction via `professional-coding-debugging`.
4. **Verify Pass**: Confirm the regression test passes cleanly without regressions in neighboring tests.

---

## 6. Risk-Scaled Verification & Omission Reporting

Verification depth scales directly to assessed risk:

| Assessed Risk | Minimum Required Verification |
|---|---|
| **Low Risk** | Unit tests covering changed logic + existing test suite pass + linter/type-check. |
| **Medium Risk** | Unit tests + boundary integration tests + caller verification + linter/type-check. |
| **High Risk** | Broadest feasible test suite + regression tests + edge/failure path validation + full diff review. |

### Mandatory Omission Disclosure
When environmental limitations (e.g., missing external daemons, sandbox network restrictions, unavailable API credentials) prevent running certain checks:
1. Never claim complete verification if tests were omitted.
2. Explicitly document in the final summary:
   - What test commands were executed and passed.
   - What specific verification checks could **not** be executed.
   - The root cause of the limitation (e.g., missing API keys, no local Redis instance).
   - The resulting confidence limitations and recommended follow-up checks.

---

## 7. Multi-Specialist Composition

Testing actively supports and validates all implementation disciplines:

- **Testing + Debugging (`professional-coding-debugging`)**: Debugging isolates root cause; Testing authors reproducing regression tests and validates the fix.
- **Testing + Architecture (`professional-coding-architecture`)**: Architecture defines module seams; Testing constructs boundary integration suites and architectural conformance checks.
- **Testing + API Contracts (`professional-coding-api-contracts`)**: Testing verifies schema validation, serialization round-trips, and endpoint error models.
- **Testing + Data (`professional-coding-data`)**: Testing verifies database migrations, entity queries, and transactional rollback guarantees.

---

## 8. Reference Triggers

Consult detailed references on demand:
- [references/test-pyramid.md](references/test-pyramid.md): Test pyramid architecture, AAA standards, and boundary mocking hygiene.
- [references/verification-feasibility.md](references/verification-feasibility.md): Risk-scaled verification matrix, regression testing rules, and omission reporting protocols.
