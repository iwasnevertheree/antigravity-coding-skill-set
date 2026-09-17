# Algorithmic Optimization, Memory Efficiency & Caching Strategies

Techniques for improving computational efficiency while preserving code maintainability.

---

## 1. Algorithmic Complexity Improvements

- **Nested Loops over Collections**: Replace $O(N \times M)$ linear searches with $O(N + M)$ set/hash lookups.
- **Sorting & Searching**: Use binary search ($O(\log N)$) on pre-sorted arrays rather than linear scans ($O(N)$).
- **Pagination & Chunking**: Process large data streams in memory-bounded batches rather than loading full tables into memory.

---

## 2. Memory Allocation & GC Optimization

- **String Building**: Avoid `str += chunk` in loops (causes $O(N^2)$ memory reallocation). Use string builders, string joins, or byte buffers.
- **Pre-Allocate Capacities**: Pre-size arrays, slices, maps, or collections when the expected item count is known, avoiding incremental array doubling overhead.
- **Avoid Unnecessary Boxing / Cloning**: Pass large immutable data structures by reference/pointer where language semantics permit.

---

## 3. Caching Best Practices

When adding a cache to eliminate an expensive operation:
1. **Cache Key Stability**: Ensure cache keys unambiguously identify the underlying inputs without collision.
2. **Eviction Strategy**: Always configure explicit bounds:
   - **TTL (Time-To-Live)**: Bound maximum staleness.
   - **LRU (Least Recently Used)**: Cap maximum memory footprint.
3. **Cache Invalidation**: Clearly document when and how cache entries are invalidated when underlying persistent records are updated.
