<hr>
<p>name: git-workflow-and-versioning
<br>description: Applies trunk-based development and atomic commits. Use when making any code change — always.
</p>
<hr>
<h1>Git Workflow and Versioning</h1>
<h2>Overview</h2>
<p>Trunk-based development with atomic commits. Every commit is a save point — small, focused, and independently revertable. The commit history is documentation.
</p>
<h2>When to Use</h2>
<ul><li>Making any code change (always)</li>
<li>Before merging to main</li>
<li>When setting up a new repository</li>
</ul>
<h2>Core Principles</h2>
<strong>Trunk-based development:</strong> Work in short-lived branches (< 1 day) or directly on main with feature flags. Long-lived branches create merge conflicts and integration debt.
<strong>Atomic commits:</strong> Each commit changes one logical thing. If you can't describe a commit in one sentence, it's too large.
<strong>Commit as save point:</strong> Commit after every working increment — not at the end of the day.
<h2>Commit Message Format</h2>
<pre><code>&lt;type&gt;(&lt;scope&gt;): &lt;short description&gt;
<p>[optional body — what changed and why]
</p>
<p>[optional footer — breaking changes, issue refs]</code></pre>
</p>
<strong>Types:</strong> <code>feat</code>, <code>fix</code>, <code>refactor</code>, <code>test</code>, <code>docs</code>, <code>chore</code>, <code>perf</code>
<strong>Good examples:</strong>
<pre><code>feat(auth): add JWT refresh token rotation
<p>fix(tasks): prevent null pointer when user is undefined
<br>refactor(api): extract validation middleware from route handlers
<br>test(billing): add coverage for failed payment scenarios</code></pre>
</p>
<strong>Bad examples:</strong>
<pre><code>fix bug
<p>update stuff
<br>WIP
<br>changes</code></pre>
</p>
<h2>Change Sizing</h2>
<pre><code>~100 lines changed   → Good. Atomic and reviewable.
<p>~300 lines changed   → Acceptable for a single logical change.
<br>~1000 lines changed  → Too large. Split it.</code></pre>
</p>
<h2>Branch Strategy</h2>
<pre><code>main (always deployable)
<p>│
<br>├── feat/task-sharing (&lt; 1 day, then merge)
<br>├── fix/null-pointer-login (&lt; 1 day, then merge)
<br>└── chore/update-deps (&lt; 1 day, then merge)</code></pre>
</p>
<p>Never let a branch live longer than a day without merging or rebasing.
</p>
<h2>Before Every Commit</h2>
<ul><li>[ ] <code>git diff --staged</code> — review exactly what you're committing</li>
<li>[ ] Tests pass</li>
<li>[ ] Build succeeds</li>
<li>[ ] No secrets or debug code included</li>
<li>[ ] Commit message follows the format</li>
</ul>
<h2>Common Rationalizations</h2>
<p>| Rationalization | Reality |
<br>|---|---|
<br>| "I'll clean up the commits before merging" | You won't. Commit clean from the start. |
<br>| "This is just a WIP commit" | WIP commits in main history make bisecting impossible. |
<br>| "One big commit is easier" | One big commit makes rollbacks, bisecting, and review painful. |
</p>
<h2>Red Flags</h2>
<ul><li>Commits with "fix", "update", or "changes" as the entire message</li>
<li>Commits that mix unrelated changes</li>
<li>Branches older than a day without merging</li>
<li>Uncommitted changes accumulating over multiple sessions</li>
</ul>