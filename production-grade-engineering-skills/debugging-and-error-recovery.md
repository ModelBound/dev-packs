<hr>
<p>name: debugging-and-error-recovery
<br>description: Five-step triage for bugs and failures. Use when tests fail, builds break, or behavior is unexpected.
</p>
<hr>
<h1>Debugging and Error Recovery</h1>
<h2>Overview</h2>
<p>Systematic five-step triage: reproduce, localize, reduce, fix, guard. Stop-the-line rule — don't proceed past a broken state. Safe fallbacks over heroic fixes.
</p>
<h2>When to Use</h2>
<ul><li>Tests fail</li>
<li>Build breaks</li>
<li>Unexpected runtime behavior</li>
<li>Error reports from production</li>
</ul>
<h2>The Five-Step Process</h2>
<h3>Step 1: Reproduce</h3>
<p>Before doing anything else, reproduce the failure reliably:
</p>
<ul><li>What exact inputs trigger it?</li>
<li>Does it happen every time or intermittently?</li>
<li>What environment does it occur in?</li>
</ul>
<p>A bug you can't reproduce reliably is a bug you can't fix reliably.
</p>
<h3>Step 2: Localize</h3>
<p>Narrow down where the failure occurs:
</p>
<ul><li>Which component, function, or line?</li>
<li>What's the smallest code path that triggers it?</li>
<li>What changed recently? (<code>git log --oneline -20</code>)</li>
</ul>
<h3>Step 3: Reduce</h3>
<p>Create the minimal reproduction case:
</p>
<ul><li>Strip away everything not needed to trigger the bug</li>
<li>The smaller the reproduction, the clearer the fix</li>
</ul>
<h3>Step 4: Fix</h3>
<p>With a clear reproduction and localized cause:
</p>
<ul><li>Fix the root cause, not the symptom</li>
<li>Don't add workarounds that mask the underlying issue</li>
<li>Prefer the simplest fix that addresses the root cause</li>
</ul>
<h3>Step 5: Guard</h3>
<p>Prevent regression:
</p>
<ul><li>Write a test that would have caught this bug</li>
<li>The test must fail before the fix and pass after</li>
<li>Run the full test suite to confirm no regressions</li>
</ul>
<h2>The Stop-the-Line Rule</h2>
<p>If the build is broken or tests are failing, <strong>stop all other work</strong> until it's fixed. A broken build is a team-wide blocker. Don't add new code on top of a broken foundation.
</p>
<h2>Safe Fallbacks</h2>
<p>When a fix isn't immediately clear:
</p>
<ul><li>Revert to the last known good state</li>
<li>Use a feature flag to disable the broken behavior</li>
<li>Add a safe default that degrades gracefully</li>
</ul>
<p>Never leave the system in a broken state to "fix it later."
</p>
<h2>Common Rationalizations</h2>
<p>| Rationalization | Reality |
<br>|---|---|
<br>| "I'll just try things until it works" | Random changes without understanding the cause create new bugs. |
<br>| "It works on my machine" | Environment differences are a cause, not an excuse. Reproduce it, then fix the environment difference. |
<br>| "I'll add a workaround for now" | Workarounds become permanent. Fix the root cause. |
</p>
<h2>Red Flags</h2>
<ul><li>Fixing symptoms without understanding the cause</li>
<li>Skipping the reproduction step</li>
<li>No regression test after a bug fix</li>
<li>Continuing to add features while the build is broken</li>
<li>"It seems to work now" without a clear explanation of why</li>
</ul>
<h2>Verification</h2>
<ul><li>[ ] Bug is reproducible before the fix</li>
<li>[ ] Root cause is identified and understood</li>
<li>[ ] Fix addresses the root cause, not the symptom</li>
<li>[ ] Regression test added that fails before fix and passes after</li>
<li>[ ] Full test suite passes</li>
</ul>