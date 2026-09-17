# Architectural & Implementation Decision Trees

Decision trees for structural, dependency, testing, stop-condition, and recovery choices.

---

## 1. Create New File vs. Modify Existing File

```text
Cohesive with existing module?
├── YES: Manageable size/complexity?
│   ├── YES ──> Modify existing file (Minimal Change)
│   └── NO  ──> Extract submodules, then add
└── NO: Distinct concept or subsystem?
    ├── YES ──> Create new file in domain directory
    └── NO  ──> Re-evaluate design; find proper layer
```

---

## 2. Dependency Discipline (5-Step Gate)

```text
Need external functionality?
├── 1. Solved by internal repo utility? ──────> Use existing code
├── 2. Solved by declared dependency? ────────> Use declared dependency
├── 3. Solved by platform standard library? ──> Use standard library
├── 4. Eliminates substantial complex logic? (crypto, parser)
│   ├── NO (trivial helper) ──────────────────> Implement internally
│   └── YES
└── 5. Mature, maintained, safely licensed?
    ├── YES ──> Add dependency; update lockfile & manifests
    └── NO  ──> Find reputable alternative or implement internally
```

---

## 3. Which Testing Approach

```text
What is being verified?
├── Algorithmic logic, transforms, pure functions ─> Unit test
├── Boundary integrations (storage, network) ─────> Integration test
├── Core user journeys or workflows ──────────────> E2E / acceptance test
└── Bug fix ──────────────────────────────────────> Regression test
```

---

## 4. How to Structure a Function or Module

```text
Does function perform multiple tasks?
├── YES ──> Extract single-responsibility helpers
└── NO: Deeply nested conditionals (>2 levels)?
    ├── YES ──> Use guard clauses / early returns
    └── NO: Parameter count high (>3-4 args)?
        ├── YES ──> Group into parameter object or struct
        └── NO  ──> Keep function concise and focused
```

---

## 5. Does the Change Require DevOps Changes?

```text
Altered dependencies, environment, or build outputs?
├── Added/changed dependency? ──> Update manifests, lockfile, cache
├── Added/modified env vars? ───> Update .env template, schemas, CI env
├── Changed entry points/ports? > Update container ENTRYPOINT and manifests
├── Added backing service? ─────> Update compose manifests and CI services
└── Pure internal code logic ───> No DevOps adjustments required
```

---

## 6. Stop-Condition Precedence

```text
Facing ambiguity, missing data, or risk?
├── Resolved by repo conventions or active skills?
│   └── YES ──> Follow repo evidence; do not ask unnecessary questions
└── NO:
    ├── Consequential ambiguity? ──────> STOP; ask user
    ├── Unauthorized destructive action? > STOP; get authorization
    ├── Essential credentials missing? ─> STOP; request access
    └── Inadequate confidence for high-risk change?
        ├── YES ──> Document limitation; STOP if consequential
        └── NO  ──> Infer reasonable default; note assumption
```

---

## 7. Failure Recovery & Circuit Breaker

```text
Verification check failed?
├── Cycle <= 3 for the same failure?
│   ├── Clearly fundamentally invalid, unsafe, or blocked externally?
│   │   └── YES ──> Trip early circuit breaker; report blocker
│   └── NO  ──> Recovery cycle: Diagnose -> Root Cause -> 1 Fix -> Re-run
└── Cycle > 3 for the same failure?
    └── YES ──> Circuit breaker trips:
                STOP speculative patching; reassess; report to user
```

---

## 8. Specialist Routing & OpenDesign Boundary

```text
Task parsed and complexity/risk assessed:
├── Trivial task (localized typo, formatting, comment)? ──> Core executes internally (no specialists)
└── Standard / Complex task:
    ├── Substantial UI/UX, visual styling, or animation?
    │   ├── OpenDesign discoverable in environment? ──> Delegate UI to OpenDesign
    │   └── OpenDesign absent? ──> Graceful fallback (preserve repo tokens/components)
    └── Engineering capabilities required?
        ├── Consult references/skill-registry.md
        ├── Match positive triggers & eliminate negative exclusions
        ├── Activate selected specialist(s) via environment-supported loading mechanism
        └── Core governs workflow, Scope Firewall, Git safety, verification
```
