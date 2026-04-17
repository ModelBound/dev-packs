# Architecture Boundary Rules

These rules are enforced on every PR. Use a tool like `dependency-cruiser` or `madge` to automate.

## Allowed Imports

| From layer | May import from |
|---|---|
| `domain/*` | `domain/*` only |
| `application/*` | `domain/*`, `application/*` |
| `infrastructure/*` | `domain/*`, `application/*`, `infrastructure/*` |
| `interface/*` | `application/*`, `infrastructure/*` (for DI), `domain/*` (for types only) |

## Forbidden Patterns

- `domain/` importing any framework (express, fastify, react, prisma, sequelize, axios, fetch)
- `application/` importing concrete adapters — only the port interfaces
- Circular imports between layers
- "Anemic domain" — entities with only getters/setters and no behavior

## File Layout

```
src/
  domain/
    invoice/
      Invoice.ts          ← entity
      InvoiceId.ts        ← value object
      InvoiceLine.ts
  application/
    invoice/
      CreateInvoice.ts    ← use case
      ports/
        InvoiceRepository.ts   ← interface only
        Clock.ts
  infrastructure/
    persistence/
      PostgresInvoiceRepository.ts   ← implements port
    http/
      StripeClient.ts
  interface/
    http/
      InvoiceController.ts
```

## Composition Root

Wiring lives in exactly one place per process:

```ts
// src/bootstrap.ts
const clock = new SystemClock();
const repo = new PostgresInvoiceRepository(db);
const createInvoice = new CreateInvoice(repo, clock);
const controller = new InvoiceController(createInvoice);
```

Nothing else may construct adapters.
