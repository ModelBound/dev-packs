# React Refactor Rules

These rules apply to every React/TypeScript file in this project.

## TypeScript

- No `any`. Use `unknown` and narrow with type guards.
- No `React.FC`. Type props with an explicit interface or type alias.
- Public function signatures must be explicit (no inference for exports).
- Prefer discriminated unions over enums for state machines.
- Use `as const` for literal sets, branded types for IDs.

## Component Structure

- One default export per file: the component.
- Co-locate types in the same file unless shared by 3+ files.
- File length soft cap: 200 lines. Hard cap: 300.
- Extract a sub-component when a JSX block is ≥ 30 lines or has ≥ 3 conditionals.

## Hooks

- All hooks at the top of the component, before any `return`.
- No conditional hook calls. Ever.
- Custom hooks named `use*` and live in `src/hooks/`.
- Side effects only in `useEffect` or event handlers.

## Performance

- No `useMemo` / `useCallback` / `React.memo` without a profiled reason.
- Stable keys for lists: use IDs, never array index for dynamic lists.
- Lazy-load routes with `React.lazy` + `Suspense`.

## Styling

- Tailwind only. No inline `style={{ ... }}` unless dynamic numeric values.
- Semantic color tokens only: `text-foreground`, `bg-card`, `border-border`.
- Never `text-white`, `bg-black`, `text-[#xxxxxx]`.
- Use shadcn primitives. Do not roll your own button/input/dialog.

## Accessibility

- Every interactive element keyboard-reachable.
- Labels for all form inputs (`<Label htmlFor=...>` or `aria-label`).
- Focus visible on all focusable elements.
- Modals trap focus and restore it on close.

## Testing

- Tests live next to the component: `Component.test.tsx`.
- Use React Testing Library, not Enzyme.
- Test behavior, not implementation. No `wrapper.state()`.
