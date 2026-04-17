<hr>
<p>name: security-and-hardening
<br>description: Applies security best practices. Use when handling user input, auth, data storage, or external integrations. Use before any code touches a security boundary.
</p>
<hr>
<h1>Security and Hardening</h1>
<h2>Overview</h2>
<p>Systematic security review and hardening. Security is not a phase — it's a continuous practice applied at every boundary where untrusted data enters the system.
</p>
<h2>When to Use</h2>
<ul><li>Handling user input of any kind</li>
<li>Implementing authentication or authorization</li>
<li>Storing or transmitting sensitive data</li>
<li>Integrating with external APIs or services</li>
<li>Before any code ships to production</li>
</ul>
<h2>Three-Tier Boundary System</h2>
<pre><code>Tier 1 — External boundary: Everything from users, APIs, files, env
<p>→ Validate, sanitize, reject invalid input
</p>
<p>Tier 2 — Internal boundary: Between services, modules, functions
<br>→ Verify authorization, check permissions
</p>
<p>Tier 3 — Storage boundary: Database, cache, file system
<br>→ Parameterize queries, encrypt sensitive data</code></pre>
</p>
<h2>OWASP Top 10 Prevention</h2>
<strong>Injection (SQL, NoSQL, Command)</strong>
<ul><li>Always use parameterized queries — never string concatenation</li>
<li>Validate and whitelist input types before use</li>
<li>Use ORMs with built-in parameterization</li>
</ul>
<strong>Broken Authentication</strong>
<ul><li>Use established auth libraries (never roll your own)</li>
<li>Enforce strong password policies</li>
<li>Implement rate limiting on auth endpoints</li>
<li>Use secure, httpOnly, sameSite cookies for sessions</li>
</ul>
<strong>Sensitive Data Exposure</strong>
<ul><li>Never log passwords, tokens, or PII</li>
<li>Encrypt sensitive data at rest</li>
<li>Use HTTPS everywhere — no exceptions</li>
<li>Rotate secrets regularly</li>
</ul>
<strong>Security Misconfiguration</strong>
<ul><li>No default credentials in production</li>
<li>Disable debug mode and stack traces in production</li>
<li>Set security headers (CSP, HSTS, X-Frame-Options)</li>
<li>Principle of least privilege for all service accounts</li>
</ul>
<strong>XSS Prevention</strong>
<ul><li>Encode all output — never trust user content in HTML</li>
<li>Use Content Security Policy headers</li>
<li>Sanitize HTML if rich text is required</li>
</ul>
<strong>Broken Access Control</strong>
<ul><li>Check authorization on every request — not just at login</li>
<li>Deny by default, allow explicitly</li>
<li>Validate that users can only access their own data</li>
</ul>
<h2>Secrets Management</h2>
<pre><code>NEVER:
<ul><li>Hardcode secrets in source code</li>
<li>Commit .env files with real values</li>
<li>Log secrets or tokens</li>
<li>Pass secrets in URLs or query params</li>
</ul>
<p>ALWAYS:
</p>
<ul><li>Use environment variables</li>
<li>Use a secrets manager in production</li>
<li>Rotate secrets on suspected compromise</li>
<li>Audit secret access</code></pre></li>
</ul>
<h2>Pre-Commit Security Checklist</h2>
<ul><li>[ ] No secrets, tokens, or credentials in code</li>
<li>[ ] All user input validated at entry points</li>
<li>[ ] SQL queries parameterized</li>
<li>[ ] Auth checks on all protected endpoints</li>
<li>[ ] Sensitive data not logged</li>
<li>[ ] Dependencies audited (<code>npm audit</code>)</li>
</ul>
<h2>Red Flags</h2>
<ul><li>String concatenation in SQL queries</li>
<li><code>eval()</code> or <code>exec()</code> with user input</li>
<li>Secrets in source code or logs</li>
<li>Missing authorization checks</li>
<li>Trusting client-supplied data without validation</li>
<li>Disabled SSL/TLS verification</li>
</ul>
<h2>Verification</h2>
<ul><li>[ ] All input validated at system boundaries</li>
<li>[ ] No secrets in code or version control</li>
<li>[ ] Auth and authorization checked on all protected routes</li>
<li>[ ] <code>npm audit</code> shows no high/critical vulnerabilities</li>
<li>[ ] Security headers configured</li>
</ul>