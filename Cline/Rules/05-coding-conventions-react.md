---
paths:
  - "src/components/**"
  - "src/pages/**"
  - "src/hooks/**"
  - "src/features/**"
  - "src/layouts/**"
  - "src/utils/**"
  - "src/services/**"
  - "src/store/**"
  - "src/types/**"
  - "**/*.tsx"
  - "**/*.jsx"
---

# Coding Conventions — ReactJS (Daily Rules)

> For file structure, state management, and performance guidelines,
> see `05-coding-conventions-react-architecture.md`.

## Naming

| Type             | Convention      | Example                |
|------------------|-----------------|------------------------|
| Component        | `PascalCase`    | `OrderCard`            |
| Hook             | `use*`          | `useOrderList`         |
| Util function    | `camelCase`     | `formatCurrency`       |
| Type / Interface | `PascalCase`    | `OrderItem`            |
| CSS module       | `*.module.css`  | `OrderCard.module.css` |

## Component Rules

Prefer: functional components, single responsibility, custom hooks for reusable logic,
named exports (except where framework requires default exports).

Avoid: class components, excessive prop drilling, mixing UI and business logic, inline styles.

## TypeScript

Prefer: explicit prop types, narrow types, separate API DTO types from UI view models when shapes diverge.

Avoid: `any` without documented reason, duplicating type definitions across features.

## Error Handling

- Use error boundaries for component-level errors.
- Handle async errors explicitly — no silent failures.
- Show user-friendly messages, not raw error objects.

## Testing

- Component test: `*.test.tsx` / `*.spec.tsx` — use React Testing Library.
- Hook test: `*.test.ts`.
- Test user behavior, not implementation details.
- Each custom hook must have at least one test.
- No snapshot tests as primary assertion strategy.
