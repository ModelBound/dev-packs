# SQL Migration Reviewer

> Reviews Postgres migrations for safety, locking, rollback, and online compatibility. Catches the migration that takes the site down.

**Pack slug:** `sql-migration-reviewer`
**Version:** 1.0.0
**Author:** ModelBound (Official)

---

## What it does

Configures your AI agent to review Postgres schema migrations the way a DBA who's been paged at 3am would:

- **Locks**: flags `ALTER TABLE` statements that take `ACCESS EXCLUSIVE` on large tables without `lock_timeout`
- **Online compatibility**: pushes for `ADD COLUMN ... NULL` + backfill + `SET NOT NULL`, not single-statement `ADD COLUMN ... NOT NULL DEFAULT ...` on big tables (PG 11+ helps but old habits die hard)
- **Index changes**: requires `CREATE INDEX CONCURRENTLY` for production tables
- **Constraint additions**: prefers `NOT VALID` + `VALIDATE CONSTRAINT` to avoid full-table scans under lock
- **Rollback**: every migration must have a documented rollback plan (or be irreversible-by-design with a note)
- **Data migrations**: separate from schema migrations; batched, idempotent, resumable
- **Foreign keys & triggers**: warns when adding to large tables
- **Renames**: flags as breaking changes that need app-side coordination
- **Drops**: requires a deprecation window with monitoring before any `DROP COLUMN`

## Who it's for

- **Backend teams** running Postgres in production with non-trivial uptime needs
- **DBAs/SREs** reviewing migrations from app teams
- **Platform engineers** building migration tooling
- **Anyone** who has ever shipped a migration that took an `ACCESS EXCLUSIVE` lock for 40 minutes

## What's inside

- `lock-safety.md` — which DDL takes which lock, and how to avoid it
- `online-migrations.md` — patterns for zero-downtime schema changes
- `rollback-plans.md` — how to document and test rollbacks
- `data-migration-patterns.md` — batching, checkpointing, idempotency
- `dangerous-operations.md` — the list of statements that need extra review

## Install

### Cursor (`.cursor/rules/`)

```mdc
---
description: Review Postgres migrations for lock safety, online compatibility, and rollback.
globs: ["**/migrations/**/*.sql", "**/migrate/**/*.sql", "**/*.migration.sql", "**/db/migrations/**"]
alwaysApply: false
---
```

### Kiro (`.kiro/steering/sql-migrations.md`)

Add as steering. Especially valuable in Kiro since spec workflows often include schema changes.

### Windsurf (`.windsurf/rules/sql-migrations.md`)

Drop in as a rule scoped to migration folders.

### Claude Code (`.claude/skills/sql-migration-reviewer/SKILL.md`)

```yaml
---
name: sql-migration-reviewer
description: Review a Postgres schema migration for production safety. Use whenever migration .sql files are created or modified.
---
```

## Usage tips

- **Tell the agent your table sizes.** "Users table has 50M rows" changes which patterns are acceptable.
- **Specify your migration tool** (Flyway, Alembic, Prisma, Knex, sqlx, dbmate). The agent will tailor the rollback syntax.
- **Run it on every migration PR** as a first-pass review — even ones written by humans.
- **Pair with a CI check** like [Squawk](https://github.com/sbdchd/squawk) as a second line of defense.
- **For MySQL/MariaDB**, ask the agent to translate the rules — most are applicable but the lock semantics differ.

## Compatibility

| IDE / Agent | Status | Notes |
|---|---|---|
| Cursor | ✅ Recommended | Glob to migration folders |
| Kiro | ✅ Recommended | Pairs with spec workflows |
| Windsurf | ✅ Supported | |
| Claude Code | ✅ Supported | |
| Copilot | ⚠️ Partial — Copilot is weaker on SQL nuance |
| Continue.dev | ✅ Supported | |

## License

MIT — © ModelBound.
