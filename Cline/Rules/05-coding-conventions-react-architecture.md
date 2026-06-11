# Coding Conventions — ReactJS (Architecture Reference)

> This file has no `paths` frontmatter — it is NOT auto-loaded.
> Read it when: starting a new project, planning feature structure,
> choosing state management strategy, or doing large-scale refactoring.

## Target Stack

- ReactJS (functional components)
- npm

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

## Performance

- Memoize expensive computations with `useMemo`.
- Memoize stable callbacks passed as props with `useCallback`.
- Avoid premature memoization — profile first.
