# Race Condition Elimination & Deadlock Prevention

Guidelines for diagnosing, isolating, and eliminating concurrency hazards in multithreaded and asynchronous software.

---

## 1. Race Conditions vs. Data Races

- **Data Race**: Two or more concurrent instructions access the same memory location without synchronization, and at least one access is a write. (Undefined behavior in C/C++/Go/Rust).
- **Race Condition**: A semantic flaw where the correctness of a program depends on the relative timing or interleaving of concurrent operations (e.g. Check-Then-Act / Time-of-Check to Time-of-Use).
- **Rule**: Eliminating data races via mutexes does not automatically eliminate race conditions. The entire logical transaction must be atomic.

---

## 2. Deadlock Elimination (The Coffman Conditions)

A deadlock occurs only when all four conditions hold simultaneously:
1. **Mutual Exclusion**: Resources cannot be shared.
2. **Hold and Wait**: A thread holding a resource waits for another.
3. **No Preemption**: Resources cannot be forcibly revoked.
4. **Circular Wait**: Thread A waits for Thread B while Thread B waits for Thread A.

### Prevention Strategies
- **Hierarchical Lock Ordering**: Assign a strict global integer rank to every lock. Always acquire locks in ascending order.
- **Lock Coarsening**: Combine separate fine-grained locks into a single cohesive mutex protecting related invariant state.
- **Avoid Nested Locks**: Never acquire a second lock inside the critical section of a first lock if preventable.

---

## 3. Detecting Concurrency Hazards

- **Static Analysis**: Linters that flag lock-ordering inversions or unsynchronized field access.
- **Dynamic Race Detectors**:
  - Go: `go test -race ./...`
  - C/C++: Clang/GCC ThreadSanitizer (`-fsanitize=thread`).
  - Java/JVM: Concurrency testing frameworks (e.g. jcstress).
- **Stress Testing**: Run 1,000+ iterations with randomized thread schedules to expose timing windows.
