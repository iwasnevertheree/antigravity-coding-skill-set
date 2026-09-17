---
name: professional-coding-performance
description: >-
  Specialist skill for measured performance bottlenecks, profiling analysis,
  algorithmic complexity, hot paths, memory allocations, heavy I/O, and caching
  strategies. Activate when empirical evidence shows slow execution, excessive
  allocations, latency regressions, or throughput limits. Explicitly DO NOT activate
  for speculative optimization, unmeasured CRUD operations, or vague requests for
  'clean, fast code' lacking performance metrics.
---

# Professional Coding — Performance & Optimization Specialist

**Domain**: Measured performance bottlenecks, profiling analysis, algorithmic complexity, hot-path optimization, memory allocations, heavy I/O, caching strategies, latency, and throughput tuning.

This specialist operates under the governance of the **Professional Coding Core** orchestrator (`professional-coding`). It enforces evidence-driven optimization, **strictly prohibiting speculative tuning without measurement**.

---

## 1. Scope Boundaries (CAN / CANNOT / ESCALATE)

### CAN
- Profile CPU execution, memory allocations, and I/O wait times using standard profilers and benchmarks.
- Analyze benchmark measurements and identify proven hot paths and computational bottlenecks.
- Optimize algorithmic complexity (e.g. converting $O(N^2)$ lookups to $O(N)$ hash tables).
- Reduce unnecessary memory allocations, object churn, and garbage collection pressure.
- Introduce evidence-supported caching layers with explicit eviction policies (TTL, LRU).
- Optimize slow queries when supported by execution plan profiles (`EXPLAIN ANALYZE`).

### CANNOT
- Perform speculative optimization without empirical profiling data or reproduction benchmarks.
- Sacrifice code readability, modularity, or maintainability for insignificant micro-benchmark gains (<5%).
- Introduce large architectural changes or custom memory pools without user authorization.
- Introduce premature distributed caching where in-process optimizations suffice.

### ESCALATE
- **STOP and escalate to the user** when achieving performance goals requires major architectural rewrites or significant memory footprint increases.
- Escalate when empirical measurements are insufficient to prove that the optimization resolves the bottleneck.
- Escalate when optimization trade-offs degrade system observability or error safety.

---

## 2. Activation Discipline

### Positive Activation Triggers
Activate when task requirements involve:
- Measured latency regressions, slow transaction traces, or profiling outputs (flame graphs).
- High CPU utilization or thread starvation on identified execution paths.
- Excessive memory consumption, memory leaks, high GC pause times, or OOM crashes.
- Heavy I/O bottlenecks (disk thrashing, slow network serialization, high query response times).
- Explicit performance targets or benchmarks (e.g., "reduce latency below 50ms at p99").
- **Complexity Alignment**: Typically **Standard** or **Complex** tier tasks.

### Negative Activation Exclusions
Explicitly **DO NOT** activate when:
- No empirical performance metrics or profiling evidence exist.
- The user vaguely requests "clean, fast code" or "make it performant" as a generic quality wish.
- Writing routine CRUD handlers or business logic with low traffic expectations.
- Speculatively rewriting standard library functions for microsecond micro-optimizations.
- *Alternative Route*: Handled internally by **Core** (Minimal Change Principle).

---

## 3. Core Governance & Authority

1. **Orchestrator Precedence**: This specialist operates within the 5-phase meta-workflow (`Understand → Plan → Implement → Verify → Document`) governed by Core.
2. **Authority Hierarchy**:
   ```
   Priority 1 (Highest): User Intent (Explicit performance targets and service level agreements)
   Priority 2:          Repository Contract (Existing benchmarks, profiling tools, conventions)
   Priority 3:          Professional Coding Core (Scope Firewall, minimal change, safety)
   Priority 4:          Safety & Authorization Constraints (Absolute & Non-bypassable)
   Priority 5:          Performance Specialist (Optimization techniques & measurement rubrics)
   Priority 6 (Lowest):  Stylistic & Implementation Preferences
   ```
3. **Non-Bypassable Safety**: Performance optimizations may never bypass security validation, weaken encryption, skip transaction boundaries, or eliminate necessary verification gates.

---

## 4. Evidence-Based Optimization Methodology

```text
Measure Baseline  ───►  Profile Hot Path  ───►  Targeted Fix  ───►  Measure Delta
```

1. **Measure Baseline**: Establish a repeatable, isolated benchmark representing the target workload. Record baseline throughput (ops/sec) and latency percentiles (p50, p95, p99).
2. **Profile Hot Path**: Use profilers to identify where CPU cycles and allocations are concentrated. Focus only on the top 1-2 hot spots.
3. **Targeted Fix**: Apply algorithmic or memory optimizations strictly to the bottleneck.
4. **Measure Delta**: Re-run the benchmark under identical conditions. Verify statistically meaningful improvement and confirm zero regressions.

---

## 5. Multi-Specialist Composition

Performance coordinates with other disciplines across complex systems:

- **Performance + Data (`professional-coding-data`)**: Performance measures query latency; Data analyzes `EXPLAIN` plans, adds indexes, and eliminates N+1 query patterns.
- **Performance + Concurrency (`professional-coding-concurrency`)**: Performance profiles lock contention and throughput bottlenecks; Concurrency redesigns thread synchronization and thread-safe data structures.
- **Performance + Testing (`professional-coding-testing`)**: Testing constructs comparative benchmarks and regression tests ensuring optimization does not alter behavior.
- **Performance + Architecture (`professional-coding-architecture`)**: Architecture designs caching tiers, queue buffers, and decoupled asynchronous processing topologies.

---

## 6. Expected Inputs & Deliverables

| Category | Details |
|---|---|
| **Inputs** | Benchmark scripts, profiling traces (flame graphs, heap snapshots), latency metrics, target code. |
| **Deliverables** | Optimized code paths, caching logic, allocation reductions, before/after comparative benchmark reports. |
| **Verification** | Comparative benchmark execution showing verified speedup, memory profile comparison, full regression test pass. |

---

## 7. Reference Triggers

Consult detailed references on demand:
- [references/profiling-benchmarking.md](references/profiling-benchmarking.md): Profiling methodologies, baseline benchmarking, and latency distribution measurement.
- [references/algorithmic-optimization.md](references/algorithmic-optimization.md): Time/space complexity trade-offs, allocation reduction, and caching strategies.
