<hr>
<p>name: performance-optimization
<br>description: React + Vite performance optimization. Use when diagnosing slow renders, large bundles, or poor Core Web Vitals scores.
</p>
<hr>
<h1>Performance Optimization</h1>
<h2>Overview</h2>
<p>Measure before optimizing. Every optimization in this skill should be applied in response to a measured problem, not preemptively. The React Compiler handles most memoization automatically — manual optimization is for cases the compiler can't handle.
</p>
<h2>The Measurement-First Rule</h2>
<pre><code>1. Measure → identify the actual bottleneck
<li>Hypothesize → what change will fix it?</li>
<li>Implement → smallest possible change</li>
<li>Measure again → did it improve?</code></pre></li>
<p>Tools:
</p>
<ul><li><strong>React DevTools Profiler</strong> — find slow renders</li>
<li><strong>Chrome DevTools Performance tab</strong> — find long tasks</li>
<li><strong>Lighthouse</strong> — Core Web Vitals baseline</li>
<li><code>npm run build:analyze</code> — find large chunks</li>
</ul>
<h2>React Compiler (React 19)</h2>
<p>With React Compiler enabled, you do NOT need to manually add <code>useMemo</code>, <code>useCallback</code>, or <code>React.memo</code> for most cases. The compiler analyzes your code at build time and applies optimizations automatically.
</p>
<strong>When the compiler CAN'T help (manual optimization still needed):</strong>
<ul><li>Expensive computations that depend on external data (not props/state)</li>
<li>Components that receive new object/array references on every render from outside React</li>
<li>Third-party components that don't follow React rules</li>
</ul>
<pre><code>// ✅ Compiler handles this automatically — no useMemo needed
<p>function UserList({ users }: { users: User[] }) {
<br>const sortedUsers = users.slice().sort((a, b) =&gt; a.name.localeCompare(b.name));
<br>return &lt;ul&gt;{sortedUsers.map(u =&gt; &lt;UserItem key={u.id} user={u} /&gt;)}&lt;/ul&gt;;
<br>}
</p>
<p>// ⚠️ Manual useMemo still useful here — expensive computation from external source
<br>function DataGrid({ rawData }: { rawData: RawRow[] }) {
<br>const processedData = useMemo(
<br>() =&gt; heavyTransform(rawData), // rawData comes from outside React
<br>[rawData]
<br>);
<br>return &lt;Grid data={processedData} /&gt;;
<br>}</code></pre>
</p>
<h2>Concurrent Features</h2>
<h3>useTransition — non-urgent updates</h3>
<pre><code>import { useTransition, useState } from &#39;react&#39;;
<p>function SearchPage() {
<br>const [query, setQuery] = useState(&#39;&#39;);
<br>const [results, setResults] = useState&lt;Result[]&gt;([]);
<br>const [isPending, startTransition] = useTransition();
</p>
<p>function handleSearch(value: string) {
<br>setQuery(value); // urgent — update input immediately
<br>startTransition(() =&gt; {
<br>// non-urgent — can be interrupted by user input
<br>setResults(expensiveSearch(value));
<br>});
<br>}
</p>
<p>return (
<br>&lt;&gt;
<br>&lt;input value={query} onChange={(e) =&gt; handleSearch(e.target.value)} /&gt;
<br>{isPending ? &lt;SearchSkeleton /&gt; : &lt;ResultsList results={results} /&gt;}
<br>&lt;/&gt;
<br>);
<br>}</code></pre>
</p>
<h3>useDeferredValue — defer expensive renders</h3>
<pre><code>function FilteredList({ items, filter }: Props) {
<p>const deferredFilter = useDeferredValue(filter);
<br>const isStale = filter !== deferredFilter;
</p>
<p>const filtered = items.filter(item =&gt;
<br>item.name.toLowerCase().includes(deferredFilter.toLowerCase())
<br>);
</p>
<p>return (
<br>&lt;ul style={{ opacity: isStale ? 0.7 : 1 }}&gt;
<br>{filtered.map(item =&gt; &lt;ListItem key={item.id} item={item} /&gt;)}
<br>&lt;/ul&gt;
<br>);
<br>}</code></pre>
</p>
<h2>Virtualization</h2>
<p>For lists with 100+ items, virtualize:
</p>
<pre><code>import { useVirtualizer } from &#39;@tanstack/react-virtual&#39;;
<p>function VirtualList({ items }: { items: Item[] }) {
<br>const parentRef = useRef&lt;HTMLDivElement&gt;(null);
</p>
<p>const virtualizer = useVirtualizer({
<br>count: items.length,
<br>getScrollElement: () =&gt; parentRef.current,
<br>estimateSize: () =&gt; 60, // estimated row height in px
<br>overscan: 5,
<br>});
</p>
<p>return (
<br>&lt;div ref={parentRef} style={{ height: &#39;600px&#39;, overflow: &#39;auto&#39; }}&gt;
<br>&lt;div style={{ height: <code>${virtualizer.getTotalSize()}px</code>, position: &#39;relative&#39; }}&gt;
<br>{virtualizer.getVirtualItems().map((virtualItem) =&gt; (
<br>&lt;div
<br>key={virtualItem.key}
<br>style={{
<br>position: &#39;absolute&#39;,
<br>top: 0,
<br>left: 0,
<br>width: &#39;100%&#39;,
<br>height: <code>${virtualItem.size}px</code>,
<br>transform: <code>translateY(${virtualItem.start}px)</code>,
<br>}}
<br>&gt;
<br>&lt;ListItem item={items[virtualItem.index]} /&gt;
<br>&lt;/div&gt;
<br>))}
<br>&lt;/div&gt;
<br>&lt;/div&gt;
<br>);
<br>}</code></pre>
</p>
<h2>Image Optimization</h2>
<pre><code>// ✅ Lazy load images below the fold
<p>&lt;img
<br>src={user.avatarUrl}
<br>alt={user.name}
<br>loading=&quot;lazy&quot;
<br>decoding=&quot;async&quot;
<br>width={48}
<br>height={48}
<br>/&gt;
</p>
<p>// ✅ Use modern formats with fallback
<br>&lt;picture&gt;
<br>&lt;source srcSet={image.webp} type=&quot;image/webp&quot; /&gt;
<br>&lt;source srcSet={image.avif} type=&quot;image/avif&quot; /&gt;
<br>&lt;img src={image.jpg} alt={image.alt} loading=&quot;lazy&quot; /&gt;
<br>&lt;/picture&gt;</code></pre>
</p>
<h2>Bundle Size</h2>
<pre><code>// ✅ Dynamic import for heavy libraries
<p>const { Chart } = await import(&#39;chart.js&#39;);
</p>
<p>// ✅ Tree-shakeable imports
<br>import { format } from &#39;date-fns&#39;; // ✅ tree-shakeable
<br>import dateFns from &#39;date-fns&#39;;    // ❌ imports everything
</p>
<p>// ✅ Check bundle impact before adding a dependency
<br>// npx bundlephobia &lt;package-name&gt;</code></pre>
</p>
<h2>Core Web Vitals Targets</h2>
<p>| Metric | Target | How to improve |
<br>|---|---|---|
<br>| LCP | < 2.5s | Preload hero image, reduce TTFB |
<br>| INP | < 200ms | useTransition for heavy updates |
<br>| CLS | < 0.1 | Reserve space for images/ads |
<br>| FID | < 100ms | Break up long tasks |
</p>
<h2>Verification</h2>
<ul><li>[ ] Profiler used to identify slow renders before optimizing</li>
<li>[ ] No manual <code>useMemo</code>/<code>useCallback</code> without a measured reason</li>
<li>[ ] Lists with 100+ items use virtualization</li>
<li>[ ] Images have <code>loading="lazy"</code> and explicit dimensions</li>
<li>[ ] Route-level code splitting on every page</li>
<li>[ ] Lighthouse score > 90 on production build</li>
</ul>