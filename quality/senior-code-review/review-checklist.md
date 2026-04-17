# Code Review Checklist

Walk this checklist for every PR. Skip items that do not apply.

## Correctness

- [ ] Logic matches the PR description and linked ticket
- [ ] Edge cases: empty input, max input, null/undefined, very large lists
- [ ] Error paths handled (not just happy path)
- [ ] Concurrent / race conditions considered for shared state
- [ ] No off-by-one errors in loops or pagination

## Security

- [ ] All user input validated at the boundary (zod / pydantic / dto)
- [ ] No secrets, API keys, or tokens in code, logs, or error messages
- [ ] Authorization checked on every endpoint, not just authentication
- [ ] SQL queries parameterized, no string concatenation
- [ ] No `eval`, `Function()`, `exec()` on user input
- [ ] CORS, CSP, and cookie flags appropriate for the endpoint
- [ ] File uploads checked for type, size, and path traversal

## Readability

- [ ] Names describe intent, not implementation
- [ ] Functions do one thing, < 50 lines preferred
- [ ] Comments explain *why*, code shows *what*
- [ ] No commented-out code
- [ ] No magic numbers — extract to named constants
- [ ] Public APIs documented (jsdoc / docstrings)

## Tests

- [ ] New branches and edge cases have tests
- [ ] Tests describe behavior, not implementation
- [ ] No flaky timers / sleeps — use fakes or fixtures
- [ ] Mocks only at external boundaries (network, FS, time)

## Operability

- [ ] Errors logged with enough context to debug from logs alone
- [ ] No new dependencies without justification in PR description
- [ ] Database migrations are reversible
- [ ] Feature flag or rollback plan for risky changes

## Performance (only if hot path)

- [ ] No N+1 database queries
- [ ] No unbounded loops over user-controlled input
- [ ] Caching invalidation considered, not just caching
