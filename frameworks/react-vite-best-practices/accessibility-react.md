<hr>
<p>name: accessibility-react
<br>description: Accessibility implementation for React components. Use when building any user-facing UI — accessibility is not optional.
</p>
<hr>
<h1>Accessibility in React</h1>
<h2>Overview</h2>
<p>Accessibility (a11y) is not a feature — it's a quality standard. WCAG 2.1 AA compliance is the baseline. This skill covers the patterns that make React UIs accessible to keyboard users, screen reader users, and users with motor or visual impairments.
</p>
<h2>Semantic HTML First</h2>
<p>The most impactful accessibility improvement is using the right HTML element:
</p>
<pre><code>// ❌ Div soup — no semantics, no keyboard access
<p>&lt;div onClick={handleSubmit} className=&quot;button&quot;&gt;Submit&lt;/div&gt;
</p>
<p>// ✅ Semantic — keyboard accessible, screen reader announces role
<br>&lt;button type=&quot;submit&quot; onClick={handleSubmit}&gt;Submit&lt;/button&gt;
</p>
<p>// ❌ Div as navigation
<br>&lt;div className=&quot;nav&quot;&gt;
<br>&lt;div onClick={() =&gt; navigate(&#39;/home&#39;)}&gt;Home&lt;/div&gt;
<br>&lt;/div&gt;
</p>
<p>// ✅ Semantic navigation
<br>&lt;nav aria-label=&quot;Main navigation&quot;&gt;
<br>&lt;a href=&quot;/home&quot;&gt;Home&lt;/a&gt;
<br>&lt;/nav&gt;</code></pre>
</p>
<h2>ARIA — Use Sparingly</h2>
<p>ARIA should supplement HTML semantics, not replace them. The first rule of ARIA: don't use ARIA if a native HTML element does the job.
</p>
<pre><code>// ✅ Labeling — always label interactive elements
<p>&lt;button aria-label=&quot;Close dialog&quot;&gt;
<br>&lt;XIcon aria-hidden=&quot;true&quot; /&gt;
<br>&lt;/button&gt;
</p>
<p>// ✅ Live regions — announce dynamic content
<br>&lt;div role=&quot;status&quot; aria-live=&quot;polite&quot; aria-atomic=&quot;true&quot;&gt;
<br>{saveStatus === &#39;saved&#39; &amp;&amp; &#39;Changes saved&#39;}
<br>&lt;/div&gt;
</p>
<p>// ✅ Error messages — link to the field
<br>&lt;input
<br>id=&quot;email&quot;
<br>aria-describedby={emailError ? &#39;email-error&#39; : undefined}
<br>aria-invalid={!!emailError}
<br>/&gt;
<br>{emailError &amp;&amp; (
<br>&lt;p id=&quot;email-error&quot; role=&quot;alert&quot;&gt;{emailError}&lt;/p&gt;
<br>)}
</p>
<p>// ✅ Expanded state for toggles
<br>&lt;button
<br>aria-expanded={isOpen}
<br>aria-controls=&quot;dropdown-menu&quot;
<br>onClick={() =&gt; setIsOpen(!isOpen)}
<br>&gt;
<br>Options
<br>&lt;/button&gt;
<br>&lt;ul id=&quot;dropdown-menu&quot; hidden={!isOpen}&gt;...&lt;/ul&gt;</code></pre>
</p>
<h2>Focus Management</h2>
<pre><code>// ✅ Trap focus in modals
<p>import { useEffect, useRef } from &#39;react&#39;;
</p>
<p>function Modal({ isOpen, onClose, children }: ModalProps) {
<br>const modalRef = useRef&lt;HTMLDivElement&gt;(null);
</p>
<p>useEffect(() =&gt; {
<br>if (!isOpen) return;
</p>
<p>// Focus the modal when it opens
<br>modalRef.current?.focus();
</p>
<p>// Trap focus within modal
<br>function handleKeyDown(e: KeyboardEvent) {
<br>if (e.key === &#39;Escape&#39;) onClose();
<br>if (e.key !== &#39;Tab&#39;) return;
</p>
<p>const focusable = modalRef.current?.querySelectorAll&lt;HTMLElement&gt;(
<br>&#39;button, [href], input, select, textarea, [tabindex]:not([tabindex=&quot;-1&quot;])&#39;
<br>);
<br>if (!focusable?.length) return;
</p>
<p>const first = focusable[0];
<br>const last = focusable[focusable.length - 1];
</p>
<p>if (e.shiftKey &amp;&amp; document.activeElement === first) {
<br>e.preventDefault();
<br>last.focus();
<br>} else if (!e.shiftKey &amp;&amp; document.activeElement === last) {
<br>e.preventDefault();
<br>first.focus();
<br>}
<br>}
</p>
<p>document.addEventListener(&#39;keydown&#39;, handleKeyDown);
<br>return () =&gt; document.removeEventListener(&#39;keydown&#39;, handleKeyDown);
<br>}, [isOpen, onClose]);
</p>
<p>if (!isOpen) return null;
</p>
<p>return (
<br>&lt;div
<br>role=&quot;dialog&quot;
<br>aria-modal=&quot;true&quot;
<br>aria-labelledby=&quot;modal-title&quot;
<br>ref={modalRef}
<br>tabIndex={-1}
<br>&gt;
<br>{children}
<br>&lt;/div&gt;
<br>);
<br>}</code></pre>
</p>
<h2>Forms</h2>
<pre><code>// ✅ Every input has a visible label
<p>function FormField({ label, name, error, ...props }: FormFieldProps) {
<br>const id = React.useId();
<br>const errorId = <code>${id}-error</code>;
</p>
<p>return (
<br>&lt;div&gt;
<br>&lt;label htmlFor={id}&gt;{label}&lt;/label&gt;
<br>&lt;input
<br>id={id}
<br>name={name}
<br>aria-describedby={error ? errorId : undefined}
<br>aria-invalid={!!error}
<br>{...props}
<br>/&gt;
<br>{error &amp;&amp; (
<br>&lt;p id={errorId} role=&quot;alert&quot; aria-live=&quot;assertive&quot;&gt;
<br>{error}
<br>&lt;/p&gt;
<br>)}
<br>&lt;/div&gt;
<br>);
<br>}</code></pre>
</p>
<h2>Color and Contrast</h2>
<pre><code>// ✅ Never use color alone to convey information
<p>// ❌ Bad — only color distinguishes states
<br>&lt;span style={{ color: isError ? &#39;red&#39; : &#39;green&#39; }}&gt;{status}&lt;/span&gt;
</p>
<p>// ✅ Good — color + icon + text
<br>&lt;span className={isError ? &#39;text-error&#39; : &#39;text-success&#39;}&gt;
<br>{isError ? &lt;ErrorIcon aria-hidden=&quot;true&quot; /&gt; : &lt;CheckIcon aria-hidden=&quot;true&quot; /&gt;}
<br>{status}
<br>&lt;/span&gt;</code></pre>
</p>
<p>Minimum contrast ratios (WCAG AA):
</p>
<ul><li>Normal text: 4.5:1</li>
<li>Large text (18px+ or 14px+ bold): 3:1</li>
<li>UI components and graphics: 3:1</li>
</ul>
<h2>Skip Links</h2>
<pre><code>// Add to App.tsx — first element in the DOM
<p>function App() {
<br>return (
<br>&lt;&gt;
<br>&lt;a
<br>href=&quot;#main-content&quot;
<br>className=&quot;sr-only focus:not-sr-only focus:absolute focus:top-4 focus:left-4 focus:z-50 focus:px-4 focus:py-2 focus:bg-white focus:text-black&quot;
<br>&gt;
<br>Skip to main content
<br>&lt;/a&gt;
<br>&lt;Header /&gt;
<br>&lt;main id=&quot;main-content&quot; tabIndex={-1}&gt;
<br>&lt;Outlet /&gt;
<br>&lt;/main&gt;
<br>&lt;/&gt;
<br>);
<br>}</code></pre>
</p>
<h2>Screen Reader Only Text</h2>
<pre><code>/<em> src/styles/utilities.css </em>/
<p>.sr-only {
<br>position: absolute;
<br>width: 1px;
<br>height: 1px;
<br>padding: 0;
<br>margin: -1px;
<br>overflow: hidden;
<br>clip: rect(0, 0, 0, 0);
<br>white-space: nowrap;
<br>border-width: 0;
<br>}</code></pre>
</p>
<pre><code>// ✅ Provide context for screen readers without visual clutter
<p>&lt;button onClick={handleDelete}&gt;
<br>&lt;TrashIcon aria-hidden=&quot;true&quot; /&gt;
<br>&lt;span className=&quot;sr-only&quot;&gt;Delete {item.name}&lt;/span&gt;
<br>&lt;/button&gt;</code></pre>
</p>
<h2>Keyboard Navigation Checklist</h2>
<ul><li>[ ] All interactive elements reachable via Tab</li>
<li>[ ] Focus order follows visual/logical order</li>
<li>[ ] Focus indicator visible (never <code>outline: none</code> without replacement)</li>
<li>[ ] Escape closes modals, dropdowns, and popovers</li>
<li>[ ] Arrow keys navigate within menus and listboxes</li>
<li>[ ] Enter/Space activate buttons and checkboxes</li>
</ul>
<h2>Testing Accessibility</h2>
<pre><code>// Automated — catches ~30% of issues
<p>import { axe } from &#39;jest-axe&#39;;
<br>const results = await axe(container);
<br>expect(results).toHaveNoViolations();
</p>
<p>// Manual — required for the other 70%
<br>// 1. Tab through the entire page — can you reach everything?
<br>// 2. Use VoiceOver (Mac) or NVDA (Windows) — does it make sense?
<br>// 3. Zoom to 200% — does layout break?
<br>// 4. Disable CSS — is content still readable?</code></pre>
</p>
<h2>Verification</h2>
<ul><li>[ ] All images have alt text (or <code>alt=""</code> for decorative)</li>
<li>[ ] All form inputs have associated labels</li>
<li>[ ] Error messages linked to inputs via <code>aria-describedby</code></li>
<li>[ ] Modals trap focus and close on Escape</li>
<li>[ ] Skip link present in App shell</li>
<li>[ ] Color contrast meets WCAG AA</li>
<li>[ ] axe audit passes on every new page</li>
</ul>