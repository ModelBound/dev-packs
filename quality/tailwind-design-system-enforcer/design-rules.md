# Design System Rules

## Colors

- All colors via semantic tokens defined in `index.css`.
- Tokens stored as raw HSL values: `--primary: 222 47% 11%;`, used as `hsl(var(--primary))`.
- Component classes reference token names, not raw values: `bg-primary`, `text-primary-foreground`.
- Banned: `text-white`, `text-black`, `bg-white`, `bg-black`, any `text-{color}-{shade}` from default Tailwind palette in component files.

## Spacing

- Use the Tailwind scale: `p-1` through `p-12`, then `p-16`, `p-20`, `p-24`.
- Arbitrary values (`p-[17px]`) only when matching a fixed external requirement (e.g. embedding into a third-party iframe).
- Vertical rhythm via `space-y-{2,4,6,8}`, not ad-hoc margins.

## Typography

- Use `text-{xs,sm,base,lg,xl,2xl,...}`. No arbitrary font sizes.
- Headings via the `Heading` component (or shadcn equivalent), not raw `<h1>` with classes.
- Body text inherits color and size; do not restate `text-foreground text-base` everywhere.

## Components

- Use shadcn primitives: `Button`, `Input`, `Dialog`, `Sheet`, `Card`.
- Extend variants through CVA. Do not pass `className` to override the design.
- New primitives go through design review before merge.

## Shadows and Radii

- Shadows on the `shadow-{sm,md,lg,xl}` scale only.
- Radii on the `rounded-{sm,md,lg,xl,2xl,full}` scale only.
- No `shadow-[0_2px_4px_rgba(0,0,0,0.1)]` or `rounded-[7px]`.

## Dark Mode

- All tokens defined for both `:root` and `.dark` in `index.css`.
- No `dark:bg-...` overrides in components — the token already adapts.
- Test every new component in both themes before merging.

## Accessibility

- Interactive elements have `focus-visible:ring-2 focus-visible:ring-ring`.
- Color is not the only signal. Pair color with icon or text.
- Contrast ratio ≥ 4.5:1 for body text, ≥ 3:1 for large text and UI.
