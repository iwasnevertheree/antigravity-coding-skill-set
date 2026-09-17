# Professional Coding V3 — Specialist Skill Registry

This registry is an **internal governance and routing document** for the Professional Coding Core orchestrator. It defines the catalog of specialist engineering skills, their exact operational boundaries, positive activation triggers, negative activation exclusions, and coordination rules.

> [!NOTE]
> **Internal Document**: This file is part of the `professional-coding` Core orchestrator and is NOT an independently discoverable skill.
> All independently discoverable specialist skills reside as direct sibling directories in `~/.gemini/config/skills/`.

---

## Routing Principles

1. **Deterministic Selection**: Specialists are selected based on:
   - **Task Intent**: What the user or requirement explicitly aims to achieve.
   - **Affected System Area**: Which subsystem, files, or layers are modified or inspected.
   - **Actual Impact**: The evaluated blast radius and risk of the change.
   - **Supporting Evidence**: Verified empirical facts (**Known**) rather than ungrounded assumptions.
   - **Negative Activation Exclusions**: Explicit conditions under which a specialist must NOT activate.
2. **Anti-Overlap & Multi-Specialist Composition**:
   - **No Pure Keyword Matching**: A specialist never activates merely because a keyword appears in passing or the repository happens to contain related technology.
   - **Prevent Unintended Activation**: Eliminate accidental activation on single-domain or trivial tasks.
   - **Preserve Legitimate Composition**: Complex tasks spanning multiple domains legitimately and intentionally activate multiple specialists (e.g., `architecture` + `api-contracts` + `data` + `testing`, or `debugging` + `concurrency` + `testing`).
3. **Environment-Agnostic Activation**:
   - Core loads/activates the required specialist using the skill-loading mechanism supported by the current environment.
   - No daemon, background service, or platform-specific runtime API is hardcoded.

---

## Specialist Catalog (Initial 11 Baseline Specialists)

### 1. architecture
* **Directory**: `../professional-coding-architecture/`
* **Status**: Installed & Active (Batch 1)
* **Purpose**: Define and structure multi-component systems, service boundaries, layered separation, dependency direction, and author Architecture Decision Records (ADRs).
* **Positive Activation Triggers**: Defining or restructuring components; system boundaries; directory reorganization; inter-module interfaces; new subsystems; module splits; component boundary changes; ADR authoring required. Complexity alignment: **Complex**.
* **Negative Activation Exclusions**: Editing a single file; adding a localized function; fixing a bug within an existing class; incidental mentions of "architecture" in passing. (Core handles internally).
* **CAN**: Propose component boundaries, evaluate structural trade-offs, draft ADRs, define module interfaces.
* **CANNOT**: Commit code, approve destructive changes, override repository layer conventions, rewrite unrelated modules.
* **ESCALATE**: Escalate to user when architectural alternatives have divergent product, cost, or operational trade-offs.
* **Dependencies**: None. (Co-activates with `api-contracts`, `data`, and `testing` on multi-component features).
* **Typical Inputs**: Requirements, system overview, module dependency graph, directory layout, constraints.
* **Expected Outputs**: Architecture Decision Records (ADRs), module interface definitions, component boundary specifications, structural migration plans.
* **Typical Verification**: Structural dependency checks, layer isolation verification, architecture conformance reviews.

---

