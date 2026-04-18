<hr>
<p>name: state-management
<br>description: State management patterns for React apps. Use when deciding where state lives, how to share it, or when to reach for a global store.
</p>
<hr>
<h1>State Management</h1>
<h2>Overview</h2>
<p>The most common mistake in React state management is putting too much in global state. Most state should live as close to where it's used as possible. This skill covers the decision framework and implementation patterns.
</p>
<h2>The State Decision Tree</h2>
<pre><code>Is this state used by only one component?
<p>→ useState / useReducer in that component
</p>
<p>Is this state shared between a few nearby components?
<br>→ Lift to nearest common ancestor
</p>
<p>Is this state shared across a feature?
<br>→ Feature-level context or Zustand slice
</p>
<p>Is this server data (from an API)?
<br>→ TanStack Query — never put it in useState or Zustand
</p>
<p>Is this truly global UI state (theme, auth, notifications)?
<br>→ Zustand global store</code></pre>
</p>
<h2>Local State</h2>
<pre><code>// ✅ Simple value
<p>const [isOpen, setIsOpen] = useState(false);
</p>
<p>// ✅ Complex local state — useReducer
<br>type FormAction =
<br>| { type: &#39;SET_FIELD&#39;; field: string; value: string }
<br>| { type: &#39;SUBMIT&#39; }
<br>| { type: &#39;RESET&#39; };
</p>
<p>function formReducer(state: FormState, action: FormAction): FormState {
<br>switch (action.type) {
<br>case &#39;SET_FIELD&#39;:
<br>return { ...state, [action.field]: action.value };
<br>case &#39;SUBMIT&#39;:
<br>return { ...state, isSubmitting: true };
<br>case &#39;RESET&#39;:
<br>return initialFormState;
<br>}
<br>}
</p>
<p>function ComplexForm() {
<br>const [state, dispatch] = useReducer(formReducer, initialFormState);
<br>// ...
<br>}</code></pre>
</p>
<h2>Context — Feature-Level State</h2>
<pre><code>// ✅ Context for state shared within a feature tree
<p>interface CartContextValue {
<br>items: CartItem[];
<br>addItem: (product: Product, quantity: number) =&gt; void;
<br>removeItem: (productId: string) =&gt; void;
<br>total: number;
<br>}
</p>
<p>const CartContext = React.createContext&lt;CartContextValue | null&gt;(null);
</p>
<p>export function CartProvider({ children }: { children: React.ReactNode }) {
<br>const [items, setItems] = useState&lt;CartItem[]&gt;([]);
</p>
<p>const total = items.reduce((sum, item) =&gt; sum + item.price * item.quantity, 0);
</p>
<p>function addItem(product: Product, quantity: number) {
<br>setItems((prev) =&gt; {
<br>const existing = prev.find((i) =&gt; i.productId === product.id);
<br>if (existing) {
<br>return prev.map((i) =&gt;
<br>i.productId === product.id
<br>? { ...i, quantity: i.quantity + quantity }
<br>: i
<br>);
<br>}
<br>return [...prev, { productId: product.id, name: product.name, price: product.price, quantity }];
<br>});
<br>}
</p>
<p>function removeItem(productId: string) {
<br>setItems((prev) =&gt; prev.filter((i) =&gt; i.productId !== productId));
<br>}
</p>
<p>return (
<br>&lt;CartContext.Provider value={{ items, addItem, removeItem, total }}&gt;
<br>{children}
<br>&lt;/CartContext.Provider&gt;
<br>);
<br>}
</p>
<p>export function useCart(): CartContextValue {
<br>const ctx = React.useContext(CartContext);
<br>if (!ctx) throw new Error(&#39;useCart must be used within CartProvider&#39;);
<br>return ctx;
<br>}</code></pre>
</p>
<h2>Zustand — Global Client State</h2>
<p>Use Zustand only for state that is genuinely global and not server data:
</p>
<pre><code>// src/shared/store/uiStore.ts
<p>import { create } from &#39;zustand&#39;;
<br>import { devtools, persist } from &#39;zustand/middleware&#39;;
</p>
<p>interface UIState {
<br>theme: &#39;light&#39; | &#39;dark&#39; | &#39;system&#39;;
<br>sidebarOpen: boolean;
<br>setTheme: (theme: UIState[&#39;theme&#39;]) =&gt; void;
<br>toggleSidebar: () =&gt; void;
<br>}
</p>
<p>export const useUIStore = create&lt;UIState&gt;()(
<br>devtools(
<br>persist(
<br>(set) =&gt; ({
<br>theme: &#39;system&#39;,
<br>sidebarOpen: true,
<br>setTheme: (theme) =&gt; set({ theme }),
<br>toggleSidebar: () =&gt; set((state) =&gt; ({ sidebarOpen: !state.sidebarOpen })),
<br>}),
<br>{ name: &#39;ui-preferences&#39; }
<br>)
<br>)
<br>);
</p>
<p>// ✅ Select only what you need — prevents unnecessary re-renders
<br>function Header() {
<br>const theme = useUIStore((state) =&gt; state.theme);
<br>const setTheme = useUIStore((state) =&gt; state.setTheme);
<br>// ...
<br>}</code></pre>
</p>
<h2>URL State</h2>
<p>URL is underused as state. Use it for:
</p>
<ul><li>Current page/tab</li>
<li>Filter and sort values</li>
<li>Search queries</li>
<li>Modal open state (for shareable links)</li>
</ul>
<pre><code>import { useSearchParams } from &#39;react-router-dom&#39;;
<p>function UserListPage() {
<br>const [searchParams, setSearchParams] = useSearchParams();
</p>
<p>const page = Number(searchParams.get(&#39;page&#39;) ?? &#39;1&#39;);
<br>const search = searchParams.get(&#39;search&#39;) ?? &#39;&#39;;
<br>const role = searchParams.get(&#39;role&#39;) ?? &#39;all&#39;;
</p>
<p>function handleSearch(value: string) {
<br>setSearchParams((prev) =&gt; {
<br>prev.set(&#39;search&#39;, value);
<br>prev.set(&#39;page&#39;, &#39;1&#39;); // reset page on new search
<br>return prev;
<br>});
<br>}
</p>
<p>return (
<br>&lt;div&gt;
<br>&lt;SearchInput value={search} onChange={handleSearch} /&gt;
<br>&lt;UserList page={page} search={search} role={role} /&gt;
<br>&lt;/div&gt;
<br>);
<br>}</code></pre>
</p>
<h2>Form State</h2>
<p>For forms, use React Hook Form — it avoids controlled input re-renders:
</p>
<pre><code>import { useForm } from &#39;react-hook-form&#39;;
<p>import { zodResolver } from &#39;@hookform/resolvers/zod&#39;;
<br>import { z } from &#39;zod&#39;;
</p>
<p>const schema = z.object({
<br>email: z.string().email(&#39;Invalid email&#39;),
<br>password: z.string().min(8, &#39;Password must be at least 8 characters&#39;),
<br>});
</p>
<p>type FormValues = z.infer&lt;typeof schema&gt;;
</p>
<p>function LoginForm() {
<br>const {
<br>register,
<br>handleSubmit,
<br>formState: { errors, isSubmitting },
<br>} = useForm&lt;FormValues&gt;({
<br>resolver: zodResolver(schema),
<br>});
</p>
<p>async function onSubmit(data: FormValues) {
<br>await login(data);
<br>}
</p>
<p>return (
<br>&lt;form onSubmit={handleSubmit(onSubmit)}&gt;
<br>&lt;FormField label=&quot;Email&quot; error={errors.email?.message}&gt;
<br>&lt;input type=&quot;email&quot; {...register(&#39;email&#39;)} /&gt;
<br>&lt;/FormField&gt;
<br>&lt;FormField label=&quot;Password&quot; error={errors.password?.message}&gt;
<br>&lt;input type=&quot;password&quot; {...register(&#39;password&#39;)} /&gt;
<br>&lt;/FormField&gt;
<br>&lt;button type=&quot;submit&quot; disabled={isSubmitting}&gt;
<br>{isSubmitting ? &#39;Signing in…&#39; : &#39;Sign in&#39;}
<br>&lt;/button&gt;
<br>&lt;/form&gt;
<br>);
<br>}</code></pre>
</p>
<h2>Anti-Patterns</h2>
<p>| Anti-Pattern | Problem | Fix |
<br>|---|---|---|
<br>| Server data in Zustand | Stale data, double-fetching | Use TanStack Query |
<br>| Everything in global store | Hard to test, tight coupling | Colocate state |
<br>| Syncing state with <code>useEffect</code> | Race conditions, extra renders | Derive from source of truth |
<br>| Context for high-frequency updates | Performance issues | Use Zustand or local state |
<br>| Storing derived data | Gets out of sync | Compute from source |
</p>
<h2>Verification</h2>
<ul><li>[ ] Server data managed by TanStack Query, not useState/Zustand</li>
<li>[ ] State lives as close to usage as possible</li>
<li>[ ] Zustand selectors select only needed fields</li>
<li>[ ] URL used for shareable/bookmarkable state</li>
<li>[ ] Forms use React Hook Form with Zod validation</li>
</ul>