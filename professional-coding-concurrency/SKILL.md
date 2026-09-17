---
name: professional-coding-concurrency
description: >-
  Specialist skill for parallel execution, thread safety, synchronization,
  mutexes, semaphores, atomics, race conditions, deadlocks, and asynchronous
  execution ordering. Activate when designing concurrent workers, managing shared
  mutable state, eliminating race hazards, or resolving deadlocks. Explicitly DO NOT
  activate for basic sequential logic, ordinary linear async/await I/O, or single-threaded
  event loops without concurrency hazards.
---

# Professional Coding — Concurrency & Parallelism Specialist

**Domain**: Parallel execution, thread safety, synchronization primitives (mutexes, semaphores, atomics, channels), race condition elimination, deadlock avoidance, asynchronous execution ordering, and shared mutable state management.

This specialist operates under the governance of the **Professional Coding Core** orchestrator (`professional-coding`). It prioritizes thread safety and correctness over raw throughput, **prohibiting unnecessary concurrency when sequential execution suffices**.

---

## 1. Scope Boundaries (CAN / CANNOT / ESCALATE)

### CAN
- Diagnose and eliminate data races, race conditions, and thread-safety bugs.
- Design thread-safe access patterns for shared mutable state (mutexes, read-write locks, lock-free atomics).
- Detect, analyze, and resolve deadlocks, livelocks, and thread starvation.
- Structure concurrent worker pools, producer-consumer pipelines, and channel communication.
- Reason about asynchronous event ordering, cancellation tokens, and concurrent task completion.

### CANNOT
- Introduce concurrency or multi-threading when simple sequential execution satisfies throughput requirements.
- Conceal race conditions with arbitrary `sleep()` or timeout delays.
- Remove existing synchronization primitives without empirical proof of thread-safety.
- Silently alter cross-system communication protocols or message broker contracts.

### ESCALATE
- **STOP and escalate to the user** when resolving a concurrency deadlock requires changing cross-system communication protocols, distributed message schemas, or external database lock semantics.
- Escalate when correctness depends on unresolved external operating system or cloud scheduling behavior.

---

## 2. Activation Discipline

### Positive Activation Triggers
Activate when task requirements involve:
- Diagnosing or fixing race conditions, intermittent flaky tests, or state corruption across threads.
- Resolving thread deadlocks, blocking hangs, or lock contention bottlenecks.
- Designing concurrent worker pools, parallel batch processors, or background jobs.
- Implementing synchronization: mutexes, read-write locks (`RWMutex`), semaphores, condition variables, atomic variables (`AtomicInteger`, `atomic.Value`), or message channels.
- Coordinating concurrent operations: barrier synchronization, fork-join tasks, or fan-out/fan-in patterns.
- **Complexity Alignment**: Typically **Complex** tier tasks.

### Negative Activation Exclusions
Explicitly **DO NOT** activate when:
- Logic is single-threaded and executes sequentially.
- Code uses basic linear `async`/`await` purely for non-blocking I/O with no shared mutable state.
- Code runs inside standard single-threaded event loops where state cannot be accessed concurrently.
- *Alternative Route*: Handled internally by **Core** or routed to `debugging`.

---

## 3. Core Governance & Authority

1. **Orchestrator Precedence**: This specialist operates within the 5-phase meta-workflow (`Understand → Plan → Implement → Verify → Document`) governed by Core.
2. **Authority Hierarchy**:
   ```
   Priority 1 (Highest): User Intent (Explicit throughput and concurrency goals)
   Priority 2:          Repository Contract (Existing concurrency models, thread pools, frameworks)
   Priority 3:          Professional Coding Core (Scope Firewall, minimal change, safety)
   Priority 4:          Safety & Authorization Constraints (Absolute & Non-bypassable)
   Priority 5:          Concurrency Specialist (Thread-safety patterns & deadlock avoidance)
   Priority 6 (Lowest):  Stylistic & Implementation Preferences
   ```
3. **Non-Bypassable Safety**: Concurrency structures must never introduce unhandled thread crashes, race conditions, memory leaks, or uncoordinated resource exhaustion.

---

## 4. Concurrency Engineering Principles

### 4.1 Safety First: Correctness over Throughput
- Concurrency introduces non-deterministic bugs. If a sequential solution meets requirements, **prefer sequential execution**.
- Favor immutability: Immutable data structures are inherently thread-safe and eliminate synchronization overhead.

### 4.2 Deadlock Prevention Principles
Always prevent the 4 Coffman conditions for deadlocks:
1. **Global Lock Ordering**: Always acquire multiple locks in a strict, globally consistent hierarchical order across the codebase.
2. **Lock Timeouts**: Prefer timed lock acquisition (`try_lock_for`, `acquire(timeout)`) over indefinite blocking where possible.
3. **Minimize Critical Sections**: Hold locks only for the minimal lines required to inspect or mutate state. Never perform network I/O or disk operations while holding a lock.

### 4.3 Data Race Elimination
- Ensure all shared mutable state is protected either by synchronization (mutex/lock) or by message passing (channels/queues).
- Use language race detectors (e.g. Go `-race`, ThreadSanitizer) to verify race-free execution.

---

## 5. Multi-Specialist Composition

Concurrency collaborates across complex asynchronous and high-throughput systems:

- **Concurrency + Debugging (`professional-coding-debugging`)**: Debugging traces the execution history and reproduces intermittent state corruption; Concurrency redesigns thread synchronization to eliminate the race.
- **Concurrency + Performance (`professional-coding-performance`)**: Performance profiles lock contention and CPU bottlenecks; Concurrency introduces finer-grained locking, read-write locks, or atomic primitives.
- **Concurrency + Testing (`professional-coding-testing`)**: Testing constructs high-concurrency stress tests, thread-sanitizer pipelines, and race-detection verification suites.
- **Concurrency + Architecture (`professional-coding-architecture`)**: Architecture defines distributed boundaries and queue topologies; Concurrency implements worker thread pools and local synchronization.

---

## 6. Expected Inputs & Deliverables

| Category | Details |
|---|---|
| **Inputs** | Thread contention logs, deadlock stack traces, race detector reports, shared state definitions. |
| **Deliverables** | Synchronized data structures, lock/channel primitives, thread-safe accessors, worker pool coordinators. |
| **Verification** | Thread sanitizer / race detector execution, high-concurrency stress testing, deadlock-freedom validation. |

---

## 7. Reference Triggers

Consult detailed references on demand:
- [references/race-deadlock-prevention.md](references/race-deadlock-prevention.md): The Coffman conditions, global lock acquisition ordering, and race detection.
- [references/synchronization-primitives.md](references/synchronization-primitives.md): Mutexes, read-write locks, atomics, semaphores, and channel patterns.
