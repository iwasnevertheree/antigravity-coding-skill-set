# Code Review & Self-Review Guidelines

Comprehensive checklist for conducting rigorous self-reviews and code reviews across any language or architecture.

---

## The 6 C's of Self-Review

Before presenting changes or submitting for review, systematically evaluate against the 6 C's:

### 1. Correctness
- Does the code solve the stated problem and handle edge cases?
- Are off-by-one errors, null/nil states, and empty collection states accounted for?
- Are error conditions handled gracefully without swallowing failures?
- Does the change preserve existing behavior for unaffected paths?

### 2. Clarity
- Can another engineer understand the code without explanation?
- Are identifiers descriptive, distinct, and intention-revealing?
- Is control flow straightforward without deeply nested conditionals?
- Are comments used only for non-obvious logic (explaining *why*, not *what*)?

### 3. Consistency
- Does the change adhere to existing codebase conventions, idioms, and style?
- Are patterns consistent with neighboring modules?
- Does it match established architectural boundaries and layer responsibilities?

### 4. Completeness
- Are all affected caller sites and consumers updated?
- Are automated tests included for both happy-path and error cases?
- Is documentation updated if behavior, signatures, or configuration changed?
- Are configuration schemas or environment definitions updated if required?

### 5. Conformance
- Does the change adhere to project linter, formatter, and type-checker rules?
- Does it comply with team and repository security policies?
- Are commit messages structured according to project conventions?

### 6. Constraint (Minimal Change Principle & Scope Firewall)
- Is this the smallest change that fully satisfies the requirement?
- Are unrelated formatting changes, refactorings, and drive-by fixes excluded?
- Are out-of-scope defects recorded in observations rather than modified?
- Are debug statements, temporary logs, and commented-out blocks removed?

---

## Scope Firewall & Technical Debt Policy

- **Wall off technical debt**: Do not refactor adjacent legacy patterns or fix unrelated bugs in the same changeset.
- **Record in summary**: Document discovered issues under *Observations / Discovered Technical Debt* in the task summary so they can be prioritized separately.
