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
  - "package.json"
---

# Coding Conventions — ReactJS / npm

## Target Stack

- ReactJS (functional components)
- npm

## Component Rules

Prefer:

- Functional components with hooks
- Single responsibility per component
- Custom hooks for reusable logic extraction
- Named exports for components (default export only for pages)

Avoid:

- Class components
- Prop drilling beyond 2 levels (use context or state manager)
- Mixing UI logic with business logic in one component
- Inline styles (prefer CSS modules or utility classes)

## Naming

| Type          | Convention      | Example              |
|---------------|-----------------|----------------------|
| Component     | `PascalCase`    | `OrderCard`          |
| Hook          | `use*`          | `useOrderList`       |
| Util function | `camelCase`     | `formatCurrency`     |
| Type / Interface | `PascalCase` | `OrderItem`          |
| CSS module    | `*.module.css`  | `OrderCard.module.css` |

## File Structure

Co-locate files by feature, not by type:

```
src/features/order/
├── OrderCard.tsx
├── OrderCard.module.css
├── useOrderList.ts
├── orderService.ts
└── types.ts
```

## State Management

- Local UI state: `useState`
- Shared/server state: context, Zustand, or React Query
- Do not put server data in global client state unless necessary

## Error Handling

- Use error boundaries for component-level errors.
- Handle async errors explicitly — avoid silent failures.
- Show user-friendly error messages, not raw error objects.

## Testing

Naming:

- Unit/component test: `*.test.tsx` or `*.spec.tsx`
- Hook test: `*.test.ts`

Prefer:

- React Testing Library over Enzyme
- Test user behavior, not implementation details
- Mock only external dependencies (API calls, modules), not internal components

Rules:

- Each custom hook must have at least one test.
- Do not test internal state directly — test rendered output or side effects.

Avoid:

- Snapshot tests as the primary assertion strategy.
- Tests that break when component internals change without behavioral impact.

## Performance

- Memoize expensive computations with `useMemo`.
- Memoize stable callbacks passed as props with `useCallback`.
- Avoid premature memoization — profile first.
