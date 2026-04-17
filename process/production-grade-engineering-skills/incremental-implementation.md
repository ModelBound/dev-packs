<hr>
<p>name: incremental-implementation
<br>description: Delivers changes incrementally. Use when implementing any feature or change that touches more than one file. Use when you're about to write a large amount of code at once.
</p>
<hr>
<h1>Incremental Implementation</h1>
<h2>Overview</h2>
<p>Build in thin vertical slices — implement one piece, test it, verify it, then expand. Each increment should leave the system in a working, testable state.
</p>
<h2>When to Use</h2>
<ul><li>Implementing any multi-file change</li>
<li>Building a new feature from a task breakdown</li>
<li>Refactoring existing code</li>
<li>Any time you're tempted to write more than ~100 lines before testing</li>
</ul>
<h2>The Increment Cycle</h2>
<pre><code>Implement ──→ Test ──→ Verify ──→ Commit ──→ Next slice</code></pre>
<p>For each slice:
</p>
<li>Implement the smallest complete piece of functionality</li>
<li>Test — run the test suite</li>
<li>Verify — confirm the slice works (tests pass, build succeeds)</li>
<li>Commit — save progress with a descriptive message</li>
<li>Move to the next slice</li>
<h2>Slicing Strategies</h2>
<strong>Vertical slices (preferred):</strong> Build one complete path through the stack per slice. Each slice delivers working end-to-end functionality.
<strong>Contract-first:</strong> Define the API contract first, then implement backend and frontend in parallel against it.
<strong>Risk-first:</strong> Tackle the riskiest piece first. If it fails, you discover it before investing in the rest.
<h2>Implementation Rules</h2>
<strong>Rule 0 — Simplicity first:</strong> Ask "What is the simplest thing that could work?" before writing any code. Implement the naive, obviously-correct version first. Optimize only after correctness is proven with tests.
<strong>Rule 0.5 — Scope discipline:</strong> Touch only what the task requires. If you notice something worth improving outside your task scope, note it — don't fix it.
<strong>Rule 1 — One thing at a time:</strong> Each increment changes one logical thing. Don't mix concerns.
<strong>Rule 2 — Keep it compilable:</strong> After each increment, the project must build and existing tests must pass.
<strong>Rule 3 — Feature flags for incomplete features:</strong> If a feature isn't ready for users but you need to merge increments, gate it behind a flag.
<strong>Rule 4 — Safe defaults:</strong> New code should default to safe, conservative behavior.
<strong>Rule 5 — Rollback-friendly:</strong> Each increment should be independently revertable.
<h2>Increment Checklist</h2>
<ul><li>[ ] The change does one thing and does it completely</li>
<li>[ ] All existing tests still pass (<code>npm test</code>)</li>
<li>[ ] The build succeeds (<code>npm run build</code>)</li>
<li>[ ] Type checking passes (<code>npx tsc --noEmit</code>)</li>
<li>[ ] Linting passes (<code>npm run lint</code>)</li>
<li>[ ] The new functionality works as expected</li>
<li>[ ] The change is committed with a descriptive message</li>
</ul>
<h2>Common Rationalizations</h2>
<p>| Rationalization | Reality |
<br>|---|---|
<br>| "I'll test it all at the end" | Bugs compound. A bug in Slice 1 makes Slices 2-5 wrong. |
<br>| "It's faster to do it all at once" | It feels faster until something breaks and you can't find which of 500 changed lines caused it. |
<br>| "These changes are too small to commit separately" | Small commits are free. Large commits hide bugs and make rollbacks painful. |
</p>
<h2>Red Flags</h2>
<ul><li>More than 100 lines of code written without running tests</li>
<li>Multiple unrelated changes in a single increment</li>
<li>"Let me just quickly add this too" scope expansion</li>
<li>Build or tests broken between increments</li>
<li>Touching files outside the task scope "while I'm here"</li>
</ul>