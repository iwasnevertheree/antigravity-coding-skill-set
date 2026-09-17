# Architecture Decision Record (ADR) Template

Format for documenting architectural decisions, trade-offs, context-sensitive risk evaluations, and scope boundaries.

---

# ADR-[NUMBER]: [TITLE]

## Status
[Proposed | Accepted | Superseded | Deprecated]

## Context
- **Problem Statement**: What problem is being solved?
- **Drivers**: Business, technical, operational, or architectural forces.
- **Evidence & Assumptions**:
  - **Known**: Facts verified in repository, documentation, or active skills.
  - **Inferred**: Deductions from established codebase patterns.
  - **Assumed**: Unverified defaults or requirements.
- **Context-Sensitive Risk Assessment**:
  - **Trigger Domains Evaluated**: [Security | Data | Performance | Observability]
  - **Assessed Risk Level**: [Low | Medium | High]
  - **Risk Rationale**: Explanation of actual impact rather than domain presence alone.

## Decision
- **Chosen Approach**: Clear description of selected solution.
- **Minimal Change Principle**: How this approach minimizes blast radius.
- **Scope Firewall**: Out-of-scope legacy patterns or technical debt left untouched.

## Consequences
- **Positive Consequences**: Benefits and capabilities gained.
- **Negative Consequences**: Trade-offs, complexities, or constraints introduced.
- **Verification Plan**: Broadest feasible verification and confidence limitations.

## Alternatives Considered
- **Alternative 1**: Description, pros, cons, and rejection reason.
- **Alternative 2**: Description, pros, cons, and rejection reason.
