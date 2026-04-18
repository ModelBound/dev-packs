<hr>
<p>name: data-fetching-patterns
<br>description: Server state management with TanStack Query and React 19 patterns. Use when implementing data fetching, mutations, caching, or optimistic updates.
</p>
<hr>
<h1>Data Fetching Patterns</h1>
<h2>Overview</h2>
<p>Server state (data from APIs) is fundamentally different from client state (UI state). TanStack Query v5 manages server state. Zustand or <code>useState</code> manages client state. Never mix them.
</p>
<h2>TanStack Query Setup</h2>
<pre><code>// src/app/providers.tsx
<p>import { QueryClient, QueryClientProvider } from &#39;@tanstack/react-query&#39;;
<br>import { ReactQueryDevtools } from &#39;@tanstack/react-query-devtools&#39;;
</p>
<p>const queryClient = new QueryClient({
<br>defaultOptions: {
<br>queries: {
<br>staleTime: 1000 <em> 60 </em> 5,      // 5 minutes
<br>gcTime: 1000 <em> 60 </em> 10,         // 10 minutes
<br>retry: (failureCount, error) =&gt; {
<br>// Don&#39;t retry on 4xx errors
<br>if (error instanceof ApiError &amp;&amp; error.status &lt; 500) return false;
<br>return failureCount &lt; 2;
<br>},
<br>refetchOnWindowFocus: false,
<br>},
<br>mutations: {
<br>onError: (error) =&gt; {
<br>// Global error handler — toast, log, etc.
<br>logger.error(&#39;Mutation failed&#39;, error);
<br>},
<br>},
<br>},
<br>});
</p>
<p>export function Providers({ children }: { children: React.ReactNode }) {
<br>return (
<br>&lt;QueryClientProvider client={queryClient}&gt;
<br>{children}
<br>{import.meta.env.DEV &amp;&amp; &lt;ReactQueryDevtools /&gt;}
<br>&lt;/QueryClientProvider&gt;
<br>);
<br>}</code></pre>
</p>
<h2>Query Key Factory Pattern</h2>
<p>Centralize query keys to avoid typos and enable precise invalidation:
</p>
<pre><code>// src/features/users/api/queryKeys.ts
<p>export const userKeys = {
<br>all: [&#39;users&#39;] as const,
<br>lists: () =&gt; [...userKeys.all, &#39;list&#39;] as const,
<br>list: (filters: UserFilters) =&gt; [...userKeys.lists(), filters] as const,
<br>details: () =&gt; [...userKeys.all, &#39;detail&#39;] as const,
<br>detail: (id: string) =&gt; [...userKeys.details(), id] as const,
<br>};
</p>
<p>// Usage — precise invalidation after mutation
<br>queryClient.invalidateQueries({ queryKey: userKeys.lists() });</code></pre>
</p>
<h2>Query Hooks</h2>
<pre><code>// src/features/users/hooks/useUsers.ts
<p>import { useQuery, useSuspenseQuery } from &#39;@tanstack/react-query&#39;;
<br>import { userKeys } from &#39;../api/queryKeys&#39;;
<br>import { fetchUsers } from &#39;../api/usersApi&#39;;
</p>
<p>// Standard query (handles loading/error in component)
<br>export function useUsers(filters: UserFilters) {
<br>return useQuery({
<br>queryKey: userKeys.list(filters),
<br>queryFn: () =&gt; fetchUsers(filters),
<br>select: (data) =&gt; data.users, // transform at the query level
<br>});
<br>}
</p>
<p>// Suspense query (throws to nearest Suspense boundary)
<br>export function useSuspenseUsers(filters: UserFilters) {
<br>return useSuspenseQuery({
<br>queryKey: userKeys.list(filters),
<br>queryFn: () =&gt; fetchUsers(filters),
<br>});
<br>}</code></pre>
</p>
<h2>Mutation Hooks</h2>
<pre><code>// src/features/users/hooks/useUpdateUser.ts
<p>import { useMutation, useQueryClient } from &#39;@tanstack/react-query&#39;;
<br>import { userKeys } from &#39;../api/queryKeys&#39;;
<br>import { updateUser } from &#39;../api/usersApi&#39;;
</p>
<p>export function useUpdateUser() {
<br>const queryClient = useQueryClient();
</p>
<p>return useMutation({
<br>mutationFn: updateUser,
</p>
<p>// Optimistic update
<br>onMutate: async (variables) =&gt; {
<br>await queryClient.cancelQueries({ queryKey: userKeys.detail(variables.id) });
<br>const previous = queryClient.getQueryData(userKeys.detail(variables.id));
<br>queryClient.setQueryData(userKeys.detail(variables.id), (old: User) =&gt; ({
<br>...old,
<br>...variables,
<br>}));
<br>return { previous };
<br>},
</p>
<p>onError: (err, variables, context) =&gt; {
<br>// Roll back on error
<br>if (context?.previous) {
<br>queryClient.setQueryData(userKeys.detail(variables.id), context.previous);
<br>}
<br>},
</p>
<p>onSettled: (data, error, variables) =&gt; {
<br>// Always refetch after mutation settles
<br>queryClient.invalidateQueries({ queryKey: userKeys.detail(variables.id) });
<br>},
<br>});
<br>}</code></pre>
</p>
<h2>Suspense Boundaries</h2>
<p>Wrap data-fetching components in Suspense + ErrorBoundary:
</p>
<pre><code>import { Suspense } from &#39;react&#39;;
<p>import { ErrorBoundary } from &#39;react-error-boundary&#39;;
</p>
<p>function UserListPage() {
<br>return (
<br>&lt;ErrorBoundary
<br>fallback={&lt;ErrorMessage message=&quot;Failed to load users&quot; /&gt;}
<br>onError={(error) =&gt; logger.error(&#39;UserList error&#39;, error)}
<br>&gt;
<br>&lt;Suspense fallback={&lt;UserListSkeleton /&gt;}&gt;
<br>&lt;UserList /&gt;
<br>&lt;/Suspense&gt;
<br>&lt;/ErrorBoundary&gt;
<br>);
<br>}
</p>
<p>// UserList uses useSuspenseQuery — no loading state needed
<br>function UserList() {
<br>const { data: users } = useSuspenseUsers({ page: 1 });
<br>return &lt;ul&gt;{users.map(u =&gt; &lt;UserItem key={u.id} user={u} /&gt;)}&lt;/ul&gt;;
<br>}</code></pre>
</p>
<h2>API Layer</h2>
<p>Keep API calls in a dedicated module, never in components or hooks directly:
</p>
<pre><code>// src/features/users/api/usersApi.ts
<p>import { apiClient } from &#39;@shared/utils/apiClient&#39;;
<br>import type { User, UserFilters, UpdateUserInput } from &#39;../types&#39;;
</p>
<p>export async function fetchUsers(filters: UserFilters): Promise&lt;{ users: User[]; total: number }&gt; {
<br>return apiClient.get(&#39;/users&#39;, { params: filters });
<br>}
</p>
<p>export async function updateUser(input: UpdateUserInput): Promise&lt;User&gt; {
<br>return apiClient.patch(<code>/users/${input.id}</code>, input);
<br>}</code></pre>
</p>
<pre><code>// src/shared/utils/apiClient.ts
<p>import axios from &#39;axios&#39;;
<br>import { config } from &#39;@config/env&#39;;
</p>
<p>export const apiClient = axios.create({
<br>baseURL: config.apiUrl,
<br>headers: { &#39;Content-Type&#39;: &#39;application/json&#39; },
<br>});
</p>
<p>// Auth interceptor
<br>apiClient.interceptors.request.use((req) =&gt; {
<br>const token = getAuthToken();
<br>if (token) req.headers.Authorization = <code>Bearer ${token}</code>;
<br>return req;
<br>});
</p>
<p>// Error normalization
<br>apiClient.interceptors.response.use(
<br>(res) =&gt; res.data,
<br>(error) =&gt; Promise.reject(new ApiError(error))
<br>);</code></pre>
</p>
<h2>Pagination</h2>
<pre><code>export function useInfiniteUsers(filters: Omit&lt;UserFilters, &#39;page&#39;&gt;) {
<p>return useInfiniteQuery({
<br>queryKey: userKeys.list(filters),
<br>queryFn: ({ pageParam }) =&gt; fetchUsers({ ...filters, page: pageParam }),
<br>initialPageParam: 1,
<br>getNextPageParam: (lastPage, allPages) =&gt;
<br>lastPage.users.length === filters.limit ? allPages.length + 1 : undefined,
<br>select: (data) =&gt; ({
<br>users: data.pages.flatMap((p) =&gt; p.users),
<br>total: data.pages[0]?.total ?? 0,
<br>}),
<br>});
<br>}</code></pre>
</p>
<h2>Verification</h2>
<ul><li>[ ] Query keys use the factory pattern</li>
<li>[ ] Mutations invalidate relevant queries on settle</li>
<li>[ ] Optimistic updates roll back on error</li>
<li>[ ] All data-fetching components wrapped in Suspense + ErrorBoundary</li>
<li>[ ] API calls isolated in <code>api/</code> module, not in hooks or components</li>
<li>[ ] No <code>useEffect</code> for data fetching</li>
</ul>