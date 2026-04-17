<hr>
<p>name: source-driven-development
<br>description: Ground every framework decision in official documentation. Use when you want authoritative, source-cited code for any framework or library.
</p>
<hr>
<h1>Source-Driven Development</h1>
<h2>Overview</h2>
<p>Ground every framework and library decision in official documentation. Verify before implementing. Cite sources. Flag what's unverified. AI agents confidently produce plausible-but-wrong code — source-driven development is the antidote.
</p>
<h2>When to Use</h2>
<ul><li>Using a framework or library you're not certain about</li>
<li>Implementing a pattern that might have changed between versions</li>
<li>Any time you're about to write code based on memory rather than documentation</li>
</ul>
<h2>The Process</h2>
<h3>Step 1: Identify What Needs Verification</h3>
<p>Before writing any framework-specific code, list what you need to verify:
</p>
<pre><code>NEEDS VERIFICATION:
<li>Next.js App Router data fetching pattern (v14+)</li>
<li>Prisma transaction API (v5+)</li>
<li>React Query v5 mutation syntax</code></pre></li>
<h3>Step 2: Find the Official Source</h3>
<p>Priority order for sources:
</p>
<li>Official documentation (docs.framework.com)</li>
<li>Official GitHub repository (README, examples)</li>
<li>Official changelog or migration guide</li>
<li>Official blog posts from maintainers</li>
<p>Not acceptable as primary sources: Stack Overflow, blog posts, tutorials, AI-generated content.
</p>
<h3>Step 3: Verify the Version</h3>
<p>Always check that the documentation matches the version you're using:
</p>
<pre><code>// Verify: package.json
<p>&quot;next&quot;: &quot;^14.2.0&quot;  // Using App Router patterns, not Pages Router
<br>&quot;prisma&quot;: &quot;^5.0.0&quot;  // Using v5 API, not v4</code></pre>
</p>
<h3>Step 4: Cite Your Sources</h3>
<p>When implementing from documentation, cite it:
</p>
<pre><code>// Source: https://nextjs.org/docs/app/building-your-application/data-fetching/fetching
<p>// Next.js 14 App Router — server component data fetching
<br>async function TaskList() {
<br>const tasks = await fetch(&#39;/api/tasks&#39;, { cache: &#39;no-store&#39; });
<br>// ...
<br>}</code></pre>
</p>
<h3>Step 5: Flag What's Unverified</h3>
<p>If you're implementing something you haven't verified:
</p>
<pre><code>⚠️ UNVERIFIED: The following uses the Prisma v5 batch API.
<p>I&#39;m working from memory — verify against https://www.prisma.io/docs before shipping.</code></pre>
</p>
<h2>Common Rationalizations</h2>
<p>| Rationalization | Reality |
<br>|---|---|
<br>| "I know this framework well" | APIs change between versions. Verify anyway. |
<br>| "The AI generated this, it's probably right" | AI confidently produces outdated or incorrect API usage. Always verify. |
</p>
<h2>Verification</h2>
<ul><li>[ ] All framework-specific code verified against official documentation</li>
<li>[ ] Version numbers checked against package.json</li>
<li>[ ] Sources cited in comments for non-obvious patterns</li>
<li>[ ] Unverified code explicitly flagged</li>
</ul>