### 2. debugging
* **Directory**: `../professional-coding-debugging/`
* **Status**: Installed & Active (Batch 1)
* **Purpose**: Defect diagnosis, failure reproduction, root-cause isolation, minimal corrective changes, regression verification, and failure recovery with a 3-cycle anti-loop circuit breaker.
* **Positive Activation Triggers**: Fixing broken behavior or test failures; failing logic; exception paths; failing test suites; bugs, crashes, regressions, unexpected errors, timeouts. Complexity alignment: **Standard / Complex**.
* **Negative Activation Exclusions**: Implementing newly requested features from scratch; planned structural refactoring; user casually stating "we need to fix our architecture". (Core or `refactoring` handles).
* **CAN**: Analyze stack traces, insert temporary diagnostics (must be removed before commit), apply minimal fixes to root causes, execute the 4-step recovery cycle (Diagnose → Root Cause → 1 Minimal Correction → Re-run), enforce the 3-cycle circuit breaker.
* **CANNOT**: Perform speculative refactoring of adjacent code, suppress errors silently, exceed 3 recovery cycles.
* **ESCALATE**: Escalate to user after 3 unsuccessful recovery cycles, or upon discovering verifiable external system/infrastructure blockers.
* **Dependencies**: `testing` (for reproducing regression tests).
* **Typical Inputs**: Error logs, stack traces, reproduction steps, failing test outputs, recent diffs.
* **Expected Outputs**: Minimal root-cause bug fix, diagnostic cleanup, regression test reproducing and verifying the fix.
* **Typical Verification**: Regression test passes against fixed code and fails against unpatched code; zero adjacent regressions.

---

### 3. testing
* **Directory**: `../professional-coding-testing/`
* **Status**: Installed & Active (Batch 1)
* **Purpose**: Test strategy design, test pyramid construction (unit, integration, E2E), boundary mocking, assertion hygiene, test coverage, and risk-scaled verification with omission reporting.
* **Positive Activation Triggers**: Designing tests; establishing test suites; increasing test coverage; configuring mock boundaries; validating behavioral requirements; verifying complex changes. Complexity alignment: **Standard / Complex**.
* **Negative Activation Exclusions**: Performing Trivial-tier edits (typos, formatting, documentation, comments); editing a test file solely to fix a spelling typo in an assertion string. (Core handles internally).
* **CAN**: Author unit/integration/regression test suites, configure test mocks at external system boundaries (network, disk, time), audit coverage, declare omitted checks with confidence limitations.
* **CANNOT**: Delete failing tests without explicit user authorization, mock internal domain logic excessively, bypass verification feasibility rules.
* **ESCALATE**: Escalate to user when verification requires unavailable external credentials, real payment gateways, or paid cloud infrastructure.
* **Dependencies**: None. (Supports all implementation specialists).
* **Typical Inputs**: Source code, requirements, interface contracts, existing test runners/configurations.
* **Expected Outputs**: Automated test files (unit, integration, regression) following AAA pattern; verification logs; test omission disclosures.
* **Typical Verification**: Automated test suite execution, coverage reports, regression pass validation.

---

### 4. security
* **Directory**: `../professional-coding-security/`
* **Status**: Installed & Active (Batch 2)
* **Purpose**: Trust boundary definition, authentication, authorization, input validation/sanitization, cryptography, secrets management, and least-privilege operations.
* **Positive Activation Triggers**: Protecting data, auth, or trust boundaries; authentication modules; token verification; permissions/RBAC; secrets handling; input parsers/sanitization; cryptographic operations. Complexity alignment: **Standard / Complex**.
* **Negative Activation Exclusions**: Benign UI layout changes; internal non-sensitive string utilities; local documentation updates; projects that happen to be web applications without security changes. (Core handles internally).
* **CAN**: Enforce input sanitization, design authentication/authorization guards, audit secrets and credentials, flag security anti-patterns.
* **CANNOT**: Weaken existing encryption or auth policies, store secrets in source code, override repository security configurations without approval.
* **ESCALATE**: Escalate to user immediately when security policies conflict with requested feature functionality.
* **Dependencies**: `testing` (for security test vectors and negative testing).
* **Typical Inputs**: Threat surface, auth schemas, credential requirements, data ingress points.
* **Expected Outputs**: Secure input validators, auth middleware/guards, secret management patterns, security vulnerability fixes.
* **Typical Verification**: Negative test cases, auth bypass tests, injection payload validation, static security analysis (SAST).

---

### 5. api-contracts
* **Directory**: `../professional-coding-api-contracts/`
* **Status**: Installed & Active (Batch 2)
* **Purpose**: API design, interface schemas, contract compatibility, serialization formats, request/response models, versioning, and network transport resilience (timeouts, retries, backoff, circuit breakers, idempotency).
* **Positive Activation Triggers**: Creating or changing external/internal contracts; endpoints; RPC schemas; models; public signatures; API routes; contract versioning; breaking signature changes; transport resilience setup. Complexity alignment: **Standard / Complex**.
* **Negative Activation Exclusions**: Modifying private/internal helper functions not consumed outside the immediate file; renaming an unexported local variable. (Core handles internally).
* **CAN**: Define schemas, enforce backward-compatible extensions, design error models, configure transport retry/backoff policies and idempotency keys.
* **CANNOT**: Introduce uncoordinated breaking contract changes, alter existing response schemas without caller updates.
* **ESCALATE**: Escalate to user when a breaking interface change is required that affects external consumers or third-party clients.
* **Dependencies**: `architecture` (when boundaries change); `testing` (for contract testing).
* **Typical Inputs**: Endpoint specifications, schema definitions, consumer requirements, payload samples.
* **Expected Outputs**: Schema definitions (OpenAPI, JSON Schema, Protobuf, type definitions), route handlers, serializer logic, retry/backoff policies.
* **Typical Verification**: Schema validation tests, contract tests, backward compatibility checks, serializer round-trip tests.

---

### 6. data
* **Directory**: `../professional-coding-data/`
* **Status**: Installed & Active (Batch 2)
* **Purpose**: Data modeling, persistence schemas, storage engines, transactional boundaries, query optimization, indexing, and non-destructive schema migrations with rollback safety.
* **Positive Activation Triggers**: Storing, querying, or migrating data; database entities; table schemas; schema migrations; ORM mappings; indexing; SQL queries; transactional consistency. Complexity alignment: **Standard / Complex**.
* **Negative Activation Exclusions**: Managing purely ephemeral in-memory state (local variables, temporary UI state); storing temporary strings in a local array. (Core handles internally).
* **CAN**: Design relational/document schemas, write non-destructive migrations, optimize queries, define transaction boundaries and indexing strategies.
* **CANNOT**: Execute irreversible data drops (`DROP TABLE`, `TRUNCATE`) without explicit authorization, perform schema migrations without rollback plans.
* **ESCALATE**: Escalate to user before executing any schema change that permanently alters or migrates existing production data.
* **Dependencies**: `api-contracts` (when contracts and models change); `testing` (for data persistence tests).
* **Typical Inputs**: Domain entities, database dialect, migration files, query profiles, storage engine requirements.
* **Expected Outputs**: Migration scripts (up/down), entity schemas, query definitions, index definitions.
* **Typical Verification**: Migration up and rollback tests, transactional integrity tests, query explain plan verification.

---

### 7. performance
* **Directory**: `../professional-coding-performance/`
* **Status**: Installed & Active (Batch 3)
* **Purpose**: Algorithmic complexity reduction, hot-path optimization, profiling analysis, memory allocations, I/O bottlenecks, caching strategies, latency and throughput tuning.
* **Positive Activation Triggers**: Eliminating proven bottlenecks; hot loops; excessive memory allocations; heavy I/O; slow queries; high latency; profiling data; CPU bottlenecks. Complexity alignment: **Standard / Complex**.
* **Negative Activation Exclusions**: No empirical performance data exists; speculative optimization; standard CRUD logic; user vaguely asking for "clean, fast code". (Core Minimal Change handles).
* **CAN**: Profile bottlenecks, optimize hot loops, introduce caching layers, reduce computational complexity, benchmark before/after.
* **CANNOT**: Speculatively optimize unmeasured code, degrade code readability for micro-benchmarks without evidence.
* **ESCALATE**: Escalate to user when significant performance gains require major architectural rewrites or increased memory footprints.
* **Dependencies**: `testing` (benchmarks, regression tests).
* **Typical Inputs**: Profiling traces, benchmark metrics, hot path code, resource utilization data.
* **Expected Outputs**: Optimized algorithms, memory reuse patterns, caching configurations, comparative benchmark measurements.
* **Typical Verification**: Comparative benchmarks (before vs after), profile verification, memory leak tests.

