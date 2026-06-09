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

# Coding Conventions — ReactJS / npm

## Relationship to `00-tool-routing.md`

Apply these React conventions after the tool-routing gate has selected the right context path.

Do not use coding conventions as a reason to skip CodeGraph or Code Review Graph for non-trivial code understanding, refactoring, review, or impact analysis tasks.

## Target Stack

- ReactJS (functional components)
- npm

## Component Rules

Prefer:

- Functional components with hooks
- Single responsibility per component
- Custom hooks for reusable logic extraction
- Named exports for components, except where framework conventions require default exports

Avoid:

- Class components
- Excessive prop drilling when it harms readability; prefer composition, context, or a state manager when it simplifies data flow
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

- Local UI state: `useState`.
- Shared client state: context or Zustand when local state is insufficient.
- Server/cache state: React Query or equivalent data-fetching cache.
- Do not put server data in global client state unless necessary.

## TypeScript

Prefer:

- Explicit prop types for components.
- Narrow types over broad types.
- Separate API DTO types from UI view models when their shapes diverge.

Avoid:

- `any` unless there is a documented reason.
- Duplicating type definitions across features.

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
