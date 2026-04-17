# TypeScript Rules

## Compiler

- `strict: true` is non-negotiable.
- `noUncheckedIndexedAccess: true` so `arr[0]` is `T | undefined`.
- `exactOptionalPropertyTypes: true` distinguishes `{ x?: number }` from `{ x?: number | undefined }`.

## Bans

- No `any`. Replace with `unknown` + narrowing, or generics.
- No `as unknown as T`. If unavoidable, comment with the invariant.
- No `@ts-ignore`. Use `@ts-expect-error` and link a ticket.
- No `!` non-null assertions outside the line that proves non-null.

## Type Definitions

- `interface` for object shapes that may be extended or augmented.
- `type` for unions, intersections, mapped/conditional types.
- Public function signatures must be explicit:
  ```ts
  // ✅
  export function format(date: Date): string { ... }

  // ❌ inferred return leaks across module boundary
  export function format(date: Date) { ... }
  ```
- Co-locate types with the code that uses them. Promote to `types.ts` only when 3+ files share.

## Domain Modeling

- Use discriminated unions for state with multiple shapes.
- Use branded/opaque types for primitives that should not be confused (`UserId`, `Email`, `Currency`).
- Use `Readonly<T>` and `ReadonlyArray<T>` for data that should not mutate.
- Use `as const` and derive types from values (`(typeof X)[number]`) over hand-written enums.

## Generics

- Constrain generics: `<T extends { id: string }>`, not bare `<T>`.
- One letter is fine for trivial generics, name them otherwise: `<TUser>`, `<TKey extends string>`.
- No more than 3 type parameters per function. If you need more, the function does too much.

## Errors

- Throw `Error` subclasses with stable `name`, not strings.
- Functions that can fail predictably return `Result<T, E>` or a discriminated union, not throw.
