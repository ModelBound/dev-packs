# Tailwind Design System Enforcer

> Refuses raw colors and arbitrary values. Pushes everything through semantic tokens and shadcn/CVA variants.

**Pack slug:** `tailwind-design-system-enforcer`
**Version:** 1.0.0
**Author:** ModelBound (Official)

---

## What it does

Configures your AI agent to treat your Tailwind config as the law:

- **No raw colors** — no `text-white`, `bg-black`, `text-red-500`. Always semantic tokens (`text-foreground`, `bg-background`, `text-destructive`).
- **No arbitrary values** — no `w-[437px]`, no `text-[#ff0000]`. If you need it, add it to `tailwind.config.ts`.
- **shadcn/CVA variants** for components — define variants in `cva()`, don't conditionally string-concat classes
- **HSL-based tokens** in `index.css` so themes work
- **Spacing through scale** — no `mt-[13px]` when `mt-3` exists
- **Typography from the scale** — no `text-[15px]` font-size hacks
- **Dark mode via tokens**, never hardcoded `dark:bg-zinc-900`
- **`cn()` helper** for class merging, never raw string concatenation

The agent will refuse to write color literals and instead suggest the right token — or ask you to add one.

## Who it's for

- **Design system teams** rolling out semantic tokens across many product surfaces
- **shadcn/ui users** who want their AI agent to use the system, not bypass it
- **Frontend leads** tired of dark-mode bugs caused by hardcoded colors
- **Solo developers** building on top of shadcn who want consistency from day one

## What's inside

- `semantic-tokens.md` — the token vocabulary (`background`, `foreground`, `primary`, `muted`, `accent`, `destructive`, etc.)
- `cva-patterns.md` — how to define and use variants with `class-variance-authority`
- `forbidden-patterns.md` — every anti-pattern this pack catches
- `adding-new-tokens.md` — when and how to extend the system
- `dark-mode.md` — token-driven theming

## Install

### Cursor (`.cursor/rules/`)

```mdc
---
description: Enforce semantic Tailwind tokens and shadcn/CVA. No raw colors, no arbitrary values.
globs: ["**/*.tsx", "**/*.jsx", "**/*.css"]
alwaysApply: true
---
```

`alwaysApply: true` is critical — design-system rules need to apply on every component the agent touches.

### Kiro (`.kiro/steering/design-system.md`)

Add as steering. Pair with a `design-tokens.md` spec file describing your specific tokens.

### Windsurf (`.windsurfrules`)

Add to root.

### Claude Code (`.claude/skills/tailwind-design-system/SKILL.md`)

```yaml
---
name: tailwind-design-system-enforcer
description: Enforce semantic Tailwind tokens and shadcn variants. Use whenever writing or editing component styles.
---
```

### GitHub Copilot

Append `semantic-tokens.md` and `forbidden-patterns.md` to `.github/copilot-instructions.md`.

## Usage tips

- **Document your tokens.** Add a section to your project `instructions.md` listing your specific semantic tokens. The pack provides the *rules*; you provide the *vocabulary*.
- **Pair with `perfect-react-refactor`** — the two reinforce each other and clean up legacy components fast.
- **Add an ESLint rule** ([`eslint-plugin-tailwindcss`](https://github.com/francoismassart/eslint-plugin-tailwindcss) with `no-arbitrary-value`) as a backstop.
- **When the agent asks** "should I add a new token?", say yes and add it. Don't let it inline.
- **For non-shadcn projects** (Mantine, Chakra), strip the CVA section and replace with your library's primitives.

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
