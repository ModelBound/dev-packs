<hr>
<p>name: performance-optimization
<br>description: Measure-first performance optimization. Use when performance requirements exist or you suspect regressions.
</p>
<hr>
<h1>Performance Optimization</h1>
<h2>Overview</h2>
<p>Measure before optimizing. Never optimize based on intuition — profile first, then fix the actual bottleneck. Premature optimization is the root of much unnecessary complexity.
</p>
<h2>When to Use</h2>
<ul><li>Performance requirements exist (e.g., LCP < 2.5s)</li>
<li>A performance regression is suspected</li>
<li>Before shipping a feature that processes large datasets</li>
<li>After profiling reveals a bottleneck</li>
</ul>
<h2>Core Web Vitals Targets</h2>
<p>| Metric | Good | Needs Work | Poor |
<br>|--------|------|------------|------|
<br>| LCP (Largest Contentful Paint) | < 2.5s | 2.5–4s | > 4s |
<br>| INP (Interaction to Next Paint) | < 200ms | 200–500ms | > 500ms |
<br>| CLS (Cumulative Layout Shift) | < 0.1 | 0.1–0.25 | > 0.25 |
</p>
<h2>The Measure-First Workflow</h2>
<pre><code>1. ESTABLISH BASELINE — measure current performance
<li>IDENTIFY BOTTLENECK — profile to find the actual slow part</li>
<li>FORM HYPOTHESIS — what change will improve it?</li>
<li>IMPLEMENT — make the smallest change that tests the hypothesis</li>
<li>MEASURE AGAIN — did it improve? By how much?</li>
<li>REPEAT or STOP — continue if more improvement needed</code></pre></li>
<p>Never skip step 1 and 2. Optimizing the wrong thing wastes time and adds complexity.
</p>
<h2>Common Backend Bottlenecks</h2>
<strong>N+1 queries:</strong> Loading a list, then querying for each item individually.
<pre><code>// Bad: N+1
<p>const tasks = await db.tasks.findAll();
<br>for (const task of tasks) {
<br>task.assignee = await db.users.findById(task.assigneeId); // N queries
<br>}
</p>
<p>// Good: Single query with join
<br>const tasks = await db.tasks.findAll({ include: &#39;assignee&#39; });</code></pre>
</p>
<strong>Missing indexes:</strong> Queries that scan entire tables.
<ul><li>Add indexes on columns used in WHERE, JOIN, and ORDER BY clauses</li>
<li>Check query plans with EXPLAIN ANALYZE</li>
</ul>
<strong>Unbounded queries:</strong> No LIMIT on list endpoints.
<ul><li>Always paginate list endpoints</li>
<li>Set maximum page sizes</li>
</ul>
<h2>Common Frontend Bottlenecks</h2>
<strong>Unnecessary re-renders:</strong> Components re-rendering when their data hasn't changed.
<ul><li>Use React.memo, useMemo, useCallback appropriately</li>
<li>Profile with React DevTools before adding memoization</li>
</ul>
<strong>Large bundles:</strong> Shipping code the user doesn't need.
<ul><li>Code-split at route boundaries</li>
<li>Lazy-load heavy components</li>
<li>Analyze bundle with <code>npm run build -- --analyze</code></li>
</ul>
<strong>Render-blocking resources:</strong> Scripts and stylesheets that delay page load.
<ul><li>Defer non-critical scripts</li>
<li>Inline critical CSS</li>
<li>Preload key resources</li>
</ul>
<h2>Anti-Patterns</h2>
<ul><li>Optimizing without measuring first</li>
<li>Adding caching before understanding the bottleneck</li>
<li>Micro-optimizing hot paths that aren't actually hot</li>
<li>Removing readability for marginal performance gains</li>
</ul>
<h2>Verification</h2>
<ul><li>[ ] Baseline measured before optimization</li>
<li>[ ] Bottleneck identified via profiling (not intuition)</li>
<li>[ ] Performance improvement measured after change</li>
<li>[ ] No regression in other metrics</li>
<li>[ ] Code is still readable and maintainable</li>
</ul>