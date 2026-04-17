<hr>
<p>name: deprecation-and-migration
<br>description: Code-as-liability mindset for removing old systems and migrating users. Use when removing old systems, migrating users, or sunsetting features.
</p>
<hr>
<h1>Deprecation and Migration</h1>
<h2>Overview</h2>
<p>Code is a liability, not an asset. Every line of code you maintain has a cost. Deprecation is the discipline of removing that cost safely — with a clear migration path, no broken callers, and no zombie code left behind.
</p>
<h2>When to Use</h2>
<ul><li>Removing an old API or feature</li>
<li>Migrating users from one system to another</li>
<li>Sunsetting a service or dependency</li>
<li>Cleaning up after a migration is complete</li>
</ul>
<h2>Code-as-Liability Mindset</h2>
<p>More code = more maintenance burden. The best code is code you don't have to maintain. Before adding anything, ask: "Is there existing code that already does this?" Before keeping anything, ask: "Is this still earning its maintenance cost?"
</p>
<h2>Deprecation Types</h2>
<strong>Compulsory deprecation:</strong> Callers must migrate by a deadline. Use when:
<ul><li>The old behavior is insecure</li>
<li>The old behavior is incorrect</li>
<li>The infrastructure cost is too high</li>
</ul>
<strong>Advisory deprecation:</strong> Callers should migrate but aren't forced to. Use when:
<ul><li>The old behavior still works but a better alternative exists</li>
<li>Migration is complex and callers need time</li>
</ul>
<h2>The Migration Pattern</h2>
<pre><code>Phase 1: Add new behavior alongside old
<p>→ Both old and new work simultaneously
</p>
<p>Phase 2: Notify callers
<br>→ Deprecation warnings in logs
<br>→ Documentation updated
<br>→ Migration guide published
</p>
<p>Phase 3: Migrate callers
<br>→ Update all internal callers first
<br>→ Help external callers migrate
<br>→ Set a deadline for compulsory deprecations
</p>
<p>Phase 4: Remove old behavior
<br>→ Only after all callers are migrated
<br>→ Remove the code, not just the documentation</code></pre>
</p>
<p>Never skip Phase 4. Deprecated code that's never removed becomes zombie code.
</p>
<h2>Zombie Code</h2>
<p>Zombie code is code that's "deprecated" but never removed. It:
</p>
<ul><li>Confuses future engineers about what's current</li>
<li>Adds maintenance burden with no benefit</li>
<li>Creates security risks if it's still reachable</li>
</ul>
<strong>Rule:</strong> If you deprecate it, schedule its removal. Set a date. Put it in the task tracker.
<h2>Database Migrations</h2>
<p>Database migrations need special care:
</p>
<ul><li>Always write a rollback migration alongside the forward migration</li>
<li>Test the rollback before deploying</li>
<li>For large tables, use online schema changes to avoid locking</li>
<li>Never drop a column in the same migration that removes the code using it — separate them by at least one deployment</li>
</ul>
<h2>Verification</h2>
<ul><li>[ ] Migration guide written before deprecation announced</li>
<li>[ ] All internal callers migrated before external deadline</li>
<li>[ ] Deprecation warnings in place for advisory deprecations</li>
<li>[ ] Removal date scheduled and tracked</li>
<li>[ ] Old code actually removed (not just commented out)</li>
<li>[ ] Database rollback migrations tested</li>
</ul>