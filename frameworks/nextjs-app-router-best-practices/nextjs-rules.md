# Next.js App Router Rules

## File Conventions

- `page.tsx` — route UI
- `layout.tsx` — shared shell
- `loading.tsx` — Suspense fallback for the segment
- `error.tsx` — error boundary for the segment (must be a Client Component)
- `not-found.tsx` — 404 UI
- `route.ts` — HTTP handler (REST endpoint)

## Server vs Client

- Files default to Server Components.
- `"use client"` directive at the top of files that need React state, effects, or browser APIs.
- Push `"use client"` as far down the tree as possible.
- Server Components can import Client Components, but not vice versa for component code (props are fine, must be serializable).

## Data Fetching

- Use `fetch()` directly in Server Components. Next dedupes within a request.
- For per-user data: `fetch(url, { cache: "no-store" })`.
- For shared data with TTL: `fetch(url, { next: { revalidate: 300 } })`.
- For tag-based invalidation: `{ next: { tags: ["invoices"] } }` + `revalidateTag("invoices")`.

## Mutations

- Use Server Actions for all data mutations.
- Mark with `"use server"` either as a directive in a server-only file or inline in an async function.
- Always call `revalidatePath()` or `revalidateTag()` after a successful mutation.
- Validate inputs with zod inside the action; never trust the form payload.

## Environment Variables

- Server-only: any `process.env.X` not prefixed `NEXT_PUBLIC_`.
- Public (sent to client): `NEXT_PUBLIC_X`. Treat these as public information.
- Use the `server-only` package on modules that must never reach the client.

## Images and Fonts

- `next/image` only. Provide `width` and `height` (or `fill` with a sized parent).
- `next/font` for fonts. No `<link rel="stylesheet">` to Google Fonts.

## Routing

- `next/link` for internal navigation.
- `next/navigation` (`useRouter`, `usePathname`) — never the legacy `next/router`.
- Use Route Groups `(name)` to share layouts without affecting URLs.

## Performance

- Stream slow data with `<Suspense>`.
- Cache aggressively, invalidate explicitly via `revalidateTag`.
- Avoid client-side waterfalls — fetch in the server parent and pass props down.

## Forbidden

- `getServerSideProps`, `getStaticProps`, `getInitialProps` (Pages Router only)
- `next/router` (Pages Router only)
- `<img>`, `<a>` for internal links
- Importing server-only modules into Client Components
- Storing user data in module-level variables (Server Components are not request-scoped instances)
