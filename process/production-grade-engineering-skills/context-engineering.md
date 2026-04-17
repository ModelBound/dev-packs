<hr>
<p>name: context-engineering
<br>description: Feed agents the right information at the right time. Use when starting a session, switching tasks, or when output quality drops.
</p>
<hr>
<h1>Context Engineering</h1>
<h2>Overview</h2>
<p>AI agents are only as good as the context they receive. Context engineering is the discipline of providing the right information at the right time — not everything at once, not too little to be useful.
</p>
<h2>When to Use</h2>
<ul><li>Starting a new agent session</li>
<li>Switching between tasks in the same session</li>
<li>When agent output quality drops</li>
<li>When the agent seems to be working from stale or incorrect assumptions</li>
</ul>
<h2>The Context Loading Pattern</h2>
<p>Load context in layers, from most stable to most dynamic:
</p>
<pre><code>Layer 1 (always): Project rules, coding standards, tech stack
<p>Layer 2 (per task): Relevant source files, related tests
<br>Layer 3 (per session): Recent decisions, current state, open questions
<br>Layer 4 (per turn): Specific error messages, current file content</code></pre>
</p>
<p>Don't dump everything into Layer 1. Stable context belongs in rules files. Dynamic context belongs in the conversation.
</p>
<h2>Rules Files</h2>
<p>Store stable project context in rules files that agents load automatically:
</p>
<ul><li><code>.cursor/rules/</code> for Cursor</li>
<li><code>.claude/instructions.md</code> for Claude Code</li>
<li><code>.github/copilot-instructions.md</code> for Copilot</li>
<li><code>.amazonq/rules/</code> for Amazon Q</li>
</ul>
<p>Rules files should contain:
</p>
<ul><li>Tech stack and versions</li>
<li>Coding conventions with examples</li>
<li>Project structure</li>
<li>What to always do / ask first / never do</li>
</ul>
<h2>Context Packing</h2>
<p>Before starting a task, pack the relevant context:
</p>
<li>Load the spec or task description</li>
<li>Load the relevant source files (not the whole codebase)</li>
<li>Load related tests</li>
<li>Load any recent decisions or constraints</li>
<strong>Don't load:</strong> Files unrelated to the current task, entire codebases, documentation for libraries you're not using.
<h2>MCP Integrations</h2>
<p>Use MCP servers to give agents live access to context:
</p>
<ul><li>Search your knowledge base for relevant documentation</li>
<li>Retrieve the current state of external systems</li>
<li>Access tools that extend agent capabilities</li>
</ul>
<h2>Session Handoff</h2>
<p>When a session ends before a task is complete, write a handoff note:
</p>
<pre><code>## Session Handoff
<h3>What was accomplished</h3>
<p>[List completed tasks]
</p>
<h3>Current state</h3>
<p>[What&#39;s in progress, what&#39;s broken, what&#39;s working]
</p>
<h3>Next steps</h3>
<p>[Exactly what to do next, in order]
</p>
<h3>Open questions</h3>
<p>[Anything unresolved that needs a decision]
</p>
<h3>Relevant files</h3>
<p>[Files that will be needed in the next session]</code></pre>
</p>
<p>Load this note at the start of the next session.
</p>
<h2>Verification</h2>
<ul><li>[ ] Rules files contain stable project context</li>
<li>[ ] Only relevant files loaded for current task</li>
<li>[ ] Session handoff note written if task is incomplete</li>
<li>[ ] Agent is working from current, accurate context</li>
</ul>