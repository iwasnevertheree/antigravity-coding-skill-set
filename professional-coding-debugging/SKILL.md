---
name: professional-coding-debugging
description: >-
  Specialist skill for defect diagnosis, reproduction, root-cause isolation,
  minimal corrective changes, regression verification, and failure recovery.
  Enforces the 4-step recovery cycle (Diagnose -> Root Cause -> 1 Minimal Correction ->
  Re-run) and the maximum 3-cycle anti-loop circuit breaker. Activate when fixing
  application crashes, test failures, regressions, logic bugs, unexpected errors, or
  timeouts. Explicitly DO NOT activate for implementing new features from scratch,
  planned structural refactoring, or passing mentions of 'fixing architecture'.
---

# Professional Coding — Debugging & Recovery Specialist

**Domain**: Defect diagnosis, failure reproduction, root-cause isolation, minimal corrective changes, regression verification, and systematic failure recovery.

This specialist operates under the governance of the **Professional Coding Core** orchestrator (`professional-coding`). It is an evidence-driven diagnostic discipline, **strictly prohibiting speculative patching or drive-by refactoring**.

---

## 1. Scope Boundaries (CAN / CANNOT / ESCALATE)

### CAN
- Analyze error logs, stack traces, crash dumps, and reproduction steps.
- Formulate testable hypotheses and isolate root causes from symptoms.
- Insert temporary diagnostic logging or breakpoints to isolate state (must be 100% removed before commit).
- Apply a single, targeted, minimal correction addressing the identified root cause.
- Execute the 4-step recovery loop and enforce the 3-cycle anti-loop circuit breaker.
- Coordinate with `professional-coding-testing` to construct automated regression tests.

### CANNOT
- Perform speculative refactoring of adjacent or untouched code during a bug fix.
- Suppress errors silently (e.g., empty catch blocks, omitting error propagation).
- Continue iterative guessing beyond 3 recovery cycles for the same failure.
- Commit temporary diagnostic logging, print statements, or debug helpers.
- Alter public contracts, schemas, or unrelated features under the guise of debugging.

### ESCALATE
- **STOP and escalate to the user** after 3 unsuccessful recovery cycles for the same failure, presenting diagnostic findings, hypotheses tested, and specific blockers.
- **Halt early** if objective evidence shows the defect is caused by missing external infrastructure, unauthorized services, or invalid project specifications.

---

## 2. Activation Discipline

### Positive Activation Triggers
Activate when task requirements involve:
- Application crashes, panics, unhandled exceptions, or abnormal terminations.
- Failing automated test suites, assertions, or linter/compiler errors.
- Behavioral regressions where previously working functionality is broken.
- Logic errors, off-by-one calculations, or unexpected data transformations.
- Intermittent timeouts, deadlocks, or race conditions (co-activated with `concurrency`).
- **Complexity Alignment**: Typically **Standard** or **Complex** tier tasks.

### Negative Activation Exclusions
Explicitly **DO NOT** activate when:
- Implementing a brand new feature requested from scratch where no bug exists.
- Performing planned structural cleanup or code modularization (route to `refactoring`).
- The user casually remarks "we need to fix our architecture" or "clean up this mess".
- Performing Trivial-tier fixes such as updating a typo in a documentation string.
- *Alternative Route*: Handled internally by **Core** or routed to `refactoring` / `architecture`.

---

## 3. Core Governance & Authority

1. **Orchestrator Precedence**: This specialist operates within the 5-phase meta-workflow (`Understand → Plan → Implement → Verify → Document`) governed by Core.
2. **Authority Hierarchy**:
   ```
   Priority 1 (Highest): User Intent (Explicit requirements and scope bounds)
   Priority 2:          Repository Contract (Existing patterns, CONTRIBUTING.md, linter configs)
   Priority 3:          Professional Coding Core (Scope Firewall, minimal change, safety)
   Priority 4:          Safety & Authorization Constraints (Absolute & Non-bypassable)
   Priority 5:          Debugging Specialist (Diagnostic procedures & recovery loop)
   Priority 6 (Lowest):  Stylistic & Implementation Preferences
   ```
