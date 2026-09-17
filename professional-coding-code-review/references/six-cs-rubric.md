# The 6 C's Code Review Rubric

Detailed inquiry guide for evaluating code changes across six fundamental engineering qualities.

---

## 1. Correctness
- Does the implementation achieve the specified requirements under all conditions?
- Are off-by-one errors, null/empty states, and boundary conditions handled?
- Are error conditions checked and propagated idioms followed?
- Is concurrency handled correctly with appropriate synchronization?

---

## 2. Clarity
- Can a peer engineer understand the intent within 30 seconds of reading?
- Do names of functions, variables, and classes clearly state *what* they represent?
- Are non-obvious algorithms or business rules accompanied by "why" comments?
- Is the code free of obscure cleverness that impairs long-term maintainability?

---

## 3. Consistency
- Does the diff adopt the conventions of surrounding code?
- Are naming conventions (camelCase, snake_case, PascalCase) consistent with repository idioms?
- Are architectural patterns (e.g. repository pattern, dependency injection) applied uniformly?

---

## 4. Completeness
- Are there automated tests verifying the new or modified logic?
- Do tests cover both happy paths and realistic failure scenarios?
- Are documentation, API schemas, and environment configs updated to match?

---

## 5. Conformance
- Does the code pass all configured linters, formatters, and static type checkers?
- Does it comply with the repository's `CONTRIBUTING.md` and architecture rules?
- Are security standards met (no hardcoded secrets, parameterized queries)?

---

## 6. Constraint (Minimal Change & Scope Firewall)
- Is the diff as small as possible while remaining correct and robust?
- Does it strictly avoid opportunistic refactoring of untouched neighboring files?
- Is unrelated technical debt documented in observations rather than modified in code?
