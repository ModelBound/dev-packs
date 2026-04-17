<hr>
<p>name: code-simplification
<br>description: Reduces complexity while preserving exact behavior. Use when code works but is harder to read or maintain than it should be.
</p>
<hr>
<h1>Code Simplification</h1>
<h2>Overview</h2>
<p>Reduce complexity while preserving exact behavior. Clarity over cleverness. The best code is code that doesn't need a comment to explain what it does.
</p>
<h2>When to Use</h2>
<ul><li>Code works but is hard to read</li>
<li>A function is doing too many things</li>
<li>Abstractions are adding complexity without adding value</li>
<li>You're about to add to code that's already complex</li>
</ul>
<h2>Chesterton's Fence</h2>
<blockquote><p>Don't remove something until you understand why it was put there.</p></blockquote>
<p>Before simplifying any code:
</p>
<li>Understand why it was written this way</li>
<li>Check git history for context</li>
<li>Look for tests that reveal the intent</li>
<li>Ask if there's a reason for the complexity you're not seeing</li>
<p>Removing code you don't understand creates bugs.
</p>
<h2>The Rule of 500</h2>
<p>A file over 500 lines is a signal to split. A function over 50 lines is a signal to extract. These aren't hard limits — they're prompts to ask "is this too much?"
</p>
<h2>Simplification Patterns</h2>
<strong>Extract function:</strong> When a block of code needs a comment to explain it, extract it into a named function.
<pre><code>// Before: comment needed
<p>// Calculate the discount based on membership tier
<br>const discount = user.tier === &#39;gold&#39; ? 0.2 : user.tier === &#39;silver&#39; ? 0.1 : 0;
</p>
<p>// After: self-documenting
<br>const discount = getMembershipDiscount(user.tier);</code></pre>
</p>
<strong>Flatten nesting:</strong> Deep nesting is hard to follow. Use early returns.
<pre><code>// Before: deeply nested
<p>function processTask(task) {
<br>if (task) {
<br>if (task.status === &#39;pending&#39;) {
<br>if (task.assignee) {
<br>// actual logic
<br>}
<br>}
<br>}
<br>}
</p>
<p>// After: early returns
<br>function processTask(task) {
<br>if (!task) return;
<br>if (task.status !== &#39;pending&#39;) return;
<br>if (!task.assignee) return;
<br>// actual logic
<br>}</code></pre>
</p>
<strong>Remove dead code:</strong> Unused variables, functions, and imports add noise. Remove them.
<strong>Inline unnecessary abstractions:</strong> If an abstraction is only used once and doesn't add clarity, inline it.
<h2>What NOT to Simplify</h2>
<ul><li>Code that handles edge cases you don't fully understand</li>
<li>Performance-critical code where the "clever" version is measurably faster</li>
<li>Code that's complex because the domain is complex (the complexity is real)</li>
</ul>
<h2>Verification</h2>
<ul><li>[ ] All existing tests still pass after simplification</li>
<li>[ ] No behavior has changed — only structure</li>
<li>[ ] The simplified code is actually easier to read</li>
<li>[ ] No dead code remains</li>
<li>[ ] Chesterton's Fence was respected — nothing removed without understanding why it existed</li>
</ul>