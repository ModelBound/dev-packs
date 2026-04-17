<hr>
<p>name: ci-cd-and-automation
<br>description: Shift Left CI/CD with quality gate pipelines. Use when setting up or modifying build and deploy pipelines.
</p>
<hr>
<h1>CI/CD and Automation</h1>
<h2>Overview</h2>
<p>Shift Left — catch problems as early as possible in the development cycle. Faster feedback loops mean faster fixes. Every pipeline stage is a quality gate.
</p>
<h2>When to Use</h2>
<ul><li>Setting up a new CI/CD pipeline</li>
<li>Adding a new quality check</li>
<li>Modifying deployment configuration</li>
<li>Debugging a failing pipeline</li>
</ul>
<h2>Shift Left Principle</h2>
<p>Move quality checks earlier in the process:
</p>
<pre><code>Developer machine → PR → CI → Staging → Production
<p>↑                ↑    ↑      ↑           ↑
<br>Fastest fix    Fast fix  Fix  Slow fix  Expensive fix</code></pre>
</p>
<p>The earlier a problem is caught, the cheaper it is to fix. Run as many checks as possible locally before pushing.
</p>
<h2>Pipeline Structure</h2>
<pre><code># Quality gate pipeline
<p>stages:
<br>- lint          # Fast: &lt; 30s
<br>- type-check    # Fast: &lt; 60s
<br>- unit-tests    # Fast: &lt; 2min
<br>- build         # Medium: &lt; 5min
<br>- integration   # Medium: &lt; 10min
<br>- e2e           # Slow: &lt; 20min (run on main only)
<br>- deploy        # Gated: requires all above to pass</code></pre>
</p>
<p>Fail fast — run the fastest checks first.
</p>
<h2>Feature Flags</h2>
<p>Use feature flags to decouple deployment from release:
</p>
<pre><code>const ENABLE_NEW_DASHBOARD = process.env.FEATURE_NEW_DASHBOARD === &#39;true&#39;;</code></pre>
<p>Benefits:
</p>
<ul><li>Merge incomplete features to main without exposing them</li>
<li>Roll out to a percentage of users</li>
<li>Instant rollback without a deployment</li>
</ul>
<h2>Faster is Safer</h2>
<p>Counterintuitively, deploying more frequently is safer than deploying less frequently:
</p>
<ul><li>Smaller changes are easier to understand and revert</li>
<li>Problems are caught sooner with less blast radius</li>
<li>Rollback is faster when the diff is small</li>
</ul>
<h2>Failure Feedback Loops</h2>
<p>When a pipeline fails:
</p>
<li>The failure notification must be immediate and specific</li>
<li>The error message must point to the exact failure</li>
<li>The fix must be deployable within minutes</li>
<p>A pipeline that takes 30 minutes to fail is a pipeline that slows the team.
</p>
<h2>Verification</h2>
<ul><li>[ ] Pipeline runs on every PR</li>
<li>[ ] Fastest checks run first</li>
<li>[ ] All quality gates must pass before deploy</li>
<li>[ ] Failure notifications are immediate and specific</li>
<li>[ ] Deployment is automated (no manual steps)</li>
<li>[ ] Rollback procedure is documented and tested</li>
</ul>