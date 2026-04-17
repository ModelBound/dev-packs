# API Design

> Designs HTTP + JSON APIs that are versioned, paginated, idempotent, and follow RFC 7807 for errors.

**Pack slug:** `api-design`
**Version:** 1.0.0
**Author:** ModelBound (Official)

---

## What it does

Configures your AI agent to design REST APIs that hold up in production:

- **Versioning** in the URL or `Accept` header — never on a whim
- **Pagination** with cursors (not offset) for any unbounded collection
- **Idempotency keys** for `POST` endpoints that mutate state
- **RFC 7807 problem details** for error responses (`type`, `title`, `status`, `detail`, `instance`)
- **Consistent resource naming** (plural nouns, kebab-case paths, snake_case JSON or camelCase — pick one)
- **Status codes used correctly** (`201` with `Location`, `409` for conflicts, `422` for validation, not `400` for everything)
- **HATEOAS-lite** — include relevant action links in responses without going full hypermedia
- **OpenAPI spec** generated alongside the code, not as an afterthought

## Who it's for

- **Backend teams** designing public or partner-facing APIs
- **Platform engineers** building internal APIs that other teams will depend on for years
- **Solo developers** shipping their first SaaS API and wanting it to age well
- **Anyone migrating** from RPC-style endpoints toward proper REST

## What's inside

- `resource-modeling.md` — nouns, hierarchies, sub-resources
- `versioning.md` — URL vs header, when to bump
- `pagination.md` — cursor-based pagination patterns
- `idempotency.md` — keys, retries, deduplication
- `error-format.md` — RFC 7807 templates
- `status-codes.md` — the actually-correct status code reference
- `openapi-conventions.md` — schema-first vs code-first

## Install

### Cursor (`.cursor/rules/`)

```mdc
---
description: Design HTTP+JSON APIs that are versioned, paginated, idempotent, and follow RFC 7807.
globs: ["src/api/**/*", "src/routes/**/*", "src/controllers/**/*", "**/openapi.yaml"]
alwaysApply: false
---
```

### Kiro (`.kiro/steering/api-design.md`)

Add as steering. Kiro is particularly good at applying API conventions consistently across endpoints.

### Windsurf (`.windsurf/rules/api-design.md`)

Drop in as a rule.

### Claude Code (`.claude/skills/api-design/SKILL.md`)

```yaml
---
name: api-design
description: Design or review an HTTP/JSON API endpoint. Use when adding routes, designing schemas, or writing OpenAPI specs.
---
```

## Usage tips

- **Tell the agent your framework** (FastAPI, Express, Hono, Spring) so it generates idiomatic code, not generic pseudo-code.
- **Pair with `clean-architecture-enforcer`** so endpoints stay thin and delegate to use cases.
- **Generate the OpenAPI spec first**, then the implementation. The agent is better at scaffolding from a spec than retrofitting one.
- **Don't skip idempotency** for payment, signup, or any externally-triggered mutation. The agent will push for it — let it.
- **Use this pack on existing APIs too**: ask it to "audit this endpoint against the design rules" for a quick review.

## Compatibility

| IDE / Agent | Status |
|---|---|
| Cursor | ✅ Recommended |
| Kiro | ✅ Recommended |
| Windsurf | ✅ Supported |
| Claude Code | ✅ Supported |
| Copilot | ✅ Supported |
| Continue.dev | ✅ Supported |

## License

MIT — © ModelBound.
