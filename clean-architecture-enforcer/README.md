# Clean Architecture Enforcer

> Keeps domain, application, and infrastructure layers separated. Flags every leak across boundaries.

**Pack slug:** `clean-architecture-enforcer`
**Version:** 1.0.0
**Author:** ModelBound (Official)

---

## What it does

Trains your AI agent to respect Clean Architecture / Hexagonal / DDD layering rules:

- **Domain layer** has zero framework imports (no Express, no Prisma, no React)
- **Application layer** orchestrates domain + ports, never reaches into infrastructure directly
- **Infrastructure layer** implements ports defined by inner layers, never imports from application logic
- **Dependencies point inward only** — outer layers depend on inner, never the reverse
- **Cross-cutting concerns** (logging, auth, caching) go through interfaces, not direct calls

When the agent writes or reviews code, it flags any import that crosses a boundary the wrong way.

## Who it's for

- **Backend teams** building long-lived systems (5+ year horizon)
- **DDD practitioners** who want their agent to stop pulling ORM models into use cases
- **Tech leads** enforcing architecture in growing codebases
- **Anyone migrating** from a "fat controller" pattern toward layered design

Not for: rapid prototypes, scripts, or apps where YAGNI clearly wins.

## What's inside

- `layer-rules.md` — the four layers and what each may import
- `dependency-direction.md` — concrete examples of inward-only deps
- `port-adapter-pattern.md` — how to define interfaces in domain, implement in infra
- `common-leaks.md` — top 10 anti-patterns this pack catches
- `migration-guide.md` — moving an existing codebase toward clean layers

## Install

### Cursor (`.cursor/rules/`)

```mdc
---
description: Enforce Clean Architecture layering. Flag any import that crosses a layer boundary the wrong way.
globs: ["src/**/*.ts", "src/**/*.py", "src/**/*.go"]
alwaysApply: true
---
```

### Kiro (`.kiro/steering/architecture.md`)

Add as steering. Kiro will reference layer rules on every code generation.

### Windsurf (`.windsurfrules`)

Add the layer rules to root. Windsurf is good at catching cross-boundary imports if the rules are clear.

### Claude Code (`.claude/skills/clean-architecture/SKILL.md`)

```yaml
---
name: clean-architecture-enforcer
description: Enforce Clean Architecture / Hexagonal layering. Use when generating, reviewing, or refactoring backend code in a layered codebase.
---
```

## Usage tips

- **Document your layer mapping.** The pack defines the *rules*; you need to tell the agent which folders map to which layer (e.g. `src/domain/`, `src/application/`, `src/infrastructure/`). Add this to your project `instructions.md`.
- **Add a dependency-cruiser config** in CI as a backstop. The agent catches issues at write time; dependency-cruiser catches them at PR time.
- **Don't apply to scripts/migrations** — exclude `scripts/` and `migrations/` from the glob.
- **Pair with `api-design`** for the application layer and `sql-migration-reviewer` for infra.

## Compatibility

| IDE / Agent | Status |
|---|---|
| Cursor | ✅ Recommended |
| Kiro | ✅ Recommended |
| Windsurf | ✅ Supported |
| Claude Code | ✅ Supported |
| Copilot | ⚠️ Partial — works, but Copilot is less strict about imports |
| Continue.dev | ✅ Supported |

## License

MIT — © ModelBound.
