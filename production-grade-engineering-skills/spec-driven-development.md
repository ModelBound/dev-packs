<hr>
<p>name: spec-driven-development
<br>description: Creates specs before coding. Use when starting a new project, feature, or significant change and no specification exists yet. Use when requirements are unclear, ambiguous, or only exist as a vague idea.
</p>
<hr>
<h1>Spec-Driven Development</h1>
<h2>Overview</h2>
<p>Write a structured specification before writing any code. The spec is the shared source of truth — it defines what we're building, why, and how we'll know it's done. Code without a spec is guessing.
</p>
<h2>When to Use</h2>
<ul><li>Starting a new project or feature</li>
<li>Requirements are ambiguous or incomplete</li>
<li>The change touches multiple files or modules</li>
<li>You're about to make an architectural decision</li>
<li>The task would take more than 30 minutes to implement</li>
</ul>
<strong>When NOT to use:</strong> Single-line fixes, typo corrections, or changes where requirements are unambiguous and self-contained.
<h2>The Gated Workflow</h2>
<pre><code>SPECIFY ──→ PLAN ──→ TASKS ──→ IMPLEMENT
<p>│          │        │          │
<br>▼          ▼        ▼          ▼
<br>Human      Human    Human      Human
<br>reviews    reviews  reviews    reviews</code></pre>
</p>
<p>Do not advance to the next phase until the current one is validated.
</p>
<h2>Phase 1: Specify</h2>
<p>Surface assumptions immediately before writing any spec content:
</p>
<pre><code>ASSUMPTIONS I&#39;M MAKING:
<li>This is a web application (not native mobile)</li>
<li>Authentication uses session-based cookies (not JWT)</li>
<li>The database is PostgreSQL</li>
<p>→ Correct me now or I&#39;ll proceed with these.</code></pre>
</p>
<strong>Spec template:</strong>
<pre><code># Spec: [Project/Feature Name]
<h2>Objective</h2>
<p>[What we&#39;re building and why. User stories or acceptance criteria.]
</p>
<h2>Tech Stack</h2>
<p>[Framework, language, key dependencies with versions]
</p>
<h2>Commands</h2>
<p>[Build, test, lint, dev — full commands]
</p>
<h2>Project Structure</h2>
<p>[Directory layout with descriptions]
</p>
<h2>Code Style</h2>
<p>[Example snippet + key conventions]
</p>
<h2>Testing Strategy</h2>
<p>[Framework, test locations, coverage requirements, test levels]
</p>
<h2>Boundaries</h2>
<ul><li>Always: [...]</li>
<li>Ask first: [...]</li>
<li>Never: [...]</li>
</ul>
<h2>Success Criteria</h2>
<p>[How we&#39;ll know this is done — specific, testable conditions]
</p>
<h2>Open Questions</h2>
<p>[Anything unresolved that needs human input]</code></pre>
</p>
<h2>Phase 2: Plan</h2>
<p>Generate a technical implementation plan covering major components, implementation order, risks, and verification checkpoints.
</p>
<h2>Phase 3: Tasks</h2>
<p>Break the plan into discrete tasks. Each task:
</p>
<ul><li>Completable in a single focused session</li>
<li>Has explicit acceptance criteria</li>
<li>Includes a verification step</li>
<li>Touches no more than ~5 files</li>
</ul>
<h2>Phase 4: Implement</h2>
<p>Execute tasks one at a time using <code>incremental-implementation</code> and <code>test-driven-development</code>.
</p>
<h2>Common Rationalizations</h2>
<p>| Rationalization | Reality |
<br>|---|---|
<br>| "This is simple, I don't need a spec" | Simple tasks still need acceptance criteria. A two-line spec is fine. |
<br>| "I'll write the spec after I code it" | That's documentation, not specification. |
<br>| "The spec will slow us down" | A 15-minute spec prevents hours of rework. |
</p>
<h2>Red Flags</h2>
<ul><li>Starting to write code without any written requirements</li>
<li>Making architectural decisions without documenting them</li>
<li>Skipping the spec because "it's obvious what to build"</li>
</ul>
<h2>Verification</h2>
<ul><li>[ ] The spec covers all six core areas</li>
<li>[ ] The human has reviewed and approved the spec</li>
<li>[ ] Success criteria are specific and testable</li>
<li>[ ] Boundaries (Always/Ask First/Never) are defined</li>
<li>[ ] The spec is saved to a file in the repository</li>
</ul>