<hr>
<p>name: documentation-and-adrs
<br>description: Architecture Decision Records and API documentation. Use when making architectural decisions, changing APIs, or shipping features.
</p>
<hr>
<h1>Documentation and ADRs</h1>
<h2>Overview</h2>
<p>Document the why, not just the what. Code explains what it does — documentation explains why decisions were made. Architecture Decision Records (ADRs) are the most valuable documentation you can write.
</p>
<h2>When to Use</h2>
<ul><li>Making an architectural decision</li>
<li>Changing a public API</li>
<li>Shipping a significant feature</li>
<li>When future engineers will need context to understand a decision</li>
</ul>
<h2>Architecture Decision Records</h2>
<p>An ADR captures a significant architectural decision and its context. Write one whenever you make a decision that would be hard to reverse or that future engineers will wonder about.
</p>
<strong>ADR Template:</strong>
<pre><code># ADR-[number]: [Short title]
<h2>Status</h2>
<p>[Proposed | Accepted | Deprecated | Superseded by ADR-X]
</p>
<h2>Context</h2>
<p>[What situation led to this decision? What forces are at play?]
</p>
<h2>Decision</h2>
<p>[What was decided? State it clearly and directly.]
</p>
<h2>Consequences</h2>
<p>[What are the results of this decision — positive and negative?
<br>What becomes easier? What becomes harder?]
</p>
<h2>Alternatives Considered</h2>
<p>[What other options were evaluated and why were they rejected?]</code></pre>
</p>
<strong>Where to store ADRs:</strong> <code>docs/decisions/</code> or <code>adr/</code> in the repository root.
<h2>API Documentation</h2>
<p>Every public API needs:
</p>
<ul><li><strong>Purpose:</strong> What does this endpoint/function do?</li>
<li><strong>Parameters:</strong> Types, constraints, required vs optional</li>
<li><strong>Return value:</strong> Shape and meaning</li>
<li><strong>Error cases:</strong> What errors can be returned and when</li>
<li><strong>Example:</strong> A working example of the happy path</li>
</ul>
<h2>Inline Documentation</h2>
<p>Comment the why, not the what:
</p>
<pre><code>// Bad: explains what the code does (obvious from reading it)
<p>// Multiply price by quantity
<br>const total = price * quantity;
</p>
<p>// Good: explains why a non-obvious decision was made
<br>// We use floor instead of round to avoid charging customers
<br>// more than the displayed price due to floating point precision
<br>const total = Math.floor(price <em> quantity </em> 100) / 100;</code></pre>
</p>
<h2>What NOT to Document</h2>
<ul><li>Code that's self-explanatory</li>
<li>Implementation details that will change</li>
<li>Obvious things that add noise without adding value</li>
</ul>
<h2>Verification</h2>
<ul><li>[ ] Architectural decisions have ADRs</li>
<li>[ ] Public APIs have documentation</li>
<li>[ ] Comments explain why, not what</li>
<li>[ ] Documentation is committed alongside the code it describes</li>
</ul>