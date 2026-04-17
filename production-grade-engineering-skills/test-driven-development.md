<hr>
<p>name: test-driven-development
<br>description: Drives development with tests. Use when implementing any logic, fixing any bug, or changing any behavior. Tests are proof — "seems right" is not done.
</p>
<hr>
<h1>Test-Driven Development</h1>
<h2>Overview</h2>
<p>Write a failing test before writing the code that makes it pass. For bug fixes, reproduce the bug with a test before attempting a fix. A codebase with good tests is an AI agent's superpower; a codebase without tests is a liability.
</p>
<h2>When to Use</h2>
<ul><li>Implementing any new logic or behavior</li>
<li>Fixing any bug (the Prove-It Pattern)</li>
<li>Modifying existing functionality</li>
<li>Any change that could break existing behavior</li>
</ul>
<h2>The TDD Cycle</h2>
<pre><code>    RED                GREEN              REFACTOR
<p>Write a test    Write minimal code    Clean up the
<br>that fails  ──→  to make it pass  ──→  implementation  ──→  (repeat)</code></pre>
</p>
<h3>Step 1: RED — Write a Failing Test</h3>
<p>Write the test first. It must fail. A test that passes immediately proves nothing.
</p>
<h3>Step 2: GREEN — Make It Pass</h3>
<p>Write the minimum code to make the test pass. Don't over-engineer.
</p>
<h3>Step 3: REFACTOR — Clean Up</h3>
<p>With tests green, improve the code without changing behavior. Run tests after every refactor step.
</p>
<h2>The Prove-It Pattern (Bug Fixes)</h2>
<pre><code>Bug report arrives
<p>│
<br>▼
<br>Write a test that demonstrates the bug (must FAIL)
<br>│
<br>▼
<br>Implement the fix
<br>│
<br>▼
<br>Test PASSES → bug fixed, regression guarded
<br>│
<br>▼
<br>Run full test suite (no regressions)</code></pre>
</p>
<h2>The Test Pyramid</h2>
<pre><code>          ╱╲
<p>╱  ╲         E2E Tests (~5%)
<br>╱────╲
<br>╱      ╲       Integration Tests (~15%)
<br>╱────────╲
<br>╱          ╲     Unit Tests (~80%)
<br>╱────────────╲</code></pre>
</p>
<strong>The Beyonce Rule:</strong> If you liked it, you should have put a test on it.
<h2>Test Sizes</h2>
<p>| Size | Constraints | Speed |
<br>|------|------------|-------|
<br>| <strong>Small</strong> | Single process, no I/O, no network | Milliseconds |
<br>| <strong>Medium</strong> | Multi-process OK, localhost only | Seconds |
<br>| <strong>Large</strong> | Multi-machine OK, external services | Minutes |
</p>
<h2>Writing Good Tests</h2>
<ul><li><strong>Test state, not interactions</strong> — assert on outcomes, not which methods were called</li>
<li><strong>DAMP over DRY</strong> — each test should tell a complete story without tracing shared helpers</li>
<li><strong>Prefer real implementations over mocks</strong> — mock only at boundaries where real deps are slow or non-deterministic</li>
<li><strong>Arrange-Act-Assert pattern</strong> — set up, perform, verify</li>
<li><strong>One assertion per concept</strong> — separate tests for separate behaviors</li>
<li><strong>Descriptive names</strong> — test names are specifications</li>
</ul>
<h2>Test Anti-Patterns</h2>
<p>| Anti-Pattern | Fix |
<br>|---|---|
<br>| Testing implementation details | Test inputs and outputs, not internal structure |
<br>| Flaky tests | Use deterministic assertions, isolate test state |
<br>| Snapshot abuse | Use sparingly, review every change |
<br>| No test isolation | Each test sets up and tears down its own state |
<br>| Mocking everything | Prefer real implementations > fakes > stubs > mocks |
</p>
<h2>Common Rationalizations</h2>
<p>| Rationalization | Reality |
<br>|---|---|
<br>| "I'll write tests after the code works" | Tests written after the fact test implementation, not behavior. |
<br>| "This is too simple to test" | Simple code gets complicated. The test documents expected behavior. |
<br>| "Tests slow me down" | They slow you down now. They speed you up every time you change the code later. |
<br>| "I tested it manually" | Manual testing doesn't persist. |
</p>
<h2>Red Flags</h2>
<ul><li>Writing code without any corresponding tests</li>
<li>Tests that pass on the first run without being written to fail first</li>
<li>Bug fixes without reproduction tests</li>
<li>Skipping tests to make the suite pass</li>
</ul>
<h2>Verification</h2>
<ul><li>[ ] Every new behavior has a corresponding test</li>
<li>[ ] All tests pass: <code>npm test</code></li>
<li>[ ] Bug fixes include a reproduction test that failed before the fix</li>
<li>[ ] Test names describe the behavior being verified</li>
<li>[ ] No tests were skipped or disabled</li>
</ul>