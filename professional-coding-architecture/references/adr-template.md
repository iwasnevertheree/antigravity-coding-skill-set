# Architecture Decision Record (ADR) Guidelines & Template

An Architecture Decision Record (ADR) captures a significant structural, infrastructural, or design decision along with its context, options evaluated, rationale, and consequences.

---

## When to Write an ADR
Author an ADR when a decision:
- Introduces a new subsystem, framework, or core dependency.
- Changes module boundaries, packaging structure, or layer contracts.
- Establishes a pattern that affects multiple team members or multiple files (>3 files).
- Involves non-obvious trade-offs between competing architectural qualities (e.g. latency vs. consistency).

---

## ADR Template

```markdown
# ADR-[NUMBER]: [Short Title of Decision]

* **Status**: [Proposed | Accepted | Superseded by ADR-X | Deprecated]
* **Date**: [YYYY-MM-DD]
* **Deciders**: [List authors and stakeholders]
* **Technical Domain**: [Architecture | Data | Security | Transport]

## Context
Describe the current state, the problem being solved, the forces influencing the decision (business goals, technical constraints, operational environment), and why a decision is necessary now.

## Decision Drivers
- [Driver 1: e.g., High-throughput write requirements (>10k ops/sec)]
- [Driver 2: e.g., Must support offline-first operation]
- [Driver 3: e.g., Existing team familiarity and ecosystem maturity]

## Considered Options
1. **Option 1: [Option Title]**
   - Description: Brief explanation of approach.
   - Pros: ...
   - Cons: ...
2. **Option 2: [Option Title]**
   - Description: Brief explanation of approach.
   - Pros: ...
   - Cons: ...

## Decision Outcome
Chosen option: **[Option Name]**, because [primary justification linked to decision drivers].

### Detailed Design & Consequences
- **Positive Impacts**: [Benefits achieved, structural clarity, performance gains]
- **Negative Impacts & Trade-offs**: [Introduced complexity, operational costs, learning curve]
- **Mitigations**: [How risks or downsides are controlled]

## Compliance & Verification
How will this decision be verified in code?
- Architectural boundary checks (e.g. dependency linters).
- Integration test suites verifying decoupled communication.
```
