<hr>
<p>name: testing-react-vite
<br>description: Testing strategy for React + Vite projects. Use when writing unit tests, integration tests, or setting up the test infrastructure.
</p>
<hr>
<h1>Testing React + Vite</h1>
<h2>Overview</h2>
<p>Tests in React projects fall into three categories: unit tests for pure logic, integration tests for component behavior, and E2E tests for critical user flows. This skill covers the patterns that make tests reliable and maintainable.
</p>
<h2>Setup</h2>
<pre><code>// src/test/setup.ts
<p>import &#39;@testing-library/jest-dom&#39;;
<br>import { cleanup } from &#39;@testing-library/react&#39;;
<br>import { afterEach, vi } from &#39;vitest&#39;;
</p>
<p>afterEach(() =&gt; {
<br>cleanup();
<br>vi.clearAllMocks();
<br>});
</p>
<p>// Mock IntersectionObserver (not in jsdom)
<br>global.IntersectionObserver = vi.fn().mockImplementation(() =&gt; ({
<br>observe: vi.fn(),
<br>unobserve: vi.fn(),
<br>disconnect: vi.fn(),
<br>}));</code></pre>
</p>
<pre><code>// src/test/renderWithProviders.tsx
<p>import { render, type RenderOptions } from &#39;@testing-library/react&#39;;
<br>import { QueryClient, QueryClientProvider } from &#39;@tanstack/react-query&#39;;
<br>import { MemoryRouter } from &#39;react-router-dom&#39;;
</p>
<p>function createTestQueryClient() {
<br>return new QueryClient({
<br>defaultOptions: {
<br>queries: { retry: false, gcTime: 0 },
<br>mutations: { retry: false },
<br>},
<br>});
<br>}
</p>
<p>interface TestOptions extends Omit&lt;RenderOptions, &#39;wrapper&#39;&gt; {
<br>initialRoute?: string;
<br>}
</p>
<p>export function renderWithProviders(ui: React.ReactElement, options: TestOptions = {}) {
<br>const { initialRoute = &#39;/&#39;, ...renderOptions } = options;
<br>const queryClient = createTestQueryClient();
</p>
<p>function Wrapper({ children }: { children: React.ReactNode }) {
<br>return (
<br>&lt;QueryClientProvider client={queryClient}&gt;
<br>&lt;MemoryRouter initialEntries={[initialRoute]}&gt;
<br>{children}
<br>&lt;/MemoryRouter&gt;
<br>&lt;/QueryClientProvider&gt;
<br>);
<br>}
</p>
<p>return { ...render(ui, { wrapper: Wrapper, ...renderOptions }), queryClient };
<br>}</code></pre>
</p>
<h2>Component Tests</h2>
<p>Test behavior, not implementation:
</p>
<pre><code>// src/features/auth/components/LoginForm.test.tsx
<p>import { screen, waitFor } from &#39;@testing-library/react&#39;;
<br>import userEvent from &#39;@testing-library/user-event&#39;;
<br>import { http, HttpResponse } from &#39;msw&#39;;
<br>import { server } from &#39;@/test/server&#39;;
<br>import { renderWithProviders } from &#39;@/test/renderWithProviders&#39;;
<br>import { LoginForm } from &#39;./LoginForm&#39;;
</p>
<p>describe(&#39;LoginForm&#39;, () =&gt; {
<br>it(&#39;submits credentials and redirects on success&#39;, async () =&gt; {
<br>const user = userEvent.setup();
<br>server.use(
<br>http.post(&#39;/api/auth/login&#39;, () =&gt;
<br>HttpResponse.json({ token: &#39;abc123&#39;, user: { id: &#39;1&#39;, email: &#39;test@example.com&#39; } })
<br>)
<br>);
</p>
<p>renderWithProviders(&lt;LoginForm /&gt;);
</p>
<p>await user.type(screen.getByLabelText(&#39;Email&#39;), &#39;test@example.com&#39;);
<br>await user.type(screen.getByLabelText(&#39;Password&#39;), &#39;password123&#39;);
<br>await user.click(screen.getByRole(&#39;button&#39;, { name: &#39;Sign in&#39; }));
</p>
<p>await waitFor(() =&gt; {
<br>expect(screen.queryByRole(&#39;button&#39;, { name: &#39;Signing in…&#39; })).not.toBeInTheDocument();
<br>});
<br>});
</p>
<p>it(&#39;shows field-level errors on invalid submission&#39;, async () =&gt; {
<br>const user = userEvent.setup();
<br>renderWithProviders(&lt;LoginForm /&gt;);
</p>
<p>await user.click(screen.getByRole(&#39;button&#39;, { name: &#39;Sign in&#39; }));
</p>
<p>expect(await screen.findByRole(&#39;alert&#39;, { name: /email is required/i })).toBeInTheDocument();
<br>});
</p>
<p>it(&#39;shows server error message on failed login&#39;, async () =&gt; {
<br>const user = userEvent.setup();
<br>server.use(
<br>http.post(&#39;/api/auth/login&#39;, () =&gt;
<br>HttpResponse.json({ error: &#39;Invalid credentials&#39; }, { status: 401 })
<br>)
<br>);
</p>
<p>renderWithProviders(&lt;LoginForm /&gt;);
<br>await user.type(screen.getByLabelText(&#39;Email&#39;), &#39;wrong@example.com&#39;);
<br>await user.type(screen.getByLabelText(&#39;Password&#39;), &#39;wrongpassword&#39;);
<br>await user.click(screen.getByRole(&#39;button&#39;, { name: &#39;Sign in&#39; }));
</p>
<p>expect(await screen.findByRole(&#39;alert&#39;)).toHaveTextContent(&#39;Invalid credentials&#39;);
<br>});
<br>});</code></pre>
</p>
<h2>Hook Tests</h2>
<pre><code>// src/features/users/hooks/useCounter.test.ts
<p>import { renderHook, act } from &#39;@testing-library/react&#39;;
<br>import { useCounter } from &#39;./useCounter&#39;;
</p>
<p>describe(&#39;useCounter&#39;, () =&gt; {
<br>it(&#39;initializes with default value&#39;, () =&gt; {
<br>const { result } = renderHook(() =&gt; useCounter());
<br>expect(result.current.count).toBe(0);
<br>});
</p>
<p>it(&#39;increments count&#39;, () =&gt; {
<br>const { result } = renderHook(() =&gt; useCounter(5));
<br>act(() =&gt; result.current.increment());
<br>expect(result.current.count).toBe(6);
<br>});
</p>
<p>it(&#39;resets to initial value&#39;, () =&gt; {
<br>const { result } = renderHook(() =&gt; useCounter(10));
<br>act(() =&gt; result.current.increment());
<br>act(() =&gt; result.current.reset());
<br>expect(result.current.count).toBe(10);
<br>});
<br>});</code></pre>
</p>
<h2>MSW for API Mocking</h2>
<pre><code>// src/test/handlers.ts
<p>import { http, HttpResponse } from &#39;msw&#39;;
</p>
<p>export const handlers = [
<br>http.get(&#39;/api/users&#39;, () =&gt;
<br>HttpResponse.json({ users: [{ id: &#39;1&#39;, name: &#39;Alice&#39; }], total: 1 })
<br>),
<br>http.post(&#39;/api/users&#39;, async ({ request }) =&gt; {
<br>const body = await request.json();
<br>return HttpResponse.json({ id: &#39;2&#39;, ...body }, { status: 201 });
<br>}),
<br>];
</p>
<p>// src/test/server.ts
<br>import { setupServer } from &#39;msw/node&#39;;
<br>import { handlers } from &#39;./handlers&#39;;
</p>
<p>export const server = setupServer(...handlers);
</p>
<p>beforeAll(() =&gt; server.listen({ onUnhandledRequest: &#39;error&#39; }));
<br>afterEach(() =&gt; server.resetHandlers());
<br>afterAll(() =&gt; server.close());</code></pre>
</p>
<h2>Accessibility Testing</h2>
<pre><code>import { axe, toHaveNoViolations } from &#39;jest-axe&#39;;
<p>expect.extend(toHaveNoViolations);
</p>
<p>it(&#39;has no accessibility violations&#39;, async () =&gt; {
<br>const { container } = renderWithProviders(&lt;UserProfile user={mockUser} /&gt;);
<br>const results = await axe(container);
<br>expect(results).toHaveNoViolations();
<br>});</code></pre>
</p>
<h2>What to Test</h2>
<p>| Layer | Test | Tool |
<br>|---|---|---|
<br>| Pure functions | Unit test | Vitest |
<br>| Custom hooks | Hook test | Testing Library |
<br>| Components | Integration test | Testing Library + MSW |
<br>| Forms | Interaction test | userEvent |
<br>| Accessibility | Axe audit | jest-axe |
<br>| Critical flows | E2E | Playwright |
</p>
<h2>What NOT to Test</h2>
<ul><li>Implementation details (internal state, private methods)</li>
<li>Third-party library behavior</li>
<li>Styling (use visual regression tools instead)</li>
<li>TypeScript types (the compiler handles this)</li>
</ul>
<h2>Naming Convention</h2>
<pre><code>describe(&#39;ComponentName&#39;, () =&gt; {
<p>it(&#39;does X when Y&#39;, ...)           // behavior description
<br>it(&#39;shows error when Z fails&#39;, ...) // error case
<br>it(&#39;calls onX when user does Y&#39;, ...) // callback verification
<br>})</code></pre>
</p>
<h2>Verification</h2>
<ul><li>[ ] Tests use <code>renderWithProviders</code> not bare <code>render</code></li>
<li>[ ] API calls mocked with MSW, not <code>vi.mock</code></li>
<li>[ ] Tests query by role/label, not by class or test-id</li>
<li>[ ] No <code>waitFor</code> wrapping non-async assertions</li>
<li>[ ] Accessibility test on every new page component</li>
</ul>