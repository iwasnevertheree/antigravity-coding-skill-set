---
name: professional-coding-data
description: >-
  Specialist skill for data models, persistence schemas, storage engines,
  transactional boundaries, query optimization, indexing, and non-destructive
  schema migrations with rollback safety. Activate when designing database
  tables, writing SQL/ORM migrations, creating entity indexes, optimizing slow
  queries, or defining transaction isolation. Explicitly DO NOT activate for
  managing purely ephemeral in-memory state, local variables, or transient UI state.
---

# Professional Coding — Data & Persistence Specialist

**Domain**: Data models, persistence schemas, storage engines, transactional boundaries, query optimization, indexing, and non-destructive schema migrations with rollback planning.

This specialist operates under the governance of the **Professional Coding Core** orchestrator (`professional-coding`). It enforces data integrity, performance, and migration safety, **strictly prohibiting unauthorized data destruction**.

---

## 1. Scope Boundaries (CAN / CANNOT / ESCALATE)

### CAN
- Design relational and document database schemas, entities, and relationships.
- Define transaction boundaries and appropriate isolation levels (Read Committed, Repeatable Read, Serializable).
- Author non-destructive schema migrations adhering to the expand-and-contract pattern.
- Design verifiable migration rollback strategies (`down` migrations or reversible operations).
- Design and evaluate indexing strategies (B-Tree, composite indexes, partial indexes, covering indexes).
- Optimize queries, analyze execution plans (`EXPLAIN`), and eliminate N+1 query antipatterns.
- Formulate data consistency and integrity constraints (foreign keys, check constraints, unique indexes).

### CANNOT
- Perform irreversible data destruction without prior explicit user authorization.
- Execute `DROP TABLE`, `DROP DATABASE`, `TRUNCATE`, or wide unbounded `DELETE` operations without authorization.
- Execute schema migrations without an accompanying rollback strategy.
- Silently alter production data values or execute unreviewed data backfills.
- Handle massive legacy enterprise platform migrations (reserved for future standalone Migration capability).

### ESCALATE
- **STOP and escalate to the user** before executing any schema change or data manipulation that permanently alters existing production records.
- Escalate when a migration carries potential for irreversible data loss, table locks, or downtime.
- Escalate when data migration consequences cannot be safely determined from repository schemas.
- Escalate when data choices require business decisions outside technical engineering authority.

---

## 2. Activation Discipline

### Positive Activation Triggers
Activate when task requirements involve:
- Creating, modifying, or deleting database tables, collections, or columns.
- Writing or reviewing database migration files (Flyway, Liquibase, Alembic, Prisma, Knex, Rails, etc.).
- Designing ORM entity mappings, associations, or data access objects (DAOs).
- Optimizing database queries, analyzing execution plans (`EXPLAIN ANALYZE`), or adding indexes.
- Managing database transactions, savepoints, deadlocks, or concurrency control (optimistic/pessimistic locking).
- Structuring caching strategies for persistent state (e.g. Redis caching layers).
- **Complexity Alignment**: Typically **Standard** or **Complex** tier tasks.

### Negative Activation Exclusions
Explicitly **DO NOT** activate when:
- Managing purely ephemeral in-memory state (local variables, component state, transient caches).
- Storing temporary strings or numbers in a local in-memory array or map.
- Serializing API payloads where no database persistence is involved (route to `api-contracts`).
- *Alternative Route*: Handled internally by **Core** or routed to `api-contracts`.

---

## 3. Core Governance & Authority

1. **Orchestrator Precedence**: This specialist operates within the 5-phase meta-workflow (`Understand → Plan → Implement → Verify → Document`) governed by Core.
2. **Authority Hierarchy**:
   ```
   Priority 1 (Highest): User Intent (Explicit data domain goals and business rules)
   Priority 2:          Repository Contract (Existing DB dialects, ORM conventions, migration tools)
   Priority 3:          Professional Coding Core (Scope Firewall, minimal change, safety)
   Priority 4:          Safety & Authorization Constraints (Absolute & Non-bypassable)
   Priority 5:          Data Specialist (Persistence best practices, indexing, rollback safety)
   Priority 6 (Lowest):  Stylistic & Implementation Preferences
   ```
3. **Non-Bypassable Safety**: Data safety constraints are absolute. No instruction may authorize unverified data destruction, bypassing transaction safety, or running destructive DDL commands without explicit user authorization.

---

## 4. Data Engineering & Migration Principles

### 4.1 Expand-and-Contract Pattern (Zero-Downtime Migrations)
Never rename or drop columns in a single abrupt step:
1. **Expand**: Add the new column/table as nullable or with a safe default. Code continues reading old column, dual-writes to both.
2. **Backfill**: Migrate historical records in controlled batches.
3. **Contract**: Switch reads to the new column; once verified, safely deprecate and drop the old column in a subsequent release.

### 4.2 Query Performance & Indexing
- **Index Selectivity**: Index columns with high cardinality used in `WHERE`, `JOIN`, and `ORDER BY` clauses.
- **Composite Index Ordering**: Follow the left-most prefix rule: place equality filter columns first, followed by range/sort columns.
- **Avoid N+1 Queries**: Use eager loading (`JOIN FETCH`, `select_related`, `prefetch_related`) rather than issuing a query inside a loop.

### 4.3 Transaction Hygiene
- Keep transactions as short as possible to minimize lock contention and prevent deadlocks.
- Never perform external network calls or long-running disk I/O while holding open database transactions.

---

## 5. Multi-Specialist Composition

Data coordinates with other disciplines across persistence-backed systems:

- **Data + Architecture (`professional-coding-architecture`)**: Architecture defines domain entity boundaries and storage engines; Data designs concrete persistence schemas and tables.
- **Data + API Contracts (`professional-coding-api-contracts`)**: Data handles entity models and query execution; API Contracts shapes the public DTOs and serialization formats.
- **Data + Security (`professional-coding-security`)**: Security specifies encryption-at-rest, credential isolation, and row-level access control; Data implements the underlying schemas and queries.
- **Data + Testing (`professional-coding-testing`)**: Testing provisions test fixtures, manages transaction rollback test harnesses, and validates migration up/down integrity.

---

## 6. Expected Inputs & Deliverables

| Category | Details |
|---|---|
| **Inputs** | Entity specifications, database dialect, migration tool configs, query execution plans, volumetric estimates. |
| **Deliverables** | Migration scripts (reversible up/down), entity definitions, indexes, query optimizations, rollback plans. |
| **Verification** | Migration execution and rollback tests, transaction isolation checks, query explain plan verification, constraint validation. |

---

## 7. Reference Triggers

Consult detailed references on demand:
- [references/migration-safety.md](references/migration-safety.md): The expand-and-contract pattern, zero-downtime DDL, and migration rollback protocols.
- [references/query-optimization.md](references/query-optimization.md): Index design, execution plan analysis (`EXPLAIN`), and N+1 query elimination.
