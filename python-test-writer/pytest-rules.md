# Pytest Rules

## Layout

- Tests live under `tests/` mirroring `src/` structure.
- Test file: `test_<module>.py`.
- Test class (optional): `Test<FunctionOrClass>`.
- Test function: `test_<expected_behavior>_when_<condition>`.

## Conftest

- Project-wide fixtures live in `tests/conftest.py`.
- Subpackage fixtures live in `tests/<subpackage>/conftest.py`.
- Never import fixtures explicitly — pytest discovers them.

## Fixtures

- Default scope: `function`. Use `session` only for expensive setup with no shared state.
- Use `yield` fixtures for setup + teardown.
- Factory fixtures return a callable when tests need varied data:
  ```python
  @pytest.fixture
  def make_user():
      def _make(**overrides):
          return User(id=uuid4(), email="test@example.com", **overrides)
      return _make
  ```

## Parametrize

- Use `@pytest.mark.parametrize` for table-driven tests.
- Give parameters `ids=` for readable failure output:
  ```python
  @pytest.mark.parametrize(
      "input,expected",
      [(0, "zero"), (1, "one"), (-1, "negative")],
      ids=["zero", "positive", "negative"],
  )
  ```

## Mocks

- Use `pytest-mock` (`mocker` fixture), not bare `unittest.mock`.
- Mock at the boundary, not the implementation: mock `requests.get`, not the function that calls it (when reasonable).
- Always assert the mock was called as expected.

## Time and Randomness

- Use `freezegun` or inject a clock fixture for time.
- Seed randomness in conftest: `random.seed(0)`.
- Never test against `datetime.now()` directly.

## Database

- Use `pytest-postgresql` or `testcontainers` for real Postgres.
- Wrap each test in a transaction that is rolled back at teardown.
- Never share state between tests.

## Coverage

- `pytest --cov=src --cov-fail-under=80` in CI.
- Branch coverage, not just line.
- Exclude `__main__`, generated code, and dataclasses-only files.
