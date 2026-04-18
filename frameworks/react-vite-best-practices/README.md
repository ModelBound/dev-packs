<h1>React + Vite Best Practices</h1>
<blockquote><p>Writes production-grade React 19 + Vite 6 code with TypeScript, TanStack Query, proper state management, accessibility, and performance optimization baked in.</p></blockquote>
<strong>Pack slug:</strong> <code>react-vite-best-practices</code>
<strong>Version:</strong> 1.0.0
<strong>Author:</strong> ModelBound (Official)
<hr>
<h2>What it does</h2>
<p>Configures your AI agent to write React + Vite code that would pass a senior engineer's code review:
</p>
<ul><li><strong>React 19 patterns</strong> — <code>useActionState</code>, <code>useOptimistic</code>, <code>use()</code> for async resources, and React Compiler-aware code that avoids unnecessary manual memoization</li>
<li><strong>Vite 6 configuration</strong> — typed env vars, path aliases, manual chunk splitting, route-level code splitting, and bundle analysis</li>
<li><strong>TypeScript strict mode</strong> — discriminated unions, generic components, correct event types, no <code>any</code>, exhaustive switches</li>
<li><strong>TanStack Query v5</strong> — query key factories, optimistic mutations, Suspense boundaries, proper cache invalidation</li>
<li><strong>State management decisions</strong> — the right tool for each layer: <code>useState</code> for local, Context for feature-level, Zustand for global UI, TanStack Query for server data, URL for shareable state</li>
<li><strong>Testing with Vitest + MSW</strong> — component integration tests, hook tests, API mocking, accessibility audits with axe</li>
<li><strong>WCAG 2.1 AA accessibility</strong> — semantic HTML, ARIA patterns, focus management, skip links, color contrast</li>
<li><strong>Performance</strong> — <code>useTransition</code>, <code>useDeferredValue</code>, virtualization for large lists, image optimization, Core Web Vitals targets</li>
</ul>
<h2>Who it's for</h2>
<ul><li><strong>React developers</strong> who want their code to reflect current best practices, not 2020 patterns</li>
<li><strong>Teams adopting React 19</strong> and the React Compiler who need guidance on what to stop doing manually</li>
<li><strong>Developers migrating to Vite</strong> from Create React App or Webpack who want an optimized config from day one</li>
<li><strong>Anyone building production SPAs</strong> who cares about accessibility, performance, and maintainability</li>
<li><strong>Code reviewers</strong> who want consistent standards enforced automatically</li>
</ul>
<h2>What's inside</h2>
<p>| File | Type | Covers |
<br>|------|------|--------|
<br>| <code>React + Vite Expert</code> | System Prompt | Agent identity, non-negotiables, core principles |
<br>| <code>React + Vite Project Rules</code> | Cursor Rules | Tech stack, feature-based folder structure, code style, boundaries |
<br>| <code>react-component-patterns</code> | Skill | Component anatomy, compound components, React 19 APIs (<code>useActionState</code>, <code>useOptimistic</code>, <code>use()</code>) |
<br>| <code>vite-configuration</code> | Skill | <code>vite.config.ts</code>, env config module, code splitting, bundle analysis, tsconfig aliases |
<br>| <code>data-fetching-patterns</code> | Skill | TanStack Query v5 setup, query key factory, mutation hooks, optimistic updates, Suspense |
<br>| <code>typescript-react-patterns</code> | Skill | Generic components, discriminated unions, event types, utility types, strict tsconfig |
<br>| <code>testing-react-vite</code> | Skill | Vitest + Testing Library, MSW for API mocking, hook tests, accessibility testing |
<br>| <code>performance-optimization</code> | Skill | React Compiler guidance, <code>useTransition</code>, <code>useDeferredValue</code>, virtualization, Core Web Vitals |
<br>| <code>accessibility-react</code> | Skill | WCAG 2.1 AA, ARIA patterns, focus trapping, semantic HTML, skip links |
<br>| <code>state-management</code> | Skill | Decision framework, Context, Zustand, URL state, React Hook Form + Zod |
</p>
<h2>Install</h2>
<h3>Cursor (<code>.cursor/rules/</code>)</h3>
<pre><code>---
<p>description: Write production-grade React 19 + Vite 6 code with TypeScript, TanStack Query, accessibility, and performance best practices.
<br>globs: [&quot;src/<strong>/<em>.tsx&quot;, &quot;src/</strong>/</em>.ts&quot;, &quot;vite.config.ts&quot;, &quot;*.config.ts&quot;]
<br>alwaysApply: false
<br>---</code></pre>
</p>
<h3>Kiro (<code>.kiro/steering/react-vite.md</code>)</h3>
<p>Add as steering with <code>inclusion: auto</code> for React projects. Kiro will apply the rules consistently across all component and hook work.
</p>
<h3>Windsurf (<code>.windsurf/rules/react-vite.md</code>)</h3>
<p>Drop in as a rule file. Works well with Windsurf's cascade mode for enforcing patterns across a codebase.
</p>
<h3>Claude Code (<code>.claude/skills/react-vite/SKILL.md</code>)</h3>
<pre><code>---
<p>name: react-vite-best-practices
<br>description: Write or review React 19 + Vite 6 code. Use when creating components, hooks, data fetching, forms, or configuring Vite.
<br>---</code></pre>
</p>
<h3>ModelBound MCP</h3>
<pre><code>{
<p>&quot;mcpServers&quot;: {
<br>&quot;modelbound&quot;: {
<br>&quot;command&quot;: &quot;npx&quot;,
<br>&quot;args&quot;: [&quot;-y&quot;, &quot;mcp-remote&quot;, &quot;https://mcp.modelbound.co&quot;]
<br>}
<br>}
<br>}</code></pre>
</p>
<p>Then ask your agent: <code>"Load the React + Vite Best Practices pack and apply it to this project."</code>
</p>
<h2>Usage tips</h2>
<ul><li><strong>Tell the agent your UI library</strong> (Tailwind, shadcn/ui, Radix, MUI) so it generates idiomatic styling, not generic inline styles.</li>
<li><strong>Pair with <code>testing-react-vite</code></strong> when adding new features — the agent will write tests alongside the implementation, not after.</li>
<li><strong>Use <code>vite-configuration</code> when starting a new project</strong> — it generates a complete <code>vite.config.ts</code> with React Compiler, path aliases, and chunk splitting configured correctly from day one.</li>
<li><strong>Don't fight the React Compiler guidance</strong> — if the agent removes your <code>useMemo</code> or <code>useCallback</code>, it's because the compiler handles it. Trust the measurement-first approach.</li>
<li><strong>Use <code>state-management</code> as a decision guide</strong> — ask the agent "where should this state live?" before implementing. It will walk through the decision tree.</li>
<li><strong>Run <code>npm run build:analyze</code></strong> after the agent makes significant changes to verify no chunk has grown unexpectedly.</li>
<li><strong>Accessibility is enforced, not optional</strong> — the agent will add ARIA attributes, labels, and focus management. Don't skip these in code review.</li>
</ul>
<h2>Key opinions in this pack</h2>
<p>This pack takes strong positions on a few contested topics:
</p>
<strong>React Compiler over manual memoization.</strong> With React 19's compiler enabled, <code>useMemo</code>, <code>useCallback</code>, and <code>React.memo</code> are code smells unless you have a measured performance problem. The compiler is smarter.
<strong>TanStack Query for all server state.</strong> Never put API data in <code>useState</code> or Zustand. Server state has different semantics (stale, loading, error) that TanStack Query handles correctly.
<strong>Feature-based folder structure.</strong> Files are organized by feature, not by type. <code>components/</code>, <code>hooks/</code>, <code>utils/</code> at the top level don't scale. <code>features/auth/</code>, <code>features/dashboard/</code> do.
<strong>Accessibility is non-negotiable.</strong> WCAG 2.1 AA is the baseline. The agent will not skip labels, alt text, or focus management to save lines of code.
<strong>URL state is underused.</strong> Filters, pagination, search queries, and active tabs belong in the URL. They're shareable, bookmarkable, and survive page refreshes.
<h2>Compatibility</h2>
<p>| IDE / Agent | Status |
<br>|---|---|
<br>| Cursor | ✅ Recommended |
<br>| Kiro | ✅ Recommended |
<br>| Windsurf | ✅ Supported |
<br>| Claude Code | ✅ Supported |
<br>| GitHub Copilot | ✅ Supported |
<br>| Continue.dev | ✅ Supported |
</p>
<h2>Requirements</h2>
<ul><li>React 19+</li>
<li>Vite 6+</li>
<li>TypeScript 5.x</li>
<li>Node.js 20+</li>
</ul>
<p>Works with any React meta-framework (Vite SPA, React Router v7, TanStack Start). Not designed for Next.js — use the Next.js pack for that.
</p>
<h2>License</h2>
<p>MIT — © ModelBound.
</p>
