# TypeScript Strictness

> Refuses `any`, narrows types aggressively, and pushes for discriminated unions over enums and branded types for IDs.

**Pack slug:** `typescript-strictness`
**Version:** 1.0.0
**Author:** ModelBound (Official)

---

## What it does

Configures your AI agent to write TypeScript the way the strictest reviewer on your team would:

- **No `any`.** Ever. Use `unknown` and narrow.
- **No `as` casts** without a comment explaining why the type system can't see what you can.
- **Discriminated unions** over string enums for state machines and variants
- **Branded types** for IDs (`UserId`, `OrderId`) so you can't pass one where the other is expected
- **`readonly` everywhere it makes sense** — props, function params that aren't mutated, return types
- **`satisfies`** instead of type annotations on object literals when you want both inference and constraint
- **`exactOptionalPropertyTypes`** and `noUncheckedIndexedAccess` aware
- **Refuses to widen** — narrows in the function signature, not at the call site

## Who it's for

- **TypeScript teams** with `"strict": true` in tsconfig who want to go further
- **Library authors** publishing types that consumers depend on
- **Tech leads** stamping out `any` proliferation
- **Anyone migrating** from JS or loose TS toward a fully-typed codebase

Not for: throwaway scripts or projects where `strict: false` is intentional.

## What's inside

- `no-any-rules.md` — banned patterns and their replacements
- `discriminated-unions.md` — when to use them instead of enums
- `branded-types.md` — opaque IDs and value objects
- `narrowing-patterns.md` — type guards, assertion functions, `in` operator
- `tsconfig-recommendations.md` — strict-plus settings
- `library-types.md` — guidance for `.d.ts` authors

## Install

### Cursor (`.cursor/rules/`)

```mdc
---
description: Strict TypeScript. No any, narrow aggressively, prefer discriminated unions and branded types.
globs: ["**/*.ts", "**/*.tsx"]
alwaysApply: true
---
```

Set `alwaysApply: true` — strictness rules should apply to every TS file the agent touches.

### Kiro (`.kiro/steering/typescript.md`)

Add as steering.

### Windsurf (`.windsurfrules`)

Add to root rules file.

### Claude Code (`.claude/skills/typescript-strictness/SKILL.md`)

```yaml
---
name: typescript-strictness
description: Write or review TypeScript with maximum type safety. Use whenever generating .ts/.tsx files.
---
```

### GitHub Copilot

Append the contents of `no-any-rules.md` and `discriminated-unions.md` to `.github/copilot-instructions.md`.

## Usage tips

- **Turn on the strict flags first** in your `tsconfig.json` (`strict`, `noUncheckedIndexedAccess`, `exactOptionalPropertyTypes`). The pack assumes you have. Without them the agent's narrowing won't surface real errors.
- **Pair with `perfect-react-refactor`** for full-stack TS work.
- **For library code**, also load `library-types.md` — public API types deserve extra rigor.
- **Don't fight it on `as unknown as Foo`** without a real reason. The pack will push back, and that's the point.
- **Use `satisfies` liberally** — the pack teaches the agent when it's the right tool.

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
