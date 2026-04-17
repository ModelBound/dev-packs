# Node.js Backend Patterns

> Production patterns: graceful shutdown, structured logging with request context, validated config, and 12-factor adherence.

**Pack slug:** `nodejs-backend-patterns`
**Version:** 1.0.0
**Author:** ModelBound (Official)

---

## What it does

Configures your AI agent to write Node.js backends that don't fall over the first time they're deployed to a real environment:

- **Graceful shutdown** — handle `SIGTERM`/`SIGINT`, drain connections, close DB pools, exit cleanly under a timeout
- **Structured logging** with request-scoped context (request ID, user ID, route) using `pino` or `winston` — no `console.log` in production paths
- **Validated config at boot** — `zod`/`envalid` schemas, fail fast on missing or malformed env vars
- **12-factor adherence** — config in env, logs to stdout, stateless processes, no local file state for app data
- **Request ID propagation** from inbound headers through downstream calls (`traceparent`, `x-request-id`)
- **Async error handling** — no unhandled `Promise` rejections, proper try/catch in `async` middlewares
- **Health and readiness endpoints** that mean different things (`/healthz` is "process alive", `/readyz` is "can serve traffic")
- **Connection pooling** — DB, HTTP clients with `keep-alive`, pool sizing tuned to env
- **No top-level await for I/O** in entrypoints — controlled startup order
- **Memory & event-loop awareness** — no synchronous CPU work blocking the loop

## Who it's for

- **Backend teams** running Node in production (Express, Fastify, Hono, NestJS, Koa)
- **Platform engineers** standardizing service templates across teams
- **Solo developers** shipping their first Node API to a real cloud environment
- **Anyone debugging** "why does my container restart every 60 seconds in Kubernetes" (hint: probes)

## What's inside

- `graceful-shutdown.md` — signal handling, drain patterns, timeouts
- `structured-logging.md` — `pino` setup, request context, log levels
- `config-validation.md` — `zod`/`envalid` boot-time checks
- `12-factor-checklist.md` — every factor with Node-specific guidance
- `health-endpoints.md` — liveness vs readiness vs startup probes
- `error-handling.md` — async, middleware, uncaught
- `connection-pooling.md` — DB, HTTP, Redis tuning
- `observability.md` — OpenTelemetry-ready patterns

## Install

### Cursor (`.cursor/rules/`)

```mdc
---
description: Node.js production backend patterns — graceful shutdown, structured logging, validated config, 12-factor.
globs: ["src/**/*.ts", "src/**/*.js", "server.ts", "index.ts", "app.ts"]
alwaysApply: false
---
```

### Kiro (`.kiro/steering/nodejs-backend.md`)

Add as steering.

### Windsurf (`.windsurf/rules/nodejs-backend.md`)

Drop in as a rule.

### Claude Code (`.claude/skills/nodejs-backend-patterns/SKILL.md`)

```yaml
---
name: nodejs-backend-patterns
description: Write or review production Node.js backend code. Use for server entrypoints, middleware, config, logging, and shutdown logic.
---
```

## Usage tips

- **Tell the agent your framework** (Express / Fastify / Hono / NestJS). Patterns are similar but APIs differ.
- **Pair with `clean-architecture-enforcer`** for the layering and `api-design` for the endpoint design.
- **Pair with `sql-migration-reviewer`** if you own the DB.
- **For serverless** (Lambda, Cloud Run, Vercel), graceful shutdown looks different — note that in your project instructions and the agent will adapt.
- **Add an OpenTelemetry SDK** early. The pack pushes for trace context propagation; it's much harder to retrofit.

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
