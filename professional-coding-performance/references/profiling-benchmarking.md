# Profiling, Benchmarking & Measurement Protocols

Guidelines for profiling applications, constructing stable benchmarks, and measuring performance deltas.

---

## 1. Benchmarking Best Practices

1. **Warm-Up Runs**: Always discard initial cold-start executions (JIT compilation, cache priming, connection pools).
2. **Representative Workloads**: Test with realistic data sizes and distributions, not tiny toy payloads that fit entirely in L1 CPU cache.
3. **Statistical Validity**: Run multiple iterations (e.g., 10-30 rounds) and report median and percentiles (p50, p95, p99) rather than pure arithmetic means which mask outliers.
4. **Isolate Environment**: Disable background tasks, power throttling, and heavy background processes during measurement.

---

## 2. Profiling Categories & Tooling

| Profiler Type | What It Measures | Typical Bottlenecks Uncovered |
|---|---|---|
| **CPU Profiling** | Time spent inside functions / stack traces | Inefficient algorithms ($O(N^2)$), heavy regex compilation, busy-wait loops. |
| **Allocation / Memory** | Heap allocations, object retention, GC pressure | Temporary string concatenation inside loops, unbuffered I/O reads, memory leaks. |
| **I/O & Tracing** | Network round-trips, database queries, disk wait | N+1 database queries, synchronous external HTTP calls, missing indexes. |
| **Lock Contention** | Threads blocked waiting on mutexes/locks | Coarse-grained locking, high mutex contention, reverse lock order. |

---

## 3. Comparative Benchmark Verification

Before declaring an optimization successful:
- Demonstrate that throughput increased or latency decreased by a statistically significant margin (typically $\ge 15\%$).
- Verify that memory consumption did not explode to achieve the CPU gain.
- Verify 100% test suite pass to ensure behavioral equivalence.
