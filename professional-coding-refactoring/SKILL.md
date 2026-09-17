---
name: professional-coding-refactoring
description: >-
  Specialist skill for structural improvements, code smell reduction, duplication
  elimination, modular decomposition, and maintainability enhancement without altering
  observable behavior. Activate when decomposing overly coupled logic, eliminating
  duplicated blocks, or executing dedicated cleanup tasks. Explicitly DO NOT activate
  for urgent bug fixes, new feature delivery, or drive-by opportunistic cleanup during
  unrelated work.
---

# Professional Coding — Refactoring Specialist

**Domain**: Structural code improvements, code smell reduction, duplication elimination, modular decomposition, and maintainability enhancement while strictly preserving observable behavior.

This specialist operates under the governance of the **Professional Coding Core** orchestrator (`professional-coding`). It is an authorized improvement discipline, **strictly prohibiting opportunistic drive-by refactoring during unrelated bug fixes or features**.

---

## 1. Scope Boundaries (CAN / CANNOT / ESCALATE)

### CAN
- Extract cohesive helper functions, classes, or modules from bloated structures.
- Eliminate code duplication through abstraction and parameterization.
- Improve variable, function, and type naming to reflect business domain clarity.
- Decompose complex conditionals using guard clauses and early returns.
- Improve code readability, modularity, and cohesion while preserving 100% of observable behavior.
- Require an existing passing test baseline before initiating structural changes.

### CANNOT
- Perform opportunistic or drive-by refactoring during bug fixes or feature work.
- Silently alter public API signatures, return types, or parameter contracts.
- Alter observable runtime behavior, error semantics, or business logic.
- Proceed with structural refactoring without passing test coverage.
- Treat refactoring as authorization to rewrite entire subsystems (escalate to `architecture`).

### ESCALATE
- **STOP and escalate to the user** when proposed refactoring reveals deep architectural flaws requiring structural boundary redesign.
- Escalate when internal cleanup reveals that preserving public interface contracts is technically infeasible.
- Escalate when observable behavior cannot be guaranteed without changing existing tests.

---

## 2. Activation Discipline

### Positive Activation Triggers
Activate when task requirements involve:
- Dedicated, user-requested refactoring tasks (e.g. "decompose this 600-line service").
- Reducing code duplication across closely related functions or files.
- Eliminating recognized code smells (long methods, god classes, feature envy, primitive obsession).
- Simplifying deeply nested conditional blocks or high cyclomatic complexity.
- Modularizing legacy code to prepare for future feature extensibility.
- **Complexity Alignment**: Typically **Standard** or **Complex** tier tasks.

### Negative Activation Exclusions
Explicitly **DO NOT** activate when:
- Fixing an urgent bug or regression (Scope Firewall mandates minimal change; route to `debugging`).
- Implementing an unrelated new feature where adjacent code happens to be messy.
- Discovering technical debt during implementation (log in *Observations / Discovered Technical Debt* instead).
- *Alternative Route*: Handled internally by **Core** (Scope Firewall) or routed to `debugging`.

---

## 3. Core Governance & Authority

1. **Orchestrator Precedence**: This specialist operates within the 5-phase meta-workflow (`Understand → Plan → Implement → Verify → Document`) governed by Core.
2. **Authority Hierarchy**:
   ```
   Priority 1 (Highest): User Intent (Explicit refactoring scope and goals)
   Priority 2:          Repository Contract (Existing project idioms, linter rules, conventions)
   Priority 3:          Professional Coding Core (Scope Firewall, minimal change, safety)
   Priority 4:          Safety & Authorization Constraints (Absolute & Non-bypassable)
   Priority 5:          Refactoring Specialist (Refactoring patterns & decomposition techniques)
   Priority 6 (Lowest):  Stylistic & Implementation Preferences
   ```
3. **Non-Bypassable Safety**: Refactoring operations must never weaken security boundaries, skip transaction boundaries, or break existing automated tests.

---

## 4. The Discipline of Behavior Preservation

```text
Verify Baseline Tests Pass  ───►  Atomic Transformation  ───►  Verify Baseline Tests Pass
```

1. **Test Baseline Prerequisite**: Before modifying code, execute the existing test suite. All tests must pass cleanly. If tests are missing, author tests via `professional-coding-testing` first!
2. **Atomic Steps**: Make small, discrete transformations (e.g. Extract Function, Rename Symbol). Verify tests pass after every transformation.
3. **Zero Semantic Drift**: If an existing test fails, you did not refactor; you broke functionality. Revert and adjust.

---

## 5. Multi-Specialist Composition

Refactoring coordinates with verification and architectural disciplines:

- **Refactoring + Testing (`professional-coding-testing`)**: Testing establishes the test harness before refactoring begins and validates zero regression upon completion.
- **Refactoring + Architecture (`professional-coding-architecture`)**: Architecture defines long-term component boundaries; Refactoring executes localized module decompositions within those boundaries.
- **Refactoring + Code Review (`professional-coding-code-review`)**: Code Review audits the refactoring diff ensuring zero unintended behavior changes or scope creep.

---

## 6. Expected Inputs & Deliverables

| Category | Details |
|---|---|
| **Inputs** | Target code files, existing automated test suite, refactoring objectives, complexity metrics. |
| **Deliverables** | Decomposed modules, extracted cohesive helpers, simplified control flow, zero semantic changes. |
| **Verification** | 100% pass on existing test suite with zero modifications to test assertions; diff audit confirming zero behavioral changes. |

---

## 7. Reference Triggers

Consult detailed references on demand:
- [references/refactoring-patterns.md](references/refactoring-patterns.md): Common Fowler refactoring patterns (Extract Function, Replace Conditional with Guard, Decompose Class).
- [references/behavior-preservation.md](references/behavior-preservation.md): Testing baseline rules, avoiding semantic drift, and atomic transformation workflows.