---

### 8. concurrency
* **Directory**: `../professional-coding-concurrency/`
* **Status**: Installed & Active (Batch 3)
* **Purpose**: Parallelism, thread safety, synchronization primitives (mutexes, semaphores, atomics, channels), race condition elimination, deadlock avoidance, async execution ordering, and shared mutable state management.
* **Positive Activation Triggers**: Managing parallel or asynchronous execution; threads; async routines; locks; shared queues; race conditions; deadlocks; thread safety; background workers; concurrent workers. Complexity alignment: **Complex**.
* **Negative Activation Exclusions**: Single-threaded synchronous logic; standard sequential execution; code using standard async/await for basic linear I/O. (Core handles internally).
* **CAN**: Design thread-safe state access, introduce synchronization primitives, resolve race conditions and deadlocks, order async execution.
* **CANNOT**: Introduce concurrency where simple sequential execution satisfies throughput requirements.
* **ESCALATE**: Escalate to user when resolving a concurrency deadlock requires changing cross-system communication protocols.
* **Dependencies**: `performance` (when throughput is affected); `testing` (concurrency stress tests).
* **Typical Inputs**: Concurrent workflows, shared mutable state, threading models, contention profiles.
* **Expected Outputs**: Synchronized data structures, lock/channel primitives, thread-safe accessors, worker pools.
* **Typical Verification**: Thread sanitizer / race detector runs, stress testing under concurrent load, deadlock-freedom verification.

---

### 9. dependencies
* **Directory**: `../professional-coding-dependencies/`
* **Status**: Installed & Active (Batch 3)
* **Purpose**: Third-party library evaluation, 5-step dependency gate, package manifest management, lockfile integrity, license compatibility, version constraints, and supply-chain safety.
* **Positive Activation Triggers**: Adding, removing, or auditing packages; package manifests; lockfiles; build definitions; dependency conflicts; license checks; upgrading libraries. Complexity alignment: **Standard**.
* **Negative Activation Exclusions**: Standard library or existing project utilities already solve the requirement; task requires basic JSON parsing or math. (Core Tree 2 Gate handles).
* **CAN**: Evaluate third-party libraries, execute 5-step dependency gate, update manifests and lockfiles, check license compatibility.
* **CANNOT**: Add external dependencies for trivial utilities, add unmaintained or non-permissively licensed packages.
* **ESCALATE**: Escalate to user before adding any heavy framework or third-party dependency not already present in the repository.
* **Dependencies**: `security` (for vulnerability and CVE auditing).
* **Typical Inputs**: Package manifests (`package.json`, `Cargo.toml`, `pyproject.toml`, `pubspec.yaml`, `go.mod`), lockfiles, library requirements.
* **Expected Outputs**: Updated dependency manifests, updated lockfiles, dependency evaluation summary (5-step gate analysis).
* **Typical Verification**: Clean package installation / lockfile validation, license compliance scan, build pass without deprecation errors.

---

### 10. refactoring
* **Directory**: `../professional-coding-refactoring/`
* **Status**: Installed & Active (Batch 3)
* **Purpose**: Structural code improvements, code smell reduction, duplication elimination, modular decomposition, and maintainability enhancement without altering observable behavior.
* **Positive Activation Triggers**: Improving code structure while preserving behavior; multi-function logic; legacy modules; duplicated code; extracting helpers; cleaning up technical debt; decomposing classes; modularizing. Complexity alignment: **Standard / Complex**.
* **Negative Activation Exclusions**: Fixing an urgent bug; implementing an unrelated feature; drive-by opportunistic cleanup when encountering adjacent messy code during a bug fix. (Scope Firewall logs debt).
* **CAN**: Reduce code duplication, extract helper functions, improve naming and cohesion, preserve 100% of observable behavior.
* **CANNOT**: Perform drive-by refactoring during bug fixes, alter public APIs during internal cleanup, change behavior without tests.
* **ESCALATE**: Escalate to user when desired refactoring reveals fundamental structural flaws requiring architectural changes.
* **Dependencies**: `testing` (requires existing test baseline before refactoring begins).
* **Typical Inputs**: Existing code to refactor, existing test suite passing as baseline, modularity goals.
* **Expected Outputs**: Refactored clean code, extracted cohesive modules/functions, zero changes to public contracts or observable behavior.
* **Typical Verification**: 100% test pass on existing test suite with zero modifications to test expectations; diff inspection confirming zero semantic drift.

---

### 11. code-review
* **Directory**: `../professional-coding-code-review/`
* **Status**: Installed & Active (Batch 3)
* **Purpose**: Rigorous self-review and PR evaluation using the 6 C's (Correctness, Clarity, Consistency, Completeness, Conformance, Constraint), pre-commit quality gates, diff auditing, and technical debt logging.
* **Positive Activation Triggers**: Auditing diffs, pull requests, and quality gates; completed code diffs; staged commits; PR descriptions; reviewing PRs; inspecting diffs; pre-commit audits; quality checklist. Complexity alignment: **Standard / Complex**.
* **Negative Activation Exclusions**: Actively writing initial code drafts; exploratory prototyping; trivial one-line fixes; user asking agent to "write and review code" (Core executes 5 phases).
* **CAN**: Audit diffs against the 6 C's, check linter/style compliance, verify Scope Firewall adherence, log technical debt observations.
* **CANNOT**: Rewrite code directly during review (must provide structured feedback), approve changes that violate Git safety.
* **ESCALATE**: Escalate to user when review reveals unresolved high-risk security, data, or architectural concerns.
* **Dependencies**: None. (Audits outputs from all implementation specialists).
* **Typical Inputs**: Git diffs, staged changes, PR descriptions, test results, commit messages.
* **Expected Outputs**: Structured code review report (6 C's analysis), pre-commit quality gate validation, list of identified observations / technical debt.
* **Typical Verification**: Linter checks, automated quality gates, diff verification against Scope Firewall.

---

## Multi-Specialist Composition Examples

Legitimate multi-specialist composition is an explicit core design capability of Professional Coding V3:

| Scenario ID | Task Scenario | Participating Specialists | Coordination Flow |
|---|---|---|---|
| **C-01** | API Endpoint with DB Persistence | `architecture` + `api-contracts` + `data` + `testing` | Component boundaries $\rightarrow$ request/response schema $\rightarrow$ data models/migrations $\rightarrow$ tests pass |
| **C-02** | Security-Sensitive Bug Fix | `debugging` + `security` + `testing` | Defect isolated $\rightarrow$ input sanitization applied $\rightarrow$ regression test added $\rightarrow$ 3-cycle breaker respected |
| **C-03** | High-Throughput Async Pipeline | `concurrency` + `performance` + `testing` | Thread-safe locks $\rightarrow$ measured throughput gain $\rightarrow$ stress tests confirm zero race conditions |
| **C-04** | Full-Stack UI + Backend Feature (OpenDesign Present) | `opendesign` + `api-contracts` + `testing` | OpenDesign implements UI $\rightarrow$ Core wires backend API $\rightarrow$ end-to-end integration verified |
| **C-05** | Full-Stack UI + Backend Feature (OpenDesign Absent) | `api-contracts` + `testing` (OpenDesign Fallback) | Existing repo UI tokens preserved $\rightarrow$ backend API implemented $\rightarrow$ user informed of design boundary |

---

## Evidence-Driven Rerouting Protocol

Specialist routing is provisional and dynamic:
1. **Promote Candidate**: If investigation during Phase 1 (Understand) or Phase 3 (Implement) reveals that a new domain is genuinely affected, Core activates the corresponding specialist provided negative exclusions do not apply.
2. **Deactivate Unneeded**: If investigation proves a suspected domain is not affected, Core immediately retires that specialist to preserve context hygiene.
3. **Escalate Complexity**: If newly activated specialists expand the blast radius beyond the initial tier, escalate the task complexity tier (e.g., Standard → Complex).

