<hr>
<p>name: shipping-and-launch
<br>description: Pre-launch checklists, staged rollouts, and rollback procedures. Use when preparing to deploy to production.
</p>
<hr>
<h1>Shipping and Launch</h1>
<h2>Overview</h2>
<p>Shipping is a skill. Production deployments should be boring — well-rehearsed, observable, and reversible. The goal is to make every deployment a non-event.
</p>
<h2>When to Use</h2>
<ul><li>Preparing to deploy to production</li>
<li>Launching a new feature</li>
<li>Rolling back a failed deployment</li>
</ul>
<h2>Pre-Launch Checklist</h2>
<strong>Code quality:</strong>
<ul><li>[ ] All tests pass</li>
<li>[ ] Build succeeds</li>
<li>[ ] No known critical bugs</li>
<li>[ ] Code reviewed and approved</li>
</ul>
<strong>Security:</strong>
<ul><li>[ ] No secrets in code or config</li>
<li>[ ] Auth and authorization verified</li>
<li>[ ] Input validation in place</li>
</ul>
<strong>Observability:</strong>
<ul><li>[ ] Logging in place for key operations</li>
<li>[ ] Error tracking configured</li>
<li>[ ] Performance monitoring active</li>
<li>[ ] Alerts configured for critical metrics</li>
</ul>
<strong>Rollback:</strong>
<ul><li>[ ] Rollback procedure documented</li>
<li>[ ] Database migrations are reversible</li>
<li>[ ] Feature flag can disable the feature instantly</li>
</ul>
<strong>Communication:</strong>
<ul><li>[ ] Stakeholders notified of deployment window</li>
<li>[ ] On-call engineer identified</li>
</ul>
<h2>Feature Flag Lifecycle</h2>
<pre><code>1. DEVELOP: Feature behind flag, flag off by default
<li>TEST: Enable flag in staging, verify behavior</li>
<li>CANARY: Enable for 1-5% of production traffic</li>
<li>RAMP: Gradually increase to 25%, 50%, 100%</li>
<li>CLEANUP: Remove flag after full rollout and stability confirmed</code></pre></li>
<p>Never leave feature flags in the codebase indefinitely — they become technical debt.
</p>
<h2>Staged Rollouts</h2>
<p>Don't deploy to 100% of users at once:
</p>
<pre><code>Internal users → 1% → 10% → 50% → 100%
<p>↑              ↑      ↑      ↑
<br>Catch obvious  Catch   Catch  Full
<br>issues        edge    scale  rollout
<br>cases   issues</code></pre>
</p>
<p>Monitor key metrics at each stage before proceeding.
</p>
<h2>Rollback Procedure</h2>
<p>When something goes wrong:
</p>
<li><strong>Detect:</strong> Monitoring alerts or user reports</li>
<li><strong>Decide:</strong> Is this a rollback situation? (data corruption, security issue, widespread failure = yes)</li>
<li><strong>Execute:</strong> Disable feature flag or revert deployment</li>
<li><strong>Communicate:</strong> Notify stakeholders immediately</li>
<li><strong>Post-mortem:</strong> Document what happened and how to prevent it</li>
<h2>Verification</h2>
<ul><li>[ ] Pre-launch checklist complete</li>
<li>[ ] Rollback procedure tested</li>
<li>[ ] Monitoring and alerts active</li>
<li>[ ] Staged rollout plan in place for significant changes</li>
</ul>