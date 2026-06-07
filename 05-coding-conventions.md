# Coding Conventions

## Java

Target:

- Java 21
- Spring Boot 3.x

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

## Spring

Architecture:

Controller
→ Service
→ Repository

Rules:

- No business logic in controllers.
- Services contain business logic.
- Repositories contain persistence logic.

## Naming

Request DTO:

*Request

Response DTO:

*Response

Service:

*Service

Repository:

*Repository

## Error Handling

Prefer:

- Domain-specific exceptions
- Consistent error responses

Avoid:

- Generic RuntimeException
- Swallowed exceptions

## Testing

Naming:

- Unit test: `*Test` (e.g., `OrderServiceTest`)
- Integration test: `*IT` (e.g., `OrderServiceIT`)

Prefer:

- Mockito for unit tests
- `@SpringBootTest` for integration tests
- Given / When / Then structure (as comments or via BDD libraries)

Rules:

- Each public Service method must have at least one unit test.
- Test only behavior, not implementation details.
- Do not mock the class under test.
- Use `@MockBean` only in integration tests, not unit tests.

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
