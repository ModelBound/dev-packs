<hr>
<p>name: code-review-and-quality
<br>description: Conducts multi-axis code review. Use before merging any change. Use when reviewing code written by yourself, another agent, or a human.
</p>
<hr>
<h1>Code Review and Quality</h1>
<h2>Overview</h2>
<p>Multi-dimensional code review with quality gates. Every change gets reviewed before merge — no exceptions. Review covers five axes: correctness, readability, architecture, security, and performance.
</p>
<strong>The approval standard:</strong> Approve when a change definitely improves overall code health, even if it isn't perfect. Don't block a change because it isn't exactly how you would have written it.
<h2>The Five-Axis Review</h2>
<h3>1. Correctness</h3>
<ul><li>Does it match the spec or task requirements?</li>
<li>Are edge cases handled (null, empty, boundary values)?</li>
<li>Are error paths handled?</li>
<li>Does it pass all tests?</li>
</ul>
<h3>2. Readability & Simplicity</h3>
<ul><li>Are names descriptive and consistent with project conventions?</li>
<li>Is the control flow straightforward?</li>
<li>Could this be done in fewer lines?</li>
<li>Are abstractions earning their complexity?</li>
</ul>
<h3>3. Architecture</h3>
<ul><li>Does it follow existing patterns?</li>
<li>Does it maintain clean module boundaries?</li>
<li>Is there code duplication that should be shared?</li>
</ul>
<h3>4. Security</h3>
<ul><li>Is user input validated and sanitized?</li>
<li>Are secrets kept out of code, logs, and version control?</li>
<li>Are SQL queries parameterized?</li>
<li>Is data from external sources treated as untrusted?</li>
</ul>
<h3>5. Performance</h3>
<ul><li>Any N+1 query patterns?</li>
<li>Any unbounded loops or unconstrained data fetching?</li>
<li>Any missing pagination on list endpoints?</li>
</ul>
<h2>Change Sizing</h2>
<pre><code>~100 lines changed   → Good. Reviewable in one sitting.
<p>~300 lines changed   → Acceptable if it&#39;s a single logical change.
<br>~1000 lines changed  → Too large. Split it.</code></pre>
</p>
<h2>Severity Labels</h2>
<p>| Prefix | Meaning | Author Action |
<br>|--------|---------|---------------|
<br>| <em>(no prefix)</em> | Required change | Must address before merge |
<br>| <strong>Critical:</strong> | Blocks merge | Security vulnerability, data loss, broken functionality |
<br>| <strong>Nit:</strong> | Minor, optional | Author may ignore |
<br>| <strong>Optional:</strong> | Suggestion | Worth considering but not required |
<br>| <strong>FYI</strong> | Informational | No action needed |
</p>
<h2>Review Process</h2>
<li>Understand the context — what is this change trying to accomplish?</li>
<li>Review the tests first — they reveal intent and coverage</li>
<li>Review the implementation across all five axes</li>
<li>Categorize findings with severity labels</li>
<li>Verify the verification story (what tests were run, build status)</li>
<h2>Common Rationalizations</h2>
<p>| Rationalization | Reality |
<br>|---|---|
<br>| "It works, that's good enough" | Working code that's unreadable or insecure creates debt that compounds. |
<br>| "AI-generated code is probably fine" | AI code needs more scrutiny, not less. It's confident and plausible, even when wrong. |
<br>| "The tests pass, so it's good" | Tests don't catch architecture problems, security issues, or readability concerns. |
</p>
<h2>Red Flags</h2>
<ul><li>PRs merged without any review</li>
<li>"LGTM" without evidence of actual review</li>
<li>Security-sensitive changes without security-focused review</li>
<li>No regression tests with bug fix PRs</li>
<li>Accepting "I'll fix it later" — it never happens</li>
</ul>
<h2>Verification</h2>
<ul><li>[ ] All Critical issues are resolved</li>
<li>[ ] All Important issues are resolved or explicitly deferred</li>
<li>[ ] Tests pass</li>
<li>[ ] Build succeeds</li>
</ul>