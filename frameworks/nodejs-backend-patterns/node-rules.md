# Node.js Backend Rules

## Project Layout

```
src/
  config.ts            ← validated env config
  bootstrap.ts         ← composition root
  server.ts            ← http server, graceful shutdown
  logger.ts            ← pino instance
  context.ts           ← AsyncLocalStorage for request context
  routes/
  services/
  repositories/
  clients/             ← outbound HTTP / queue clients
  middleware/
```

## Errors

- Throw `Error` subclasses with stable `name`, never plain strings.
- Distinguish operational errors (expected) from programmer errors (bugs).
- Operational errors → return appropriate status to client, log at `warn`.
- Programmer errors → log at `error`, alert, do not leak details to client.
- Never `catch (e) {}` silently. Either log or rethrow.

## Async

- Always `await` promises or attach `.catch()`. Floating promises are bugs.
- No top-level `await` in library files (breaks tooling); only in entry points.
- Use `Promise.allSettled` when partial failure is acceptable.
- Use `AbortController` for cancellable operations.

## HTTP Clients

- Explicit timeout on every outbound request (10s default for user-facing paths).
- Retry only for idempotent requests, with backoff and jitter.
- Cap retries (3 is usually plenty).
- Set a User-Agent that identifies the service and version.

## Database

- One pool per process, lifetime = process lifetime.
- Use parameterized queries (`$1`, `$2`) — never string concat.
- Wrap multi-statement operations in transactions.
- Set `statement_timeout` per session, not per query.

## Logging

- pino with JSON output in production, pretty in dev.
- Bind `request_id`, `user_id`, `team_id` to a child logger at request entry.
- Log structured fields, not interpolated strings: `log.info({ userId, action }, "user action")`.

## Security

- Validate every external input (request body, query, headers) with zod.
- Helmet for HTTP headers (CSP, HSTS, etc).
- Rate limit per principal at the edge.
- CSRF protection on cookie-authenticated routes.
- Never trust `X-Forwarded-For` unless behind a known proxy.

## Process Management

- Run with a process manager that restarts on crash (Kubernetes, systemd, pm2).
- Do not implement your own restart logic.
- Use `--max-old-space-size` to bound heap; size based on container memory.
- One Node process per CPU is a starting point, not a rule.
