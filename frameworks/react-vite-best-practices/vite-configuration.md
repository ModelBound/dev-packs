<hr>
<p>name: vite-configuration
<br>description: Vite 6 configuration best practices. Use when setting up a new project, optimizing builds, or debugging Vite config issues.
</p>
<hr>
<h1>Vite Configuration</h1>
<h2>Overview</h2>
<p>Vite 6 is the standard build tool for React projects. Its native ESM dev server and Rollup-based production builds require different optimization strategies. This skill covers the configuration patterns that matter in production.
</p>
<h2>Canonical vite.config.ts</h2>
<pre><code>import { defineConfig, loadEnv } from &#39;vite&#39;;
<p>import react from &#39;@vitejs/plugin-react&#39;;
<br>import { resolve } from &#39;path&#39;;
</p>
<p>export default defineConfig(({ mode }) =&gt; {
<br>const env = loadEnv(mode, process.cwd(), &#39;&#39;);
</p>
<p>return {
<br>plugins: [
<br>react({
<br>// Enable React Compiler (React 19+)
<br>babel: {
<br>plugins: [[&#39;babel-plugin-react-compiler&#39;, {}]],
<br>},
<br>}),
<br>],
</p>
<p>resolve: {
<br>alias: {
<br>&#39;@&#39;: resolve(__dirname, &#39;./src&#39;),
<br>&#39;@features&#39;: resolve(__dirname, &#39;./src/features&#39;),
<br>&#39;@shared&#39;: resolve(__dirname, &#39;./src/shared&#39;),
<br>&#39;@config&#39;: resolve(__dirname, &#39;./src/config&#39;),
<br>},
<br>},
</p>
<p>build: {
<br>target: &#39;es2022&#39;,
<br>sourcemap: mode !== &#39;production&#39;,
<br>rollupOptions: {
<br>output: {
<br>// Manual chunk splitting — keep vendor separate from app code
<br>manualChunks: {
<br>&#39;react-vendor&#39;: [&#39;react&#39;, &#39;react-dom&#39;],
<br>&#39;router&#39;: [&#39;react-router-dom&#39;],
<br>&#39;query&#39;: [&#39;@tanstack/react-query&#39;],
<br>},
<br>},
<br>},
<br>// Warn when any chunk exceeds 500KB
<br>chunkSizeWarningLimit: 500,
<br>},
</p>
<p>server: {
<br>port: 3000,
<br>strictPort: true,
<br>// Proxy API calls to avoid CORS in dev
<br>proxy: {
<br>&#39;/api&#39;: {
<br>target: env.VITE_API_URL || &#39;http://localhost:8000&#39;,
<br>changeOrigin: true,
<br>},
<br>},
<br>},
</p>
<p>test: {
<br>environment: &#39;jsdom&#39;,
<br>setupFiles: [&#39;./src/test/setup.ts&#39;],
<br>globals: true,
<br>coverage: {
<br>provider: &#39;v8&#39;,
<br>reporter: [&#39;text&#39;, &#39;lcov&#39;],
<br>exclude: [&#39;src/test/<strong>&#39;, &#39;</strong>/<em>.d.ts&#39;, &#39;</em>*/index.ts&#39;],
<br>},
<br>},
<br>};
<br>});</code></pre>
</p>
<h2>Environment Variables</h2>
<p>Never access <code>import.meta.env</code> directly in components. Create a typed config module:
</p>
<pre><code>// src/config/env.ts
<p>function requireEnv(key: string): string {
<br>const value = import.meta.env[key];
<br>if (!value) throw new Error(<code>Missing required env var: ${key}</code>);
<br>return value;
<br>}
</p>
<p>export const config = {
<br>apiUrl: requireEnv(&#39;VITE_API_URL&#39;),
<br>appEnv: import.meta.env.MODE as &#39;development&#39; | &#39;staging&#39; | &#39;production&#39;,
<br>isDev: import.meta.env.DEV,
<br>isProd: import.meta.env.PROD,
<br>} as const;</code></pre>
</p>
<h2>Code Splitting Patterns</h2>
<h3>Route-level splitting (always do this)</h3>
<pre><code>import { lazy, Suspense } from &#39;react&#39;;
<p>import { createBrowserRouter } from &#39;react-router-dom&#39;;
</p>
<p>const Dashboard = lazy(() =&gt; import(&#39;@features/dashboard/pages/Dashboard&#39;));
<br>const Settings = lazy(() =&gt; import(&#39;@features/settings/pages/Settings&#39;));
</p>
<p>export const router = createBrowserRouter([
<br>{
<br>path: &#39;/&#39;,
<br>element: &lt;AppShell /&gt;,
<br>children: [
<br>{
<br>path: &#39;dashboard&#39;,
<br>element: (
<br>&lt;Suspense fallback={&lt;PageSkeleton /&gt;}&gt;
<br>&lt;Dashboard /&gt;
<br>&lt;/Suspense&gt;
<br>),
<br>},
<br>],
<br>},
<br>]);</code></pre>
</p>
<h3>Component-level splitting (heavy components only)</h3>
<pre><code>// Only lazy-load components that are genuinely heavy (charts, editors, maps)
<p>const RichTextEditor = lazy(() =&gt; import(&#39;@shared/components/RichTextEditor&#39;));
<br>const AnalyticsChart = lazy(() =&gt; import(&#39;@features/analytics/components/Chart&#39;));</code></pre>
</p>
<h2>Bundle Analysis</h2>
<p>Add to package.json scripts:
</p>
<pre><code>{
<p>&quot;scripts&quot;: {
<br>&quot;build:analyze&quot;: &quot;vite build --mode production &amp;&amp; npx vite-bundle-visualizer&quot;
<br>}
<br>}</code></pre>
</p>
<p>Run before every major release. Target: no single chunk over 200KB gzipped.
</p>
<h2>Performance Checklist</h2>
<strong>Dev server:</strong>
<ul><li>[ ] <code>optimizeDeps.include</code> lists heavy deps that need pre-bundling</li>
<li>[ ] HMR boundary is at the feature level, not the app root</li>
</ul>
<strong>Production build:</strong>
<ul><li>[ ] <code>manualChunks</code> separates vendor from app code</li>
<li>[ ] Route-level code splitting on every page</li>
<li>[ ] <code>build.target</code> set to <code>es2022</code> (not <code>esnext</code> for broader compatibility)</li>
<li>[ ] Source maps disabled in production (<code>sourcemap: false</code>)</li>
<li>[ ] Assets fingerprinted (Vite does this by default)</li>
</ul>
<strong>Assets:</strong>
<ul><li>[ ] Images imported via <code>import</code> (not string paths) for fingerprinting</li>
<li>[ ] SVGs imported as React components via <code>vite-plugin-svgr</code></li>
<li>[ ] Fonts loaded with <code>font-display: swap</code></li>
</ul>
<h2>Common Pitfalls</h2>
<p>| Problem | Cause | Fix |
<br>|---|---|---|
<br>| Slow cold start | Too many unbundled deps | Add to <code>optimizeDeps.include</code> |
<br>| Huge vendor chunk | No manual chunking | Add <code>manualChunks</code> |
<br>| Missing env vars at runtime | Accessed before Vite replaces them | Use <code>config/env.ts</code> pattern |
<br>| HMR not working | State in module scope | Move state into React |
<br>| Path alias not resolving | Missing in <code>tsconfig.json</code> | Add to both <code>vite.config.ts</code> AND <code>tsconfig.json</code> |
</p>
<h2>tsconfig.json Path Aliases</h2>
<p>Must mirror vite.config.ts aliases:
</p>
<pre><code>{
<p>&quot;compilerOptions&quot;: {
<br>&quot;baseUrl&quot;: &quot;.&quot;,
<br>&quot;paths&quot;: {
<br>&quot;@/<em>&quot;: [&quot;src/</em>&quot;],
<br>&quot;@features/<em>&quot;: [&quot;src/features/</em>&quot;],
<br>&quot;@shared/<em>&quot;: [&quot;src/shared/</em>&quot;],
<br>&quot;@config/<em>&quot;: [&quot;src/config/</em>&quot;]
<br>}
<br>}
<br>}</code></pre>
</p>
<h2>Verification</h2>
<ul><li>[ ] <code>npm run build</code> completes without warnings</li>
<li>[ ] No chunk exceeds 500KB (check build output)</li>
<li>[ ] All env vars accessed through <code>config/env.ts</code></li>
<li>[ ] Path aliases work in both Vite and TypeScript</li>
<li>[ ] React Compiler plugin enabled</li>
</ul>