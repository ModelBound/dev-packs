# API Design Rules

## Routes

- Plural nouns: `/v1/orders`, not `/v1/order` or `/v1/getOrder`.
- Verbs only for actions that are not CRUD: `POST /v1/invoices/{id}/void`.
- No file extensions in URLs. Use the `Accept` header.

## Methods

| Method | Semantics | Idempotent | Safe |
|---|---|---|---|
| GET | Read | yes | yes |
| POST | Create or non-idempotent action | no (use Idempotency-Key) | no |
| PUT | Replace entire resource | yes | no |
| PATCH | Partial update | yes if field-level | no |
| DELETE | Remove | yes | no |

## Status Codes

- 200 OK — successful read or update with body
- 201 Created — successful create, return the new resource and a `Location` header
- 202 Accepted — async accepted, return job id
- 204 No Content — successful delete or update with no body
- 400 Bad Request — malformed input
- 401 Unauthorized — missing or invalid credentials
- 403 Forbidden — authenticated but not authorized
- 404 Not Found — resource does not exist
- 409 Conflict — state conflict (e.g. version mismatch)
- 422 Unprocessable Entity — well-formed but semantically invalid
- 429 Too Many Requests — rate limited, include `Retry-After`
- 500 Internal — unexpected server bug

## Headers

- Required: `Content-Type: application/json`, `Accept: application/json`.
- Auth: `Authorization: Bearer <token>`.
- Idempotency: `Idempotency-Key: <uuid>`.
- Tracing: propagate `Traceparent` per W3C Trace Context.
- Rate limit: respond with `X-RateLimit-Limit`, `X-RateLimit-Remaining`, `X-RateLimit-Reset`.

## Field Naming

- `snake_case` in JSON bodies.
- Timestamps in RFC 3339 / ISO 8601 with timezone: `"2024-01-15T10:30:00Z"`.
- Money as `{ "amount": 1299, "currency": "USD" }` — integer minor units.
- Booleans named as positives: `is_active`, not `is_inactive`.

## Backwards Compatibility

Allowed in a stable version:
- Adding new optional request fields
- Adding new response fields
- Adding new endpoints
- Adding new enum values (clients must accept unknown values)

Forbidden in a stable version:
- Removing or renaming any field
- Changing field types
- Changing required-ness of any field
- Changing status code returned for an existing case
