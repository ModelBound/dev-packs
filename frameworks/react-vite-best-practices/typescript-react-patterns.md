<hr>
<p>name: typescript-react-patterns
<br>description: TypeScript patterns specific to React development. Use when typing components, hooks, events, generics, or working with third-party library types.
</p>
<hr>
<h1>TypeScript React Patterns</h1>
<h2>Overview</h2>
<p>TypeScript in React is not just about adding types — it's about making impossible states impossible and letting the type system catch bugs before runtime. This skill covers the patterns that make TypeScript genuinely useful in React codebases.
</p>
<h2>Component Typing</h2>
<pre><code>// ✅ Props interface — always named, always before the component
<p>interface ButtonProps {
<br>variant?: &#39;primary&#39; | &#39;secondary&#39; | &#39;ghost&#39;;
<br>size?: &#39;sm&#39; | &#39;md&#39; | &#39;lg&#39;;
<br>isLoading?: boolean;
<br>children: React.ReactNode;
<br>onClick?: () =&gt; void;
<br>}
</p>
<p>// ✅ Extend HTML element props for wrapper components
<br>interface InputProps extends React.InputHTMLAttributes&lt;HTMLInputElement&gt; {
<br>label: string;
<br>error?: string;
<br>hint?: string;
<br>}
</p>
<p>export function Input({ label, error, hint, ...inputProps }: InputProps) {
<br>const id = React.useId();
<br>return (
<br>&lt;div&gt;
<br>&lt;label htmlFor={id}&gt;{label}&lt;/label&gt;
<br>&lt;input id={id} aria-describedby={error ? <code>${id}-error</code> : undefined} {...inputProps} /&gt;
<br>{error &amp;&amp; &lt;p id={<code>${id}-error</code>} role=&quot;alert&quot;&gt;{error}&lt;/p&gt;}
<br>&lt;/div&gt;
<br>);
<br>}</code></pre>
</p>
<h2>Generic Components</h2>
<pre><code>// ✅ Generic list component — type flows through
<p>interface ListProps&lt;T&gt; {
<br>items: T[];
<br>keyExtractor: (item: T) =&gt; string;
<br>renderItem: (item: T) =&gt; React.ReactNode;
<br>emptyState?: React.ReactNode;
<br>}
</p>
<p>export function List&lt;T&gt;({ items, keyExtractor, renderItem, emptyState }: ListProps&lt;T&gt;) {
<br>if (items.length === 0) return &lt;&gt;{emptyState ?? &lt;p&gt;No items&lt;/p&gt;}&lt;/&gt;;
<br>return (
<br>&lt;ul&gt;
<br>{items.map((item) =&gt; (
<br>&lt;li key={keyExtractor(item)}&gt;{renderItem(item)}&lt;/li&gt;
<br>))}
<br>&lt;/ul&gt;
<br>);
<br>}
</p>
<p>// Usage — T is inferred as User
<br>&lt;List
<br>items={users}
<br>keyExtractor={(u) =&gt; u.id}
<br>renderItem={(u) =&gt; &lt;UserCard user={u} /&gt;}
<br>/&gt;</code></pre>
</p>
<h2>Event Handler Types</h2>
<pre><code>// ✅ Correct event types — never use <code>any</code>
<p>function handleChange(e: React.ChangeEvent&lt;HTMLInputElement&gt;) { ... }
<br>function handleSubmit(e: React.FormEvent&lt;HTMLFormElement&gt;) { ... }
<br>function handleClick(e: React.MouseEvent&lt;HTMLButtonElement&gt;) { ... }
<br>function handleKeyDown(e: React.KeyboardEvent&lt;HTMLInputElement&gt;) { ... }
<br>function handleDrop(e: React.DragEvent&lt;HTMLDivElement&gt;) { ... }
</p>
<p>// ✅ For custom components, type the value directly
<br>function handleSelect(value: string) { ... }
<br>function handleRangeChange(range: [Date, Date]) { ... }</code></pre>
</p>
<h2>Discriminated Unions for State</h2>
<pre><code>// ✅ Make impossible states impossible
<p>type AsyncState&lt;T&gt; =
<br>| { status: &#39;idle&#39; }
<br>| { status: &#39;loading&#39; }
<br>| { status: &#39;success&#39;; data: T }
<br>| { status: &#39;error&#39;; error: Error };
</p>
<p>function useAsync&lt;T&gt;(fn: () =&gt; Promise&lt;T&gt;): AsyncState&lt;T&gt; {
<br>const [state, setState] = useState&lt;AsyncState&lt;T&gt;&gt;({ status: &#39;idle&#39; });
<br>// ...
<br>return state;
<br>}
</p>
<p>// Exhaustive switch — TypeScript catches missing cases
<br>function renderState&lt;T&gt;(state: AsyncState&lt;T&gt;, render: (data: T) =&gt; React.ReactNode) {
<br>switch (state.status) {
<br>case &#39;idle&#39;: return null;
<br>case &#39;loading&#39;: return &lt;Spinner /&gt;;
<br>case &#39;success&#39;: return render(state.data);
<br>case &#39;error&#39;: return &lt;ErrorMessage error={state.error} /&gt;;
<br>// TypeScript error if a case is missing
<br>}
<br>}</code></pre>
</p>
<h2>Hook Return Types</h2>
<pre><code>// ✅ Always type hook return values explicitly
<p>interface UseCounterResult {
<br>count: number;
<br>increment: () =&gt; void;
<br>decrement: () =&gt; void;
<br>reset: () =&gt; void;
<br>}
</p>
<p>function useCounter(initial = 0): UseCounterResult {
<br>const [count, setCount] = useState(initial);
<br>return {
<br>count,
<br>increment: () =&gt; setCount((c) =&gt; c + 1),
<br>decrement: () =&gt; setCount((c) =&gt; c - 1),
<br>reset: () =&gt; setCount(initial),
<br>};
<br>}</code></pre>
</p>
<h2>Context Typing</h2>
<pre><code>// ✅ Typed context with null check helper
<p>interface AuthContextValue {
<br>user: User | null;
<br>signIn: (credentials: Credentials) =&gt; Promise&lt;void&gt;;
<br>signOut: () =&gt; void;
<br>}
</p>
<p>const AuthContext = React.createContext&lt;AuthContextValue | null&gt;(null);
</p>
<p>export function useAuth(): AuthContextValue {
<br>const ctx = React.useContext(AuthContext);
<br>if (!ctx) throw new Error(&#39;useAuth must be used within AuthProvider&#39;);
<br>return ctx;
<br>}</code></pre>
</p>
<h2>Utility Types in Practice</h2>
<pre><code>// Pick — expose subset of a larger type
<p>type UserPreview = Pick&lt;User, &#39;id&#39; | &#39;name&#39; | &#39;avatarUrl&#39;&gt;;
</p>
<p>// Omit — remove sensitive fields
<br>type PublicUser = Omit&lt;User, &#39;passwordHash&#39; | &#39;refreshToken&#39;&gt;;
</p>
<p>// Partial — all fields optional (for update payloads)
<br>type UpdateUserInput = Partial&lt;Pick&lt;User, &#39;name&#39; | &#39;email&#39; | &#39;bio&#39;&gt;&gt; &amp; { id: string };
</p>
<p>// Required — enforce all fields (for form validation output)
<br>type ValidatedForm = Required&lt;UserFormValues&gt;;
</p>
<p>// Record — typed maps
<br>const roleLabels: Record&lt;UserRole, string&gt; = {
<br>admin: &#39;Administrator&#39;,
<br>editor: &#39;Editor&#39;,
<br>viewer: &#39;Viewer&#39;,
<br>};
</p>
<p>// Template literal types for event names
<br>type EventName = <code>on${Capitalize&lt;string&gt;}</code>;</code></pre>
</p>
<h2>Ref Typing</h2>
<pre><code>// ✅ Typed refs
<p>const inputRef = useRef&lt;HTMLInputElement&gt;(null);
<br>const timerRef = useRef&lt;ReturnType&lt;typeof setTimeout&gt;&gt;(null);
<br>const prevValueRef = useRef&lt;string&gt;(&#39;&#39;);
</p>
<p>// ✅ forwardRef with generics
<br>const Input = React.forwardRef&lt;HTMLInputElement, InputProps&gt;(
<br>({ label, ...props }, ref) =&gt; (
<br>&lt;div&gt;
<br>&lt;label&gt;{label}&lt;/label&gt;
<br>&lt;input ref={ref} {...props} /&gt;
<br>&lt;/div&gt;
<br>)
<br>);
<br>Input.displayName = &#39;Input&#39;;</code></pre>
</p>
<h2>Type Guards</h2>
<pre><code>// ✅ Type guard functions
<p>function isUser(value: unknown): value is User {
<br>return (
<br>typeof value === &#39;object&#39; &amp;&amp;
<br>value !== null &amp;&amp;
<br>&#39;id&#39; in value &amp;&amp;
<br>&#39;email&#39; in value
<br>);
<br>}
</p>
<p>function isApiError(error: unknown): error is ApiError {
<br>return error instanceof ApiError;
<br>}
</p>
<p>// Usage
<br>if (isApiError(error)) {
<br>// TypeScript knows error is ApiError here
<br>toast.error(error.message);
<br>}</code></pre>
</p>
<h2>Strict Mode Checklist</h2>
<p>Ensure <code>tsconfig.json</code> has:
</p>
<pre><code>{
<p>&quot;compilerOptions&quot;: {
<br>&quot;strict&quot;: true,
<br>&quot;noUncheckedIndexedAccess&quot;: true,
<br>&quot;exactOptionalPropertyTypes&quot;: true,
<br>&quot;noImplicitReturns&quot;: true,
<br>&quot;noFallthroughCasesInSwitch&quot;: true
<br>}
<br>}</code></pre>
</p>
<h2>Verification</h2>
<ul><li>[ ] No <code>any</code> — use <code>unknown</code> and narrow</li>
<li>[ ] All hook return types explicitly typed</li>
<li>[ ] Event handlers use correct React event types</li>
<li>[ ] Discriminated unions for multi-state values</li>
<li>[ ] Context has null check in consumer hook</li>
<li>[ ] <code>strict: true</code> in tsconfig</li>
</ul>