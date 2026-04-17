# Python Test Writer

> Generates pytest suites with realistic fixtures, parametrize, and meaningful edge cases. No `assert True` filler.

**Pack slug:** `python-test-writer`
**Version:** 1.0.0
**Author:** ModelBound (Official)

---

## What it does

Configures your AI agent to write tests that would survive a senior engineer's review:

- Uses `pytest` idioms (fixtures, `parametrize`, `monkeypatch`, `tmp_path`)
- Names tests after behavior (`test_returns_empty_list_when_user_has_no_orders`)
- Covers the happy path, the boundary, and the error path — not just the happy path
- Builds realistic fixture data via `factory_boy` or `pytest-factoryboy` when available
- Mocks at the right seam (HTTP boundary, not internal functions)
- Refuses placeholder assertions (`assert True`, `assert result is not None` alone)
- Adds `@pytest.mark.parametrize` for combinatorial coverage

## Who it's for

- **Python teams** with low test coverage and an AI agent willing to write tests
- **TDD practitioners** who want their agent to write the failing test first
- **API/data engineers** generating regression suites for legacy code
- **Anyone tired of** "I added tests" PRs full of mocks that test nothing

## What's inside

- `pytest-conventions.md` — naming, structure, markers
- `fixture-design.md` — when to use `@pytest.fixture`, scopes, factories
- `parametrize-patterns.md` — table-driven tests done right
- `mocking-rules.md` — mock at the boundary, not the internals
- `coverage-goals.md` — what to actually cover (not just %)

## Install

### Cursor (`.cursor/rules/`)

```mdc
---
description: Write pytest tests with realistic fixtures, parametrize, and meaningful assertions.
globs: ["**/*.py", "**/test_*.py", "**/tests/**"]
alwaysApply: false
---
```

### Kiro (`.kiro/steering/python-tests.md`)

Add as steering. Trigger by asking "write tests for this module."

### Windsurf (`.windsurf/rules/python-test-writer.md`)

Drop in as a rule.

### Claude Code (`.claude/skills/python-test-writer/SKILL.md`)

```yaml
---
name: python-test-writer
description: Write pytest tests for Python code. Use when the user asks to add tests, increase coverage, or do TDD on a Python file.
---
```

### GitHub Copilot

Append `pytest-conventions.md` to `.github/copilot-instructions.md`.

## Usage tips

- **Tell the agent which test framework you use** if not pytest (e.g. unittest). The pack assumes pytest.
- **Show it an existing test file** as a style anchor before asking it to write new ones.
- **Pair with `tdd-workflow`** from the Production Engineering Skills pack for a full red→green→refactor loop.
- **Run coverage**: ask the agent to target specific uncovered lines, not "increase coverage."
- **Don't let it mock your own code.** If it's mocking internal functions, the design is probably wrong — refactor first, test after.

## Compatibility

| IDE / Agent | Status |
|---|---|
| Cursor | ✅ Recommended (scope to `*.py`) |
| Kiro | ✅ Recommended |
| Windsurf | ✅ Supported |
| Claude Code | ✅ Supported |
| Copilot | ✅ Supported |
| Continue.dev | ✅ Supported |
| Aider | ✅ Supported |

## License

MIT — © ModelBound.
