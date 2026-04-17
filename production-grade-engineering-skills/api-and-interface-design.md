<hr>
<p>name: api-and-interface-design
<br>description: Contract-first API design. Use when designing APIs, module boundaries, or public interfaces.
</p>
<hr>
<h1>API and Interface Design</h1>
<h2>Overview</h2>
<p>Contract-first design — define the interface before implementing it. APIs are promises. Once published, they're hard to change. Design for the caller, not the implementer.
</p>
<h2>When to Use</h2>
<ul><li>Designing a new API endpoint</li>
<li>Defining a module's public interface</li>
<li>Creating shared types between services</li>
<li>Any boundary that will be consumed by others</li>
</ul>
<h2>Contract-First Process</h2>
<h3>Step 1: Define the Contract</h3>
<p>Before writing any implementation:
</p>
<pre><code>// Define the interface first
<p>interface CreateTaskRequest {
<br>title: string;
<br>assigneeId?: string;
<br>dueDate?: string; // ISO 8601
<br>}
</p>
<p>interface CreateTaskResponse {
<br>id: string;
<br>title: string;
<br>status: &#39;pending&#39; | &#39;in_progress&#39; | &#39;completed&#39;;
<br>createdAt: string;
<br>}</code></pre>
</p>
<h3>Step 2: Validate the Contract</h3>
<p>Ask before implementing:
</p>
<ul><li>Is this the right abstraction for callers?</li>
<li>What will callers need to do with this data?</li>
<li>What error cases need to be represented?</li>
<li>Is this consistent with existing APIs?</li>
</ul>
<h3>Step 3: Implement Against the Contract</h3>
<p>Write tests against the contract first, then implement.
</p>
<h2>Hyrum's Law</h2>
<blockquote><p>With a sufficient number of users of an API, it does not matter what you promise in the contract: all observable behaviors of your system will be depended on by somebody.</p></blockquote>
<p>Design implications:
</p>
<ul><li>Be explicit about what's guaranteed vs. implementation detail</li>
<li>Version APIs when behavior changes</li>
<li>Don't expose internal state — callers will depend on it</li>
</ul>
<h2>The One-Version Rule</h2>
<p>Maintain one version of an API at a time. When you need to change:
</p>
<li>Add the new behavior alongside the old</li>
<li>Migrate callers to the new behavior</li>
<li>Remove the old behavior only when all callers are migrated</li>
<p>Never break existing callers without a migration path.
</p>
<h2>Error Semantics</h2>
<p>Errors are part of the contract. Design them explicitly:
</p>
<pre><code>// Good: Typed, specific errors
<p>type TaskError =
<br>| { code: &#39;NOT_FOUND&#39;; taskId: string }
<br>| { code: &#39;FORBIDDEN&#39;; reason: string }
<br>| { code: &#39;VALIDATION_ERROR&#39;; fields: Record&lt;string, string&gt; };
</p>
<p>// Bad: Generic errors that callers can&#39;t handle programmatically
<br>throw new Error(&#39;Something went wrong&#39;);</code></pre>
</p>
<h2>Boundary Validation</h2>
<p>Validate at every boundary where untrusted data enters:
</p>
<ul><li>HTTP request bodies and query params</li>
<li>Function arguments from external callers</li>
<li>Data from external APIs and databases</li>
</ul>
<p>Don't trust data that crosses a boundary — validate it.
</p>
<h2>Common Rationalizations</h2>
<p>| Rationalization | Reality |
<br>|---|---|
<br>| "I'll define the interface after I implement it" | Implementation-first APIs are shaped by implementation details, not caller needs. |
<br>| "I'll add versioning later" | By the time you need versioning, you have callers depending on the current behavior. |
</p>
<h2>Verification</h2>
<ul><li>[ ] Interface defined before implementation</li>
<li>[ ] Error cases are typed and explicit</li>
<li>[ ] Validation at all entry points</li>
<li>[ ] Consistent with existing API conventions</li>
<li>[ ] Breaking changes have a migration path</li>
</ul>