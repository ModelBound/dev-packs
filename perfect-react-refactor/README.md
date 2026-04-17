# Perfect React Refactor

> A senior React engineer in a pack — refactors components for clarity, performance, and accessibility.

**Pack slug:** `perfect-react-refactor`
**Version:** 1.0.0 / 1.1
**Author:** ModelBound (Official)
**Stack:** TypeScript + Tailwind + shadcn/ui

---

## What it does

Turns your AI coding agent into an opinionated React reviewer that:

- Splits oversized components into focused, composable pieces
- Lifts state to the right level (no prop-drilling, no over-contexting)
- Replaces ad-hoc styles with semantic Tailwind tokens and shadcn variants
- Enforces accessibility (semantic HTML, ARIA only when needed, keyboard nav)
- Stabilizes renders (`useMemo`/`useCallback` only where they earn their keep)
- Refuses `any`, narrows props, prefers discriminated unions
- Extracts reusable hooks instead of copy-pasting effect logic

## Who it's for

- **React/Next.js teams** with growing component sprawl
- **Solo founders** who let an agent ship the MVP and now need it cleaned up
- **Design system maintainers** enforcing token discipline across many contributors
- Anyone whose `<Dashboard />` is 800 lines and they know it

## What's inside

- `react-refactor.md` — the core refactoring playbook
- `component-decomposition.md` — when and how to split
- `state-placement.md` — local vs lifted vs context vs server state
- `tailwind-tokens.md` — semantic tokens, no raw colors
- `accessibility-checklist.md` — keyboard, focus, ARIA
- `performance-patterns.md` — memoization rules, list virtualization, Suspense

## Install

### Cursor (`.cursor/rules/`)

```mdc
---
description: Refactor React components for clarity, performance, and a11y.
globs: ["**/*.tsx", "**/*.jsx"]
alwaysApply: false
---
```

Trigger by asking "refactor this component" — Cursor loads the rule based on the glob match.

### Kiro (`.kiro/steering/react-refactor.md`)

Add as steering. Kiro will reference it whenever it touches `.tsx` files.

### Windsurf (`.windsurf/rules/react-refactor.md`)

Drop in as a rule file. Windsurf scopes it automatically by file type if you mention React in the body.

### Claude Code (`.claude/skills/perfect-react-refactor/SKILL.md`)

```yaml
---
name: perfect-react-refactor
description: Refactor a React component for clarity, performance, and accessibility. Use when the user asks to refactor, split, or clean up a .tsx file.
---
```

### GitHub Copilot (`.github/copilot-instructions.md`)

Append the contents of `react-refactor.md` and `tailwind-tokens.md` to your existing instructions file.

## Usage tips

- **Start small**: feed it one component at a time. Big-bang refactors lose context.
- **Pair with a design system pack** (e.g. `tailwind-design-system-enforcer`) if your team uses semantic tokens — the two reinforce each other.
- **Preserve behavior**: ask the agent to write a test (or describe the component's contract) *before* refactoring.
- **Watch for over-memoization**: this pack is restrictive about `useMemo`/`useCallback` — don't fight it without a profile.
- **shadcn-aware**: the pack assumes shadcn primitives. If you're on Mantine/Chakra, swap the relevant section.

## Compatibility

| IDE / Agent | Status |
|---|---|
| Cursor | ✅ Recommended (glob to `.tsx`) |
| Kiro | ✅ Recommended |
| Windsurf | ✅ Supported |
| Claude Code | ✅ Supported |
| Copilot | ✅ Supported |
| Continue.dev | ✅ Supported |

## Versioning

- **1.0.0** — Initial release
- **1.1** — Tighter accessibility section, added Suspense guidance

## License

MIT — © ModelBound. Use freely, attribution appreciated.
