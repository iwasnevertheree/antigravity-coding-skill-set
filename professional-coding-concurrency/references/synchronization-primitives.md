# Synchronization Primitives, Lock Hygiene & Channel Patterns

Guidelines for selecting and using mutexes, read-write locks, atomics, and channels.

---

## 1. Synchronization Primitive Selection

| Primitive | Best Used For | Trade-offs & Caveats |
|---|---|---|
| **Standard Mutex** | Protecting short critical sections with frequent writes. | Simple, predictable; blocks threads on contention. |
| **Read-Write Lock (`RWMutex`)** | Data structures with frequent reads (>90%) and rare writes. | Higher overhead than standard mutex; write-starvation if readers dominate. |
| **Atomics (`Atomic*`)** | Single counter increments, state flags, or pointer swaps. | Lock-free, zero OS context switches; difficult to coordinate multiple variables. |
| **Semaphores** | Limiting concurrency to N concurrent workers (rate limiting). | Useful for resource throttling (e.g. max 10 concurrent database connections). |
| **Channels / Queues** | Communicating state across tasks (producer-consumer). | Clean isolation; channel buffer sizing and deadlock on full buffers must be handled. |

---

## 2. Lock Hygiene Rules

1. **Keep Critical Sections Minimal**:
   ```python
   # Anti-pattern: Holding lock during expensive work
   with lock:
       result = do_expensive_network_call()
       shared_state.append(result)

   # Correct: Perform work outside lock, lock only for state update
   result = do_expensive_network_call()
   with lock:
       shared_state.append(result)
   ```
2. **Always Release Locks via Language Guards**:
   - Python: `with lock:`
   - Go: `mu.Lock(); defer mu.Unlock()`
   - Rust: `let guard = lock.lock().unwrap();`
   - Java: `try { lock.lock(); ... } finally { lock.unlock(); }`
3. **Avoid Shared Mutable State**: Favor "share memory by communicating; don't communicate by sharing memory".
