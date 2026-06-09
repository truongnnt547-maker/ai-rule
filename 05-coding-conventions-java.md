---
paths:
  - "src/main/java/**"
  - "src/test/java/**"
  - "**/*.java"
---

# Coding Conventions — Java / Spring Boot (Daily Rules)

> For architecture, layering, transactions, and refactoring guidelines,
> see `05-coding-conventions-java-architecture.md`.

## Naming

| Type         | Convention    | Example              |
|--------------|---------------|----------------------|
| Request DTO  | `*Request`    | `CreateOrderRequest` |
| Response DTO | `*Response`   | `OrderResponse`      |
| Service      | `*Service`    | `OrderService`       |
| Repository   | `*Repository` | `OrderRepository`    |

## Code Style

Prefer: constructor injection, Record DTOs, immutable objects, small focused classes.

Avoid: field injection, god classes, unrelated utility methods, excessive inheritance.

## Error Handling

Prefer: domain-specific exceptions, `@ControllerAdvice` for centralized API error mapping.

Avoid: generic `RuntimeException` for business errors, swallowed exceptions, raw exception messages to API clients.

## Testing

- Unit test: `*Test` — use Mockito, Given/When/Then structure.
- Integration test: `*IT` — use `@SpringBootTest`.
- Cover public Service behavior; skip trivial pass-through methods.
- Do not mock the class under test.
- Use `@MockBean` only in integration or slice tests.
- No tests without assertions. No execution-order dependencies.
