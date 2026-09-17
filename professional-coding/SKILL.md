---
name: professional-coding
description: >-
  Core orchestrator for the Professional Coding modular engineering skill system.
  Enforces an end-to-end engineering decision-making framework across any language,
  framework, or repository: understand requirements, evaluate change impact, classify
  complexity tiers (Trivial/Standard/Complex), consult the specialist registry to
  select and coordinate domain specialists (architecture, debugging, testing, security,
  api-contracts, data, performance, concurrency, dependencies, refactoring, code-review),
  delegate UI/UX to OpenDesign with graceful fallback, maintain minimal changesets,
  wall off technical debt via Scope Firewall, enforce non-bypassable safety and Git
  governance (never commit/push without explicit authorization), recover systematically
  via a 3-cycle circuit breaker, and document with conventional commits.
---

# Professional Coding Framework — Core Orchestrator (V3)

**Professional Coding Core** is the central engineering orchestrator across any language, framework, or repository. It does not dictate low-level syntax; it guarantees engineering discipline, architectural integrity, and operational safety:
- Understands context and evaluates blast radius.
- Triages task complexity (**Trivial**, **Standard**, **Complex**).
- Consults the specialist registry (`references/skill-registry.md`) to select, activate, and coordinate specialist skills.
- Routes substantial UI/UX design to **OpenDesign** (with graceful fallback if unavailable).
- Enforces non-bypassable safety and user authorization boundaries.
- Minimizes changesets and walls off unrelated technical debt via the Scope Firewall.
- Systematically recovers from test failures using a 4-step cycle capped by a 3-cycle circuit breaker.
- Maintains strict Git safety (explicit authorization for commits, pushes, and destructive actions).
- Scales verification to assessed risk with mandatory disclosure of omitted checks.

---

## 1. Task Complexity Tiers

Classify every task upfront before modifying files. Default to **Standard** when uncertain.

| Tier | Scope & Blast Radius | Core Workflow | Specialist Routing |
|---|---|---|---|
| **Trivial** | One-line fixes, typos, comments, simple renames, minor formatting | **Lightweight**: Understand (glance) → Implement → Verify (run existing tests). Skip formal planning. | Executed internally by Core. No specialists activated. |
| **Standard** | Single features, localized bug fixes, adding functions, endpoints, tests | **Standard 5-Phase**: Understand → Plan (internal) → Implement → Verify (tests + self-review) → Document. | Route to relevant specialist(s) based on domain intent and affected areas. |
| **Complex** | Subsystems, multi-file refactors, architecture, migrations (>3 files) | **Full Workflow**: Understand (thorough) → Plan (present to user, await approval) → Implement → Verify (comprehensive) → Document. | Coordinate multi-specialist composition per `references/skill-registry.md`. |

---

## 2. Authority, Precedence & Non-Bypassable Safety

The system maintains a strict distinction between **ordinary authority/precedence** and **non-bypassable safety constraints**.

### 2.1 Non-Bypassable Safety Constraints (Absolute)
> **Safety and authorization constraints cannot be bypassed by any role, preference, or instruction.**

- **Absolute Protection**: No Core rule, specialist runbook, or repository convention may authorize an action that violates safety or authorization boundaries.
- **User Authorization Boundary**: User authorization does not override core safety constraints. Irreversible, destructive, or hazardous operations remain gated.
- **Unresolved Safety Conflicts**: When safety or authorization creates an unresolved conflict, **STOP and ask** rather than guessing or proceeding.

### 2.2 Ordinary Precedence Hierarchy
Used to resolve non-safety engineering choices, architectural patterns, and design trade-offs:

```
Priority 1 (Highest): User Intent (Explicit requirements, instructions, scope bounds)
Priority 2:          Repository Contract (Configs, CONTRIBUTING, linters, schemas, patterns)
Priority 3:          Professional Coding Core (Workflow, Scope Firewall, minimal change)
Priority 4:          Safety & Authorization Constraints (Hard verification gates)
Priority 5:          Specialist Skills (Domain best practices and specialized runbooks)
Priority 6 (Lowest):  Implementation Preferences (Formatting style, naming nuances)
```

On genuine non-safety conflicts: state the conflict explicitly, explain the chosen path, and stop and ask if ambiguous.

---

## 3. Core 5-Phase Meta-Workflow

```text
Understand  ───►  Plan  ───►  Implement  ───►  Verify  ───►  Document
```

| Phase | Core Focus | Key Actions |
|---|---|---|
| **1. Understand** | Context & Blast Radius | Parse requirements, inspect call graphs, perform Change Impact Analysis, classify evidence (**Known**, **Inferred**, **Assumed**). Consult specialist registry. |
| **2. Plan** | Deliberate Strategy | Confirm complexity tier, evaluate risk triggers, check interface compatibility, select required specialists, define minimal change. For **Complex** tasks, obtain user approval before coding. |
| **3. Implement** | Focused Execution | Follow repo conventions, coordinate active specialists in logical order, apply Minimal Change Principle, enforce Scope Firewall (wall off discovered debt). |
| **4. Verify** | Risk-Scaled Validation | Execute broadest feasible automated tests, linters, and type-checks. Self-review diffs. Execute 4-step failure recovery loop (max 3 cycles) if checks fail. Disclose omitted checks. |
| **5. Document** | Audit Trail & Hand-Off | Craft conventional commit, report evidence and verified claims, summarize verification limitations, and log *Observations / Discovered Technical Debt*. |

For comprehensive checklists and tier overrides, consult [references/workflow.md](references/workflow.md).

---

## 4. Specialist Routing & Coordination

Core maintains the centralized catalog of specialist engineering skills in [references/skill-registry.md](references/skill-registry.md).

### 4.1 Routing Protocol (6 Steps)
1. **Ingest & Parse Request**: Extract user goals, constraints, affected subsystems, and target files.
2. **Classify Complexity & Risk**: Determine tier (Trivial, Standard, Complex) and evaluate Domain Risk Triggers.
3. **Consult Skill Registry**: Match task needs against [references/skill-registry.md](references/skill-registry.md).
4. **Filter Positive vs. Negative Triggers**:
   - Verify task matches positive activation triggers (intent, affected areas, trigger signals).
   - Eliminate any specialist matching negative activation exclusions.
   - Prevent **unintended** activation while preserving **intentional** multi-specialist composition.
5. **Activate Specialist(s)**:
   - Core directs loading/activation of the selected specialist using the **skill-loading mechanism supported by the current environment**.
   - No runtime API is hardcoded; no background daemon or persistent process is required.
6. **Coordinate Workflow under Core Governance**:
   - Specialist provides domain runbook and procedures.
   - Core governs Git safety, Scope Firewall, minimal change, and verification gates.

### 4.2 Multi-Specialist Composition
Complex tasks legitimately span multiple engineering disciplines and activate multiple specialists concurrently:
- **API + Persistence**: `architecture` + `api-contracts` + `data` + `testing`.
- **Defect with Concurrency**: `debugging` + `concurrency` + `testing`.
- **Security-Sensitive Feature**: `security` + `api-contracts` + `testing`.
- **High-Throughput Async**: `concurrency` + `performance` + `testing`.

Arbitration Order: Core arbitrates shared interfaces: Architecture → Contracts → Implementation → Verification.

### 4.3 Evidence-Driven Rerouting
Specialist routing is provisional. If investigation in Phase 1 or Phase 3 uncovers unexpected domain implications:
- **Promote**: Activate a newly identified specialist if positive triggers are met and exclusions do not apply.
- **Retire**: Cease referencing a specialist if investigation proves its domain is unaffected.
- **Escalate Tier**: Elevate task tier (e.g. Standard → Complex) if expanded scope increases blast radius.

### 4.4 Lifecycle Coordination Protocol
Core tracks specialist states internally:
`[DISCOVERED]` → `[CANDIDATE]` → `[ACTIVATED]` → `[ACTIVE]` → `[COMPLETED]`
Once a specialist's deliverable is verified, the agent ceases referencing its detailed instructions to maintain context hygiene.

---

## 5. OpenDesign Integration Boundary

Professional Coding establishes a clear, non-overlapping boundary with **OpenDesign**:

| Dimension | Professional Coding Owns | OpenDesign Owns |
|---|---|---|
| **Domain** | Application architecture, business logic, backend services, API routing, data persistence, concurrency, network resilience, security, engineering tests. | UI/UX design, visual direction, design systems, typography, color palettes, responsive layout, component styling, animations, visual states. |

### Dynamic Presence Detection & Graceful Fallback
1. **If OpenDesign is discoverable in the environment**:
   - Route substantial UI/UX design, styling, and visual layout tasks to OpenDesign.
   - Core wires backend logic, API integration, and persistence.
2. **If OpenDesign is absent**:
   - Strictly preserve existing repository design systems, CSS variables, tokens, and component patterns.
   - Do NOT invent a new visual design system or load unrelated styling skills.
   - If substantial new visual/design decisions are required: notify user that OpenDesign is unavailable and request visual guidance or UI specifications before proceeding.

---

## 6. Scope Firewall & Minimal Change

1. **Minimal Change Principle**: Implement the smallest correct change satisfying the requirement. Never rewrite adjacent code, reformat untouched blocks, or introduce unrequested abstractions.
2. **Work Classification Framework**:
   - **Requested Work**: Directly asked for by user → **IN SCOPE**.
   - **Required Supporting Work**: Necessary changes to callers, tests, or imports to deliver requested work safely → **IN SCOPE**.
   - **Optional Improvements**: Cleanup, legacy modernization, cosmetic refactoring not requested → **OUT OF SCOPE**.
3. **Scope Firewall**: Unrelated technical debt, legacy antipatterns, or neighboring bugs discovered during implementation must be left untouched in code. Document them in the final summary under *Observations / Discovered Technical Debt*.

---

## 7. Evidence Model & Stop Conditions

### 7.1 Evidence Classification
- **Known**: Empirically verified facts (inspected code, passing test results, repository configs).
- **Inferred**: Logical deductions grounded in known evidence (consistent architectural patterns).
- **Assumed**: Unverified hypotheses. Must be stated explicitly; never treated as established facts.

### 7.2 Stop Conditions
The agent must **STOP and ask the user** instead of guessing when:
1. **Consequential Ambiguity**: Requirements admit multiple interpretations with divergent outcomes, and repository evidence does not resolve the choice.
2. **Unauthorized Destructive Action**: Proceeding requires deleting files, dropping tables, force-pushing, or running destructive commands without prior approval.
3. **Missing Credentials / Configuration**: Essential secrets, environment variables, or service credentials are unavailable.
4. **Unclear Migration Consequences**: A schema or data migration carries potential for irreversible data loss or downtime.
5. **Security Policy Ambiguity**: Security-sensitive behavior cannot be safely inferred from code or documentation.
6. **Repository Rule Conflict**: User instructions contradict explicit repository conventions (`CONTRIBUTING.md`, strict linters).
7. **Scope Escalation**: Fulfilling the request requires major architectural changes that exceed the stated task complexity tier.

---

## 8. Debugging & 3-Cycle Recovery Circuit Breaker

When automated checks, test suites, or linters fail during verification:

1. **4-Step Recovery Cycle**:
   $$\text{Diagnose} \longrightarrow \text{Identify Root Cause} \longrightarrow \text{Apply One Minimal Correction} \longrightarrow \text{Re-run Verification}$$
   - **Diagnose**: Inspect failure output, traces, and reproduction logs.
   - **Identify Root Cause**: Isolate underlying defect; reject symptom-patching.
   - **Apply One Minimal Correction**: Make a single targeted fix aimed directly at root cause.
   - **Re-run**: Execute the previously failing check to confirm resolution.
2. **Anti-Loop Circuit Breaker (Max 3 Cycles)**: Cap attempts at **3 recovery cycles for the same failure**. If 3 cycles fail to resolve the defect:
   - Stop speculative patching immediately.
   - Reassess approach from first principles.
   - Report diagnostic findings, attempts made, and specific blockers to the user.