3. **Non-Bypassable Safety**: Diagnostic procedures and bug fixes may never bypass security controls, execute unauthorized destructive operations, or violate Core Git safety rules.

---

## 4. Scientific Root-Cause Methodology

Debugging follows a rigorous 4-phase scientific inquiry:

```text
Reproduce  ───►  Formulate Hypothesis  ───►  Isolate Root Cause  ───►  Verify Fix
```

1. **Reproduce**: Establish a reliable, deterministic reproduction path. Never attempt to fix a defect that cannot be observed or measured.
2. **Formulate Hypothesis**: Based on logs, stack traces, and variable states, propose a specific explanation for *why* the defect occurs.
3. **Isolate Root Cause**: Distinguish between the **symptom** (the crash site) and the **root cause** (the invalid state or flawed assumption upstream).
4. **Targeted Fix**: Apply the smallest viable change that remedies the defect at its source without side effects.

---

## 5. Failure Recovery & Circuit Breaker

When an automated test or verification check fails, this specialist executes the standardized recovery protocol:

### 5.1 The 4-Step Recovery Cycle
$$\text{Diagnose} \longrightarrow \text{Identify Root Cause} \longrightarrow \text{Apply One Minimal Correction} \longrightarrow \text{Re-run Verification}$$

1. **Diagnose**: Inspect failure output, error logs, and stack traces thoroughly.
2. **Identify Root Cause**: Determine why the failure happened; reject surface symptom-patching.
3. **Apply One Minimal Correction**: Make a single targeted edit aimed directly at the verified root cause.
4. **Re-run**: Execute the previously failing verification check to confirm resolution.

### 5.2 The 3-Cycle Anti-Loop Circuit Breaker
- **Strict Limit**: Maximum **3 recovery cycles** for the same failure.
- **When Circuit Breaker Trips (After Cycle 3)**:
  1. Immediately stop speculative patching or code edits.
  2. Reassess the entire technical approach from first principles.
  3. Prepare a diagnostic report for the user detailing:
     - The underlying defect and observed symptoms.
     - The hypotheses tested across the 3 attempts.
     - Root-cause analysis and remaining blockers.
  4. Present the report and await user guidance.
- **Objective Early Stop**: Halt before reaching 3 cycles *only* upon verifiable evidence that the current approach is fundamentally invalid, requires destructive actions, or is blocked by unavailable external systems. Never stop early based on subjective difficulty.

---

## 6. Scope Firewall & Minimal Change

- **No Drive-By Cleanup**: If adjacent poorly-structured code, deprecated methods, or secondary bugs are uncovered during debugging, do **NOT** refactor them.
- **Wall Off Debt**: Record discovered technical debt in the task notes under *Observations / Discovered Technical Debt* for future triage.
- **Diff Hygiene**: Verify that `git diff` contains zero debug statements, zero formatting churn, and only the minimal logic edit necessary to fix the root cause.

---

## 7. Multi-Specialist Composition

Debugging seamlessly coordinates with other disciplines when defects span multiple domains:

- **Debugging + Testing (`professional-coding-testing`)**: Debugging isolates the defect; Testing writes a minimal regression test that fails before the fix and passes afterward.
- **Debugging + Concurrency (`professional-coding-concurrency`)**: Debugging traces thread/async state; Concurrency supplies thread-safe synchronization primitives (mutexes, channels, atomics).
- **Debugging + Security (`professional-coding-security`)**: Debugging locates vulnerability ingress points; Security designs sanitization, permission gates, and secure token validation.

---

## 8. Reference Triggers

Consult detailed references on demand:
- [references/recovery-loop.md](references/recovery-loop.md): The 4-step recovery loop, 3-cycle circuit breaker, and early stop conditions.
- [references/root-cause-analysis.md](references/root-cause-analysis.md): Evidence-based root-cause isolation, temporary diagnostics hygiene, and hypothesis testing.
