<hr>
<p>name: frontend-ui-engineering
<br>description: Component architecture, design systems, and accessibility. Use when building or modifying user-facing interfaces.
</p>
<hr>
<h1>Frontend UI Engineering</h1>
<h2>Overview</h2>
<p>Build UIs that are accessible, performant, and maintainable. Component architecture, design systems, state management, and WCAG 2.1 AA accessibility are non-negotiable.
</p>
<h2>When to Use</h2>
<ul><li>Building or modifying user-facing interfaces</li>
<li>Creating new components</li>
<li>Implementing responsive layouts</li>
<li>Any UI change that affects users</li>
</ul>
<h2>Component Architecture</h2>
<strong>Single responsibility:</strong> Each component does one thing. If a component needs a long comment to explain what it does, split it.
<strong>Composition over inheritance:</strong> Build complex UIs from simple, composable pieces.
<strong>Container/Presentational split:</strong>
<pre><code>// Presentational: pure UI, no data fetching
<p>function TaskCard({ title, status, onComplete }: TaskCardProps) {
<br>return (
<br>&lt;div className=&quot;task-card&quot;&gt;
<br>&lt;h3&gt;{title}&lt;/h3&gt;
<br>&lt;Badge&gt;{status}&lt;/Badge&gt;
<br>&lt;button onClick={onComplete}&gt;Complete&lt;/button&gt;
<br>&lt;/div&gt;
<br>);
<br>}
</p>
<p>// Container: data fetching and state
<br>function TaskCardContainer({ taskId }: { taskId: string }) {
<br>const { task, completeTask } = useTask(taskId);
<br>return &lt;TaskCard {...task} onComplete={completeTask} /&gt;;
<br>}</code></pre>
</p>
<h2>State Management</h2>
<ul><li><strong>Local state first:</strong> Use <code>useState</code> for UI state that doesn't need to be shared</li>
<li><strong>Lift state up:</strong> When two components need the same state, lift it to their common ancestor</li>
<li><strong>Server state separately:</strong> Use React Query or SWR for server data — don't put it in global state</li>
<li><strong>Global state sparingly:</strong> Only for truly global concerns (auth, theme, user preferences)</li>
</ul>
<h2>Accessibility (WCAG 2.1 AA)</h2>
<p>Every UI component must be accessible:
</p>
<strong>Keyboard navigation:</strong>
<ul><li>All interactive elements reachable via Tab</li>
<li>Logical focus order</li>
<li>Visible focus indicators</li>
<li>Escape closes modals and dropdowns</li>
</ul>
<strong>Screen readers:</strong>
<ul><li>Semantic HTML (<code><button></code>, <code><nav></code>, <code><main></code>, not <code><div></code> for everything)</li>
<li>Alt text for all images</li>
<li>ARIA labels for icon-only buttons</li>
<li>Live regions for dynamic content updates</li>
</ul>
<strong>Visual design:</strong>
<ul><li>Color contrast ratio ≥ 4.5:1 for normal text, 3:1 for large text</li>
<li>Don't rely on color alone to convey information</li>
<li>Text resizable to 200% without loss of functionality</li>
</ul>
<h2>Responsive Design</h2>
<ul><li>Mobile-first: design for small screens, then enhance for larger</li>
<li>Use relative units (rem, %, vw) not fixed pixels for layout</li>
<li>Test at 320px, 768px, 1024px, 1440px breakpoints</li>
</ul>
<h2>Verification</h2>
<ul><li>[ ] Component has a single, clear responsibility</li>
<li>[ ] All interactive elements keyboard accessible</li>
<li>[ ] Screen reader tested (or ARIA attributes verified)</li>
<li>[ ] Color contrast meets WCAG AA</li>
<li>[ ] Responsive at all target breakpoints</li>
<li>[ ] No console errors or warnings</li>
</ul>