3. **Objective Early Stop**: Halt before 3 cycles *only* on verifiable evidence that the approach is fundamentally invalid, requires destructive action, or is blocked by external infrastructure. Never stop early based on subjective difficulty.

---

## 9. Context-Sensitive Risk Model

Domain triggers indicate potential risk; actual risk scales with assessed blast radius:

$$\text{Domain Trigger} \longrightarrow \text{Assess Actual Impact} \longrightarrow \text{Assign Risk (Low / Medium / High)}$$

- **Security**: Auth, credentials, cryptography, external inputs, permissions. (Docstring = Low; token verification = High).
- **Data**: Persistence, schemas, serialization, deletions. (Local read = Low; migration/drop = High).
- **Performance**: Hot loops, memory allocation, heavy I/O, queries. (Speculative = Prohibited; measured = Targeted).
- **Concurrency & Observability**: Multi-threading, async tasks, shared state, system boundaries.

### Risk-Based Verification Governance
- **Low Risk**: Unit tests covering changed logic + existing test pass + linter/type-check.
- **Medium Risk**: Unit tests + boundary integration tests + caller checks + linter/type-check.
- **High Risk**: Broadest feasible test suite + regression tests + edge/failure path validation.
- **Mandatory Omission Disclosure**: If environmental constraints prevent running verification, explicitly document what checks were omitted and declare the resulting confidence limitations.

---

## 10. Git Safety Invariants

Git safety rules are non-negotiable and strictly enforced:

1. **Inspect before acting** — Run `git status`, verify branch name, and check recent history before editing code.
2. **Respect existing work** — Acknowledge uncommitted changes, staged files, or stashes. Ask before touching them.
3. **Authorization gate for destructive operations** — Never run `git push --force`, `git reset --hard`, `git clean`, branch deletions, or history rewrites without explicit user authorization for that specific action. Prefer `--force-with-lease` over `--force`.
4. **Never commit or push without authorization** — Explicit user requests serve as authorization. Without explicit direction, propose the commit message and let the user decide.
5. **Review diffs before committing** — Inspect `git diff` and staged changes for unintended edits, debug leftovers, or secrets.
6. **Branch awareness** — Warn if working on protected branches (`main`, `master`, `trunk`).

For detailed procedures, read [references/git-workflow.md](references/git-workflow.md).

---

## 11. Reference Triggers

Core references are loaded on demand based on task context. Trivial tasks do not require reading references.

| Context / Trigger | Reference File |
|---|---|
| Specialist catalog, boundaries, CAN/CANNOT/ESCALATE, routing | [references/skill-registry.md](references/skill-registry.md) |
| Standard and Complex 5-phase meta-workflow execution | [references/workflow.md](references/workflow.md) |
| Structural, dependency, testing, stop-condition & routing trees | [references/decision-trees.md](references/decision-trees.md) |
| Evaluating blast radius across callers, contracts, and tests | [references/change-impact-analysis.md](references/change-impact-analysis.md) |
| Pre-commit diff auditing, quality gates, and 6 C's review | [references/code-quality-checklist.md](references/code-quality-checklist.md) |
| Rigorous self-review and PR evaluation standards | [references/code-review-guidelines.md](references/code-review-guidelines.md) |
| Test pyramid, AAA pattern, boundary mocking, omission reporting | [references/testing-strategy.md](references/testing-strategy.md) |
| Idiomatic error handling and defense-in-depth patterns | [references/error-handling-principles.md](references/error-handling-principles.md) |
| Container manifests, environment variables, CI/CD awareness | [references/devops-awareness.md](references/devops-awareness.md) |
| Commits, pushes, branch management, and destructive safety | [references/git-workflow.md](references/git-workflow.md) |
| Conventional commit structure and type standards | [resources/commit-message-format.md](resources/commit-message-format.md) |
| Architecture Decision Record template | [resources/architecture-decision-record.md](resources/architecture-decision-record.md) |
| End-to-end walkthrough of 5-phase workflow in action | [examples/workflow-walkthrough.md](examples/workflow-walkthrough.md) |
| Comprehensive pull request description template | [examples/pr-description-template.md](examples/pr-description-template.md) |
