# Next.js App Router Best Practices

> Guides correct use of server vs client components, caching, streaming, and Server Actions in App Router.

**Pack slug:** `nextjs-app-router-best-practices`
**Version:** 1.0.0
**Author:** ModelBound (Official)

---

## What it does

Configures your AI agent to write App Router code the way the Next.js team would:

- **Server components by default**; `"use client"` only when you need state, effects, or browser APIs
- **Data fetching at the leaf**, not lifted to the page — let Suspense parallelize
- **`fetch` with proper caching directives** (`{ cache: 'force-cache' }`, `{ next: { revalidate: 60 } }`, `{ next: { tags: [...] } }`) — never accidentally opt out of caching
- **`revalidateTag` / `revalidatePath`** after mutations, not blanket cache busts
- **Server Actions** for mutations, with `useFormStatus` and `useActionState` for UX
- **Streaming with Suspense** for slow data, `loading.tsx` for routes
- **Route groups** `(group)` for layout sharing without URL impact
- **Parallel and intercepting routes** when they actually solve a problem (not just because they're cool)
- **Metadata API** for SEO, not manual `<head>` tags
- **No `getServerSideProps`-style patterns** — App Router is not Pages Router

## Who it's for

- **Next.js teams** migrating from Pages Router to App Router
- **Full-stack devs** who want their agent to stop putting `"use client"` on everything
- **Performance-focused teams** trying to extract every ms from streaming and caching
- **Anyone** who has been confused by App Router caching defaults at least once (so, everyone)

## What's inside

- `server-vs-client.md` — the decision tree for `"use client"`
- `data-fetching.md` — `fetch`, `cache()`, parallel fetching at leaves
- `caching-model.md` — Next.js 14/15 caching layers explained, with directives
- `server-actions.md` — when to use, how to validate, error handling
- `streaming-suspense.md` — `loading.tsx`, `<Suspense>` boundaries, partial pre-rendering
- `metadata-seo.md` — `generateMetadata`, OG, robots
- `routing-patterns.md` — groups, parallel, intercepting

## Install

### Cursor (`.cursor/rules/`)

```mdc
---
description: Next.js App Router best practices: server-first, correct caching, streaming, Server Actions.
globs: ["app/**/*", "**/app/**/*.tsx", "**/app/**/*.ts"]
alwaysApply: true
---
```

### Kiro (`.kiro/steering/nextjs.md`)

Add as steering.

### Windsurf (`.windsurfrules`)

Add to root.

### Claude Code (`.claude/skills/nextjs-app-router/SKILL.md`)

```yaml
---
name: nextjs-app-router-best-practices
description: Write or review Next.js App Router code. Use whenever editing files under app/.
---
```

## Usage tips

- **State your Next.js version.** Caching defaults changed significantly between 14 and 15 (PPR, default `no-store` for `fetch` in 15). The pack assumes 14+; tell the agent if you're on 15.
- **Pair with `typescript-strictness`** and `tailwind-design-system-enforcer` for full Next.js stack coverage.
- **Don't load this in a Pages Router project.** It will fight you constantly.
- **For RSC + tRPC or RSC + GraphQL** stacks, add a small project-specific addendum — the pack assumes vanilla `fetch` and Server Actions.
- **Test Server Action error paths.** The pack pushes for proper error UX; verify the agent actually wires `useActionState`.

## Compatibility

| IDE / Agent | Status |
|---|---|
| Cursor | ✅ Recommended |
| Kiro | ✅ Recommended |
| Windsurf | ✅ Supported |
| Claude Code | ✅ Supported |
| Copilot | ✅ Supported |
| Continue.dev | ✅ Supported |

## License

MIT — © ModelBound.
