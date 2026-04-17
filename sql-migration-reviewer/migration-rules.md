# Migration Rules

## One Concern Per Migration

- One logical change per file (e.g. "add user.timezone column" — not "add column + index + backfill + drop old").
- Independent changes get independent files so partial failures are easy to recover.

## Naming

- `YYYYMMDDHHMMSS_<verb>_<noun>.sql` (or framework convention).
- Examples: `20240115093000_add_users_timezone.sql`, `20240120140000_index_invoices_team_id.sql`.

## Reversibility

- Every migration has a rollback strategy:
  - Down migration script, or
  - A documented recovery procedure (e.g. "restore column from snapshot")
- DDL inside a transaction so failures roll back cleanly (Postgres supports this).

## Online-Safe Patterns

- `CREATE INDEX CONCURRENTLY` on any table > 100K rows.
- Add columns nullable first; `SET NOT NULL` after backfill in a separate migration.
- Add foreign keys `NOT VALID`, then `VALIDATE CONSTRAINT` later.
- Long backfills: batch in chunks of 1k–10k rows with `COMMIT` per chunk and a sleep to avoid replication lag.

## Multi-Release Drops

To drop column `x`:

1. Release N: stop writing `x` from app code. Tolerate old reads.
2. Release N+1: stop reading `x` from app code.
3. Release N+2: `ALTER TABLE t DROP COLUMN x`.

Skipping a step risks a window where running pods crash.

## Locking Discipline

- Avoid combining DDL with long-running queries in the same transaction.
- Set `lock_timeout` and `statement_timeout` for risky statements:
  ```sql
  SET lock_timeout = '5s';
  SET statement_timeout = '60s';
  ALTER TABLE ... ;
  ```
- Schedule heavy migrations during low-traffic windows.

## Code-Schema Contract

- The new column or table must exist *before* the code that uses it deploys.
- The old column or table must be removed *after* the code that uses it has been deployed.
- Lock the deployment ordering in CI.
