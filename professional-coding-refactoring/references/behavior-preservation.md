# Preserving Observable Behavior During Refactoring

Guidelines for guaranteeing zero semantic drift, maintaining regression safety, and preventing accidental feature changes.

---

## 1. What Counts as "Observable Behavior"?

Refactoring changes **how** code is written, never **what** code does. Observable behavior includes:
- Return values for all valid and boundary inputs.
- Exception types, error messages, and HTTP status codes emitted on failure.
- Side effects (database writes, files created, events emitted).
- Public class, method, and function signatures.

---

## 2. The Refactoring Loop

```text
1. Run Test Suite ─────────► All Tests MUST Pass
       │
       ▼
2. Apply Single Atomic Refactoring (e.g. Extract Helper)
       │
       ▼
3. Run Test Suite ─────────► All Tests MUST Pass
       │
       ▼
4. Repeat for Next Transformation
```

### Golden Rules
- If a test fails after an atomic edit, **undo the edit immediately**.
- Never modify existing test assertions to make a refactored implementation pass. (If tests must change, the external contract changed, which is a breaking redesign, not a refactoring).
- Never mix refactoring with bug fixing in the same commit. Fix bugs first with regression tests, then refactor in a separate atomic step.
