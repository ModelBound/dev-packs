<h1>React + Vite Project Rules</h1>
<h2>Tech Stack</h2>
<ul><li><strong>React 19</strong> with React Compiler enabled</li>
<li><strong>Vite 6</strong> as build tool and dev server</li>
<li><strong>TypeScript 5.x</strong> in strict mode</li>
<li><strong>React Router v7</strong> for routing</li>
<li><strong>TanStack Query v5</strong> for server state</li>
<li><strong>Zustand</strong> for client-only global state (when needed)</li>
<li><strong>Vitest + Testing Library</strong> for unit/integration tests</li>
<li><strong>Playwright</strong> for E2E tests</li>
</ul>
<h2>Commands</h2>
<pre><code>npm run dev          # Vite dev server with HMR
<p>npm run build        # TypeScript check + Vite production build
<br>npm run preview      # Preview production build locally
<br>npm run test         # Vitest in watch mode
<br>npm run test:run     # Vitest single run (CI)
<br>npm run test:e2e     # Playwright E2E
<br>npm run lint         # ESLint
<br>npm run typecheck    # tsc --noEmit</code></pre>
</p>
<h2>Project Structure</h2>
<pre><code>src/
<p>├── app/                    # App shell, router, providers
<br>│   ├── App.tsx
<br>│   ├── router.tsx
<br>│   └── providers.tsx
<br>├── features/               # Feature modules (colocated)
<br>│   └── [feature]/
<br>│       ├── components/     # Feature-specific components
<br>│       ├── hooks/          # Feature-specific hooks
<br>│       ├── api/            # API calls for this feature
<br>│       ├── store/          # Feature-level state (if needed)
<br>│       ├── types.ts        # Feature types
<br>│       └── index.ts        # Public API of the feature
<br>├── shared/                 # Truly shared across features
<br>│   ├── components/         # Generic UI components
<br>│   ├── hooks/              # Generic hooks
<br>│   ├── utils/              # Pure utility functions
<br>│   └── types/              # Shared type definitions
<br>├── config/                 # Typed env config, constants
<br>├── styles/                 # Global styles, design tokens
<br>└── main.tsx                # Entry point</code></pre>
</p>
<h2>Code Style</h2>
<pre><code>// ✅ Named exports for components
<p>export function UserCard({ user }: UserCardProps) { ... }
</p>
<p>// ✅ Interface for props, not inline type
<br>interface UserCardProps {
<br>user: User;
<br>onSelect?: (id: string) =&gt; void;
<br>}
</p>
<p>// ✅ Explicit return types on hooks
<br>function useUser(id: string): UseUserResult { ... }
</p>
<p>// ✅ Const assertions for static data
<br>const ROLES = [&#39;admin&#39;, &#39;editor&#39;, &#39;viewer&#39;] as const;
<br>type Role = typeof ROLES[number];</code></pre>
</p>
<h2>Boundaries</h2>
<strong>Always:</strong>
<ul><li>Run <code>npm run typecheck</code> before committing</li>
<li>Add error boundaries around async feature boundaries</li>
<li>Use semantic HTML elements</li>
<li>Test custom hooks in isolation</li>
</ul>
<strong>Ask first:</strong>
<ul><li>Adding a new dependency (check bundle impact first)</li>
<li>Changing the folder structure</li>
<li>Adding a new global store slice</li>
</ul>
<strong>Never:</strong>
<ul><li>Use <code>any</code> — use <code>unknown</code> and narrow it</li>
<li>Import from <code>../../../</code> more than 2 levels — use path aliases</li>
<li>Put business logic in components — extract to hooks or utils</li>
<li>Use <code>useEffect</code> for derived state — compute it inline</li>
<li>Disable ESLint rules without a comment explaining why</li>
</ul>