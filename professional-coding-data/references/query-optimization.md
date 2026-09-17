# Database Query Optimization, Indexing & Plan Analysis

Guidelines for analyzing query execution plans, indexing strategies, and eliminating query bottlenecks.

---

## 1. Indexing Strategy & Types

- **B-Tree Indexes**: Standard for equality (`=`) and range (`<`, `<=`, `>`, `>=`, `BETWEEN`) queries.
- **Composite Indexes**:
  - Multi-column index: `(status, created_at, user_id)`.
  - **Left-Most Prefix Rule**: A composite index on `(A, B, C)` can speed up queries on `(A)`, `(A, B)`, and `(A, B, C)`, but NOT queries on `(B)` or `(C)` alone.
  - Put equality columns first, range/sort columns last.
- **Covering Indexes**: An index containing all columns requested by a query (e.g. `INCLUDE (name, email)` in PostgreSQL). Eliminates the expensive heap/table lookup entirely.
- **Partial / Filtered Indexes**: An index with a `WHERE` clause (e.g. `WHERE active = true`). Drastically reduces index size for skewed distributions.

---

## 2. Analyzing Execution Plans (`EXPLAIN`)

Always inspect query plans before optimizing:
1. **Sequential Scans (Seq Scan / Table Scan)**:
   - Acceptable for small tables (<1,000 rows).
   - Inefficient on large tables; indicates missing or unselective indexes.
2. **Index Scan vs. Index Only Scan**:
   - `Index Only Scan` is the fastest access path (covering index).
3. **Join Types**:
   - `Hash Join`: Standard for unsorted large sets.
   - `Merge Join`: Very fast when both inputs are pre-sorted by an index.
   - `Nested Loop`: Efficient when the outer set is tiny and inner set has an index.

---

## 3. The N+1 Query Antipattern

- **Symptom**: Querying 1 parent record, then executing N separate queries in a loop to fetch related child records.
- **Detection**: High query volume proportional to data set size; high database latency under load.
- **Remediation**:
  - Use SQL `JOIN` or `IN (...)` batching.
  - In ORMs: use eager loading (`include`, `select_related`, `prefetch_related`, `JOIN FETCH`).
