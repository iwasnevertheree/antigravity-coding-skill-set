# Database Migration Safety, Rollback Strategy & Expand-Contract Pattern

Guidelines for zero-downtime database migrations, reversible DDL, and data integrity preservation.

---

## 1. The Expand-and-Contract Migration Pattern

To safely migrate schemas in production systems without downtime:

```text
Phase 1 (Expand)      ──► Phase 2 (Dual Write)  ──► Phase 3 (Backfill)    ──► Phase 4 (Contract)
Add new column/table      Write to both columns     Migrate old rows to       Drop old column after
as optional/nullable      simultaneously            new column in batches     all readers switch
```

### Safety Rules
1. **Never rename columns directly**: A rename breaks existing running application instances reading the old column name. Use expand-and-contract instead.
2. **Never add `NOT NULL` without a default**: Adding a `NOT NULL` column without a default to a populated table causes table locks and query failures. Add as nullable, backfill values, then apply the `NOT NULL` constraint.
3. **Always author reversible migrations**: Every migration script (`up`) must have an exact inverse operation (`down`). If a change is inherently destructive (e.g., dropping data), provide an explicit pre-migration backup procedure.

---

## 2. Destructive Operations Gate

The following operations require **explicit user authorization** before execution:
- `DROP TABLE`, `DROP DATABASE`, `DROP SCHEMA`
- `TRUNCATE TABLE`
- `ALTER TABLE ... DROP COLUMN`
- `DELETE FROM ...` without a restrictive `WHERE` clause
- Dropping primary keys or unique indexes

---

## 3. Migration Rollback Checklist

Before applying any migration:
- [ ] Has the `down` / rollback script been written and tested in a sandbox?
- [ ] Does the rollback restore schema compatibility with the previous application version?
- [ ] Are table-lock durations acceptable for current traffic volume?
- [ ] Is concurrent index creation (`CREATE INDEX CONCURRENTLY` in PostgreSQL) used to prevent read/write locks?
