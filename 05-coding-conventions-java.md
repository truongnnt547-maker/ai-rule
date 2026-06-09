---
paths:
  - "src/main/java/**"
  - "src/test/java/**"
  - "**/*.java"
---

# Coding Conventions — Java / Spring Boot

## Relationship to `00-tool-routing.md`

Apply these Java conventions after the tool-routing gate has selected the right context path.

Do not use coding conventions as a reason to skip CodeGraph or Code Review Graph for non-trivial code understanding, refactoring, review, or impact analysis tasks.

## Target Stack

- Java 21
- Spring Boot 3.x
- Maven

## Java

Prefer:

- Constructor injection
- Record DTOs when appropriate
- Immutable objects
- Clear method names
- Small focused classes

Avoid:

- Field injection
- God classes
- Utility classes with unrelated responsibilities
- Excessive inheritance

## Spring Architecture

```
Controller → Service → Repository
```

Rules:

- No business logic in controllers.
- Services contain business logic.
- Repositories contain persistence logic.

## Transactions

- Define transaction boundaries at the Service layer.
- Avoid transactions in controllers.
- Keep transactional methods focused on one business operation.
- Prefer read-only transactions for query-only service methods when appropriate.

## Naming

| Type         | Convention   | Example              |
|--------------|--------------|----------------------|
| Request DTO  | `*Request`   | `CreateOrderRequest` |
| Response DTO | `*Response`  | `OrderResponse`      |
| Service      | `*Service`   | `OrderService`       |
| Repository   | `*Repository`| `OrderRepository`    |

## Error Handling

Prefer:

- Domain-specific exceptions.
- Consistent error responses.
- Centralized API error mapping with `@ControllerAdvice` where appropriate.

Avoid:

- Generic `RuntimeException` for business errors.
- Swallowed exceptions.
- Returning raw exception messages to API clients.

## Testing

Naming:

- Unit test: `*Test` (e.g., `OrderServiceTest`)
- Integration test: `*IT` (e.g., `OrderServiceIT`)

Prefer:

- Mockito for unit tests
- `@SpringBootTest` for integration tests
- Given / When / Then structure (as comments or via BDD libraries)

Rules:

- Public Service behavior must be covered by unit tests.
- Avoid testing trivial pass-through methods unless they contain business logic.
- Test only behavior, not implementation details.
- Do not mock the class under test.
- Use Spring test mock-bean annotations only in integration or slice tests, not pure unit tests.

Avoid:

- Tests with no assertions.
- Tests that depend on execution order.
- Hardcoded infrastructure URLs in test code.

## Refactoring

When extracting code:

- Preserve behavior.
- Reduce class responsibility.
- Improve cohesion.
- Minimize coupling.
