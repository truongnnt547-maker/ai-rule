# Coding Conventions — Java / Spring Boot (Architecture Reference)

> This file has no `paths` frontmatter — it is NOT auto-loaded.
> Read it when: starting a new project, planning architecture,
> defining transaction boundaries, or doing large-scale refactoring.

## Target Stack

- Java 21
- Spring Boot 3.x
- Maven

## Spring Architecture

```
Controller → Service → Repository
```

- No business logic in controllers.
- Services contain business logic.
- Repositories contain persistence logic.

## Transactions

- Define transaction boundaries at the Service layer.
- Avoid transactions in controllers.
- Keep transactional methods focused on one business operation.
- Prefer read-only transactions for query-only service methods when appropriate.

## Refactoring

When extracting code:

- Preserve behavior.
- Reduce class responsibility.
- Improve cohesion.
- Minimize coupling.
