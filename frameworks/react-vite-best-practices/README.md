# React + Vite Best Practices

> Writes production-grade React 19 + Vite 6 code with TypeScript, TanStack Query, proper state management, accessibility, and performance optimization baked in.

**Pack slug:** `react-vite-best-practices`  
**Version:** 1.0.0  
**Author:** ModelBound (Official)

---

## What it does

Configures your AI agent to write React + Vite code that would pass a senior engineer's code review:

- **React 19 patterns** — `useActionState`, `useOptimistic`, `use()` for async resources, and React Compiler-aware code that avoids unnecessary manual memoization  
- **Vite 6 configuration** — typed env vars, path aliases, manual chunk splitting, route-level code splitting, and bundle analysis  
- **TypeScript strict mode** — discriminated unions, generic components, correct event types, no `any`, exhaustive switches  
- **TanStack Query v5** — query key factories, optimistic mutations, Suspense boundaries, proper cache invalidation  
- **State management decisions** — the right tool for each layer: `useState` for local, Context for feature-level, Zustand for global UI, TanStack Query for server data, URL for shareable state  
- **Testing with Vitest + MSW** — component integration tests, hook tests, API mocking, accessibility audits with axe  
- **WCAG 2.1 AA accessibility** — semantic HTML, ARIA patterns, focus management, skip links, color contrast  
- **Performance** — `useTransition`, `useDeferredValue`, virtualization for large lists, image optimization, Core Web Vitals targets  

---

## Who it's for

- **React developers** who want their code to reflect current best practices, not 2020 patterns  
- **Teams adopting React 19** and the React Compiler who need guidance on what to stop doing manually  
- **Developers migrating to Vite** from Create React App or Webpack who want an optimized config from day one  
- **Anyone building production SPAs** who cares about accessibility, performance, and maintainability  
- **Code reviewers** who want consistent standards enforced automatically  

---

## What's inside

| File | Type | Covers |
|------|------|--------|
| `React + Vite Expert` | System Prompt | Agent identity, non-negotiables, core principles |
| `React + Vite Project Rules` | Cursor Rules | Tech stack, feature-based folder structure, code style, boundaries |
| `react-component-patterns` | Skill | Component anatomy, compound components, React 19 APIs (`useActionState`, `useOptimistic`, `use()`) |
| `vite-configuration` | Skill | `vite.config.ts`, env config module, code splitting, bundle analysis, tsconfig aliases |
| `data-fetching-patterns` | Skill | TanStack Query v5 setup, query key factory, mutation hooks, optimistic updates, Suspense |
| `typescript-react-patterns` | Skill | Generic components, discriminated unions, event types, utility types, strict tsconfig |
| `testing-react-vite` | Skill | Vitest + Testing Library, MSW for API mocking, hook tests, accessibility testing |
| `performance-optimization` | Skill | React Compiler guidance, `useTransition`, `useDeferredValue`, virtualization, Core Web Vitals |
| `accessibility-react` | Skill | WCAG 2.1 AA, ARIA patterns, focus trapping, semantic HTML, skip links |
| `state-management` | Skill | Decision framework, Context, Zustand, URL state, React Hook Form + Zod |

---

## Install

### Cursor (`.cursor/rules/`)

```yaml
---
description: Write production-grade React 19 + Vite 6 code with TypeScript, TanStack Query, accessibility, and performance best practices.
globs: ["src/**/*.tsx", "src/**/*.ts", "vite.config.ts", "*.config.ts"]
alwaysApply: false
---
