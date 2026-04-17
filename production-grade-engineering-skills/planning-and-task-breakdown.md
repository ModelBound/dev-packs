<hr>
<p>name: planning-and-task-breakdown
<br>description: Decomposes specs into small, verifiable tasks. Use when you have a spec and need implementable units with acceptance criteria and dependency ordering.
</p>
<hr>
<h1>Planning and Task Breakdown</h1>
<h2>Overview</h2>
<p>Decompose a validated spec into small, verifiable tasks with acceptance criteria and dependency ordering. A good plan makes implementation mechanical — each task is clear enough that there's no ambiguity about what "done" means.
</p>
<h2>When to Use</h2>
<ul><li>You have a validated spec and need to plan implementation</li>
<li>A feature is too large to implement in one session</li>
<li>Multiple people or agents will work on the same feature</li>
<li>You need to estimate effort or sequence work</li>
</ul>
<h2>The Process</h2>
<h3>Step 1: Identify Major Components</h3>
<p>List the major pieces that need to be built:
</p>
<ul><li>Data layer (schema, migrations, queries)</li>
<li>Business logic (services, utilities)</li>
<li>API layer (endpoints, validation)</li>
<li>UI layer (components, state)</li>
<li>Tests (unit, integration, E2E)</li>
<li>Infrastructure (config, deployment)</li>
</ul>
<h3>Step 2: Map Dependencies</h3>
<p>Draw the dependency graph — what must be built before what:
</p>
<pre><code>Schema migration
<p>│
<br>▼
<br>Data access layer
<br>│
<br>▼
<br>Business logic ──→ API endpoints
<br>│
<br>▼
<br>UI components</code></pre>
</p>
<h3>Step 3: Create Tasks</h3>
<p>For each component, create tasks following this template:
</p>
<pre><code>- [ ] Task: [Description — imperative, specific]
<p>- Acceptance: [What must be true when done — testable]
<br>- Verify: [How to confirm — test command, build, manual check]
<br>- Files: [Which files will be touched]
<br>- Depends on: [Task IDs this depends on]</code></pre>
</p>
<h3>Task Sizing Rules</h3>
<ul><li>Each task should be completable in a single focused session (~1-2 hours)</li>
<li>No task should require changing more than ~5 files</li>
<li>Each task should leave the system in a working state</li>
<li>If a task feels too large, split it</li>
</ul>
<h3>Step 4: Order Tasks</h3>
<p>Order by dependency, not by perceived importance:
</p>
<li>Foundation tasks first (schema, types, interfaces)</li>
<li>Core logic second</li>
<li>Integration third</li>
<li>Polish and edge cases last</li>
<h2>Common Rationalizations</h2>
<p>| Rationalization | Reality |
<br>|---|---|
<br>| "I'll figure out the tasks as I go" | Unplanned work expands to fill available time. Tasks without acceptance criteria are never done. |
<br>| "These tasks are too small" | Small tasks are fast to complete and easy to verify. Large tasks hide complexity. |
</p>
<h2>Verification</h2>
<ul><li>[ ] Every task has explicit acceptance criteria</li>
<li>[ ] Every task has a verification step</li>
<li>[ ] Tasks are ordered by dependency</li>
<li>[ ] No task requires changing more than ~5 files</li>
<li>[ ] The human has reviewed and approved the task list</li>
</ul>