# Failure Recovery Protocol & Circuit Breaker

Detailed procedure for the 4-step recovery loop, 3-cycle anti-loop circuit breaker, and objective early stop rules.

---

## 1. The 4-Step Recovery Cycle

When an automated test, compiler check, or linter fails, execute this cycle systematically:

```text
┌────────────────────────────────────────────────────────┐
│ 1. DIAGNOSE                                            │
│ - Read failure logs, exception names, and stack traces │
│ - Locate exact line and file where failure triggered   │
│ - Inspect inputs and actual vs. expected outputs       │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│ 2. IDENTIFY ROOT CAUSE                                 │
│ - Distinguish failure symptom from actual defect origin│
│ - Identify the broken assumption or invalid state      │
│ - Reject superficial symptom patches                   │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│ 3. APPLY ONE MINIMAL CORRECTION                        │
│ - Make a single, targeted edit directly at root cause  │
│ - Do not touch unrelated functions or clean up style   │
│ - Maintain Scope Firewall boundaries                   │
└───────────────────────────┬────────────────────────────┘
                            │
                            ▼
┌────────────────────────────────────────────────────────┐
│ 4. RE-RUN VERIFICATION                                 │
│ - Run the exact check that previously failed           │
│ - Verify fix resolves issue without causing regressions│
└────────────────────────────────────────────────────────┘
```

---

## 2. The 3-Cycle Circuit Breaker

Iterative trial-and-error without root-cause understanding leads to fragile code and hallucinated fixes. The 3-cycle circuit breaker halts this anti-pattern:

- **Cycle Counter**: Tracks attempts for the **same root failure**.
  - Attempt 1: First targeted fix based on initial diagnosis.
  - Attempt 2: Second targeted fix based on refined hypothesis if Attempt 1 fails.
  - Attempt 3: Third targeted fix based on deep inspection if Attempt 2 fails.
- **Circuit Breaker Action (Triggered on 3rd consecutive failure)**:
  1. **HALT**: Stop making edits immediately.
  2. **REASSESS**: Step back and evaluate the broader context from first principles. Was the initial diagnosis flawed? Is the requirement incompatible with the architecture?
  3. **REPORT**: Present a structured diagnostic report to the user:
     - Exact error message, test name, and stack trace.
     - Root-cause hypotheses tested in cycles 1, 2, and 3.
     - Why each attempt failed to resolve the failure.
     - Concrete recommendations or questions requiring user decision.

---

## 3. Objective Early Stop Criteria

Never stop early based on subjective difficulty or frustration. Stop before 3 cycles **only** when objective evidence establishes:
1. **Fundamental Invalidity**: The requested behavior violates mathematical laws, language semantics, or explicit platform invariants.
2. **Missing External Service**: The failure is caused by an unreachable external API, database, or cloud service requiring credentials the agent does not possess.
3. **Unauthorized Destructive Action**: Fixing the defect requires deleting persistent user data, dropping schemas, or force-pushing branches without authorization.
4. **Specification Conflict**: The test asserts behavior that directly contradicts explicit user requirements or contractual specifications.
