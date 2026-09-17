# Full 5-Phase Workflow

Procedure for the engineering decision-making framework. Read at start of **Standard** and **Complex** tasks. For **Trivial** tasks, SKILL.md rules suffice.

---

## Phase 1: Understand

**Goal**: Know what is changing, understand blast radius, and classify evidence before editing code.

### Checklist
- [ ] Parse requirements and expected behavior
- [ ] Perform Change Impact Analysis per [change-impact-analysis.md](change-impact-analysis.md)
- [ ] Inspect source code for data flow, state, and patterns
- [ ] Map internal and external callers and consumers
- [ ] Classify evidence: **Known** (verified), **Inferred** (patterns), **Assumed** (unverified)
- [ ] Consult [skill-registry.md](skill-registry.md) to identify potential specialist domains
- [ ] Check for UI/UX requirements: if substantial visual/design work, route to OpenDesign (or apply graceful fallback if absent)

### Common Mistakes
- Modifying code without inspecting context or callers
- Treating unverified assumptions as established facts
- Breaking callers through uncoordinated signature changes
- Activating specialists purely on passing keywords without verifying domain impact

### Decision Points
- **Stop Conditions**: Follow repo conventions and specialized skills first. Stop and ask user **only** when:
  1. Consequential ambiguity exists that cannot be resolved by repository evidence.
  2. Proceeding requires unauthorized destructive actions (dropping tables, deleting files, force-pushing).
  3. Essential credentials, secrets, or configurations are missing.
  4. Schema or data migration consequences are unclear or risk data loss.
  5. Security-sensitive behavior cannot be safely inferred.
  6. User request directly conflicts with repository conventions (`CONTRIBUTING.md`, strict linters).
  7. Scope escalation requires architectural changes exceeding the task complexity tier.
- Re-classify task complexity if blast radius exceeds initial tier.

---

## Phase 2: Plan

**Goal**: Choose a deliberate, minimal approach and evaluate contract compatibility before editing files.

### Checklist
- [ ] Confirm complexity tier (Trivial, Standard, Complex)
- [ ] Define minimal change required to satisfy requirements
- [ ] Verify interface compatibility (backward-compatible extensions; caller updates if breaking)
- [ ] Evaluate Risk Triggers (Security, Data, Performance, Concurrency, Observability); assign **Low**, **Medium**, or **High** risk
- [ ] Select required specialist(s) from [skill-registry.md](skill-registry.md), checking positive triggers and negative exclusions
- [ ] Sequence specialist coordination: Architecture → Contracts → Implementation → Verification
- [ ] Define file-level changes and execution order
- [ ] For **Complex** tasks: present plan to user and await approval before editing code

### Common Mistakes
- Over-engineering when a focused change suffices
- Omitting backward compatibility for existing callers
- Marking High Risk based purely on domain keywords without assessing actual impact

### Decision Points
- Present trade-offs to user if unresolvable by repo patterns
- For Standard tasks, keep plans internal unless an assumption carries consequential risk

---

## Phase 3: Implement

**Goal**: Make smallest correct change solving the problem while walling off technical debt.

### Checklist
- [ ] Follow detected code style, naming, and architectural conventions
- [ ] Apply **Minimal Change Principle**: make smallest change satisfying requirements
- [ ] Enforce **Scope Firewall**: make only supporting changes necessary for task
- [ ] Leave unrelated debt, legacy patterns, or neighboring bugs untouched in code
- [ ] Write inline comments only for non-obvious logic (explain *why*, not *what*)
- [ ] Handle error conditions explicitly with project-idiomatic patterns
- [ ] Avoid external dependencies when standard libraries or existing code suffice

### Common Mistakes
- Opportunistic refactoring or reformatting of working code outside requested scope
- Adding external packages for trivial operations built-in capabilities solve
- Silently fixing adjacent defects discovered during implementation

### Decision Points
- Record discovered debt/defects under *Observations / Discovered Technical Debt* in summary notes rather than expanding scope
- Return to Phase 2 if implementation reveals the plan was flawed

---

## Phase 4: Verify

**Goal**: Confirm change works, introduces no regressions, and scales verification to risk.

### Checklist
- [ ] Run broadest relevant verification available and feasible for repository and environment
- [ ] Run configured formatters, linters, and type checkers where applicable
- [ ] Add automated tests for new/changed behavior (happy and error paths for non-trivial logic)
- [ ] For bug fixes: write regression test failing without fix and passing with it
- [ ] If verification checks cannot run: document omitted checks and explain confidence limitations
- [ ] Self-review diff: verify minimal change, scope firewall compliance, zero debug leftovers
- [ ] For **Complex** tasks: perform comprehensive self-review using 6 C's from [code-review-guidelines.md](code-review-guidelines.md)

### Common Mistakes
- Claiming success without executing available verification commands
- Silently ignoring unrunnable tests instead of documenting confidence limitations
- Speculatively patching symptoms during test failures without root-cause diagnosis

### Decision Points
- **Failure Recovery Loop**: When checks fail, execute:
  $$\text{Diagnose} \longrightarrow \text{Identify Root Cause} \longrightarrow \text{Apply One Minimal Correction} \longrightarrow \text{Re-run Verification}$$
- **Circuit Breaker**: Cap iterative corrections at **3 cycles for the same failure**. If 3 cycles fail: stop editing, reassess approach, and report diagnostic findings and blockers to user.
- **Objective Early Stop**: Stop before 3 cycles *only* on clear objective evidence of fundamental invalidity, unsafe operations, or external blockers outside authorized scope.

---

## Phase 5: Document

**Goal**: Record what changed, declare assumptions, report verification, and log observed debt.

### Checklist
- [ ] Craft conventional commit message using [commit-message-format.md](../resources/commit-message-format.md)
- [ ] Update docs, API specs, or comments if behavior or config changed
- [ ] Summarize for user: what changed/why, verification performed and limitations, declared assumptions, and Discovered Technical Debt / Observations.

---

## Complexity Tier Overrides

### Trivial
- **Understand**: Quick glance; verify zero public exports or consumers affected
- **Plan**: Skipped
- **Specialists**: Executed internally by Core; no specialists activated
- **Implement**: Targeted minimal fix
- **Verify**: Run existing tests if present; skip formal self-review
- **Document**: Single-line conventional commit message; concise summary

### Standard
- All 5 phases applied
- **Plan**: Brief internal plan; verify caller compatibility
- **Specialists**: Route to relevant specialist(s) from [skill-registry.md](skill-registry.md) based on intent and affected areas
- **Verify**: Feasible test suite run, new tests added, diff self-reviewed
- **Document**: Conventional commit message with optional body; summary with verified evidence

### Complex
- All 5 phases applied at full depth
- **Plan**: Full Change Impact Analysis; present plan to user and **await approval** before modifying code
- **Specialists**: Multi-specialist composition coordinated by Core; arbitration order: Architecture → Contracts → Implementation → Verification
- **Verify**: Broadest feasible testing, boundary checks, and full 6 C's review
- **Document**: Detailed commit message with body and full change summary with risk analysis

---

## Repository Adaptation

Scan repository to detect conventions before editing:
- **Build & package manifests**: Inspect root files for package definitions.
- **Code style & linters**: Check formatter and linter config files (`.editorconfig`, rules).
- **Test frameworks**: Identify runners, assertion libraries, directory structure, and patterns.
- **CI/CD pipelines**: Inspect workflow manifests and container definitions.
- **Project documentation**: Review `CONTRIBUTING.md`, ADRs, and `README.md`.

Adhere to detected repo conventions; do not impose external tooling.
