<hr>
<p>name: react-component-patterns
<br>description: Patterns for writing production-grade React components. Use when creating new components, refactoring existing ones, or reviewing component design.
</p>
<hr>
<h1>React Component Patterns</h1>
<h2>Overview</h2>
<p>Modern React components are functional, composable, and typed. This skill covers the patterns that separate production-grade components from prototype-quality ones.
</p>
<h2>Component Anatomy</h2>
<p>Every component follows this structure:
</p>
<pre><code>// 1. Imports — external, then internal, then types
<p>import { useState } from &#39;react&#39;;
<br>import { Button } from &#39;@/shared/components/Button&#39;;
<br>import type { User } from &#39;@/shared/types&#39;;
</p>
<p>// 2. Types — props interface before the component
<br>interface UserProfileProps {
<br>user: User;
<br>isEditable?: boolean;
<br>onSave?: (updated: User) =&gt; void;
<br>}
</p>
<p>// 3. Component — named export, explicit props type
<br>export function UserProfile({ user, isEditable = false, onSave }: UserProfileProps) {
<br>// 4. Hooks first, in order: state, refs, context, custom hooks
<br>const [isEditing, setIsEditing] = useState(false);
<br>const { mutate, isPending } = useUpdateUser();
</p>
<p>// 5. Derived values — computed inline, not in state
<br>const displayName = <code>${user.firstName} ${user.lastName}</code>;
<br>const canEdit = isEditable &amp;&amp; !isPending;
</p>
<p>// 6. Event handlers — named with handle prefix
<br>function handleSave(data: Partial&lt;User&gt;) {
<br>mutate({ id: user.id, ...data }, {
<br>onSuccess: (updated) =&gt; {
<br>setIsEditing(false);
<br>onSave?.(updated);
<br>},
<br>});
<br>}
</p>
<p>// 7. Early returns for loading/error/empty states
<br>if (!user) return null;
</p>
<p>// 8. JSX — semantic HTML, no unnecessary divs
<br>return (
<br>&lt;article aria-label={<code>Profile for ${displayName}</code>}&gt;
<br>&lt;h2&gt;{displayName}&lt;/h2&gt;
<br>{isEditing ? (
<br>&lt;UserEditForm user={user} onSave={handleSave} isPending={isPending} /&gt;
<br>) : (
<br>&lt;UserDetails user={user} /&gt;
<br>)}
<br>{canEdit &amp;&amp; (
<br>&lt;Button onClick={() =&gt; setIsEditing(true)}&gt;Edit Profile&lt;/Button&gt;
<br>)}
<br>&lt;/article&gt;
<br>);
<br>}</code></pre>
</p>
<h2>Composition Patterns</h2>
<h3>Compound Components</h3>
<p>Use when a parent and children share implicit state:
</p>
<pre><code>// ✅ Compound component — clean consumer API
<p>&lt;Tabs defaultValue=&quot;overview&quot;&gt;
<br>&lt;Tabs.List&gt;
<br>&lt;Tabs.Trigger value=&quot;overview&quot;&gt;Overview&lt;/Tabs.Trigger&gt;
<br>&lt;Tabs.Trigger value=&quot;settings&quot;&gt;Settings&lt;/Tabs.Trigger&gt;
<br>&lt;/Tabs.List&gt;
<br>&lt;Tabs.Panel value=&quot;overview&quot;&gt;&lt;Overview /&gt;&lt;/Tabs.Panel&gt;
<br>&lt;Tabs.Panel value=&quot;settings&quot;&gt;&lt;Settings /&gt;&lt;/Tabs.Panel&gt;
<br>&lt;/Tabs&gt;</code></pre>
</p>
<h3>Render Props / Children as Function</h3>
<p>Use when the parent controls data, consumer controls rendering:
</p>
<pre><code>function DataList&lt;T&gt;({ items, renderItem }: DataListProps&lt;T&gt;) {
<p>return (
<br>&lt;ul&gt;
<br>{items.map((item, i) =&gt; (
<br>&lt;li key={i}&gt;{renderItem(item)}&lt;/li&gt;
<br>))}
<br>&lt;/ul&gt;
<br>);
<br>}
</p>
<p>// Usage
<br>&lt;DataList items={users} renderItem={(user) =&gt; &lt;UserCard user={user} /&gt;} /&gt;</code></pre>
</p>
<h3>Controlled vs Uncontrolled</h3>
<p>Prefer controlled for forms that need validation or submission. Use uncontrolled only for simple, isolated inputs.
</p>
<pre><code>// ✅ Controlled — parent owns the value
<p>function SearchInput({ value, onChange }: SearchInputProps) {
<br>return (
<br>&lt;input
<br>type=&quot;search&quot;
<br>value={value}
<br>onChange={(e) =&gt; onChange(e.target.value)}
<br>aria-label=&quot;Search&quot;
<br>/&gt;
<br>);
<br>}</code></pre>
</p>
<h2>React 19 Patterns</h2>
<h3>useActionState for Forms</h3>
<p>Replace manual loading/error state with <code>useActionState</code>:
</p>
<pre><code>import { useActionState } from &#39;react&#39;;
<p>function LoginForm() {
<br>const [state, submitAction, isPending] = useActionState(
<br>async (prevState: FormState, formData: FormData) =&gt; {
<br>const result = await login({
<br>email: formData.get(&#39;email&#39;) as string,
<br>password: formData.get(&#39;password&#39;) as string,
<br>});
<br>if (!result.ok) return { error: result.error };
<br>return { success: true };
<br>},
<br>null
<br>);
</p>
<p>return (
<br>&lt;form action={submitAction}&gt;
<br>&lt;input name=&quot;email&quot; type=&quot;email&quot; required /&gt;
<br>&lt;input name=&quot;password&quot; type=&quot;password&quot; required /&gt;
<br>{state?.error &amp;&amp; &lt;p role=&quot;alert&quot;&gt;{state.error}&lt;/p&gt;}
<br>&lt;button type=&quot;submit&quot; disabled={isPending}&gt;
<br>{isPending ? &#39;Signing in…&#39; : &#39;Sign in&#39;}
<br>&lt;/button&gt;
<br>&lt;/form&gt;
<br>);
<br>}</code></pre>
</p>
<h3>useOptimistic for Instant UI</h3>
<pre><code>function TodoList({ todos }: { todos: Todo[] }) {
<p>const [optimisticTodos, addOptimistic] = useOptimistic(
<br>todos,
<br>(state, newTodo: Todo) =&gt; [...state, newTodo]
<br>);
</p>
<p>async function handleAdd(text: string) {
<br>const tempTodo = { id: crypto.randomUUID(), text, done: false };
<br>addOptimistic(tempTodo);
<br>await createTodo(text); // actual API call
<br>}
</p>
<p>return &lt;ul&gt;{optimisticTodos.map(t =&gt; &lt;TodoItem key={t.id} todo={t} /&gt;)}&lt;/ul&gt;;
<br>}</code></pre>
</p>
<h3>use() for Async Resources</h3>
<pre><code>import { use, Suspense } from &#39;react&#39;;
<p>function UserDetails({ userPromise }: { userPromise: Promise&lt;User&gt; }) {
<br>const user = use(userPromise); // suspends until resolved
<br>return &lt;div&gt;{user.name}&lt;/div&gt;;
<br>}
</p>
<p>// Wrap in Suspense at the boundary
<br>&lt;Suspense fallback={&lt;UserSkeleton /&gt;}&gt;
<br>&lt;UserDetails userPromise={fetchUser(id)} /&gt;
<br>&lt;/Suspense&gt;</code></pre>
</p>
<h2>Anti-Patterns to Avoid</h2>
<p>| Anti-Pattern | Problem | Fix |
<br>|---|---|---|
<br>| Deriving state with <code>useEffect</code> + <code>setState</code> | Creates extra render cycle | Compute inline or use <code>useMemo</code> |
<br>| Prop drilling 3+ levels | Tight coupling | Lift to context or feature store |
<br>| <code>useEffect</code> for event handling | Runs after render | Use event handlers directly |
<br>| Index as key in dynamic lists | Breaks reconciliation | Use stable unique IDs |
<br>| Storing derived data in state | State gets out of sync | Derive from source of truth |
<br>| <code>any</code> for event types | Loses type safety | Use <code>React.ChangeEvent<HTMLInputElement></code> etc. |
</p>
<h2>Verification</h2>
<ul><li>[ ] Props interface defined before component</li>
<li>[ ] No derived state — computed inline</li>
<li>[ ] Event handlers named with <code>handle</code> prefix</li>
<li>[ ] Semantic HTML used (no <code>div</code> soup)</li>
<li>[ ] Loading, error, and empty states handled</li>
<li>[ ] Component does one thing</li>
</ul>