---
paths:
  - "**/*"
---

# Security and Privacy

## Goal

Prevent security regressions and sensitive-data leaks. Apply these checks before implementing changes.

## Secrets and Sensitive Data

- Never read, log, or echo secrets. Treat credentials, tokens, keys, and PII as sensitive.
- Do not commit secrets; prefer environment configuration and secret managers.
- Mask secrets in logs and error messages; do not log raw tokens or passwords.
- Keep secrets out of screenshots, comments, and test fixtures.

## Authentication & Authorization

- Enforce authn/authz on all privileged operations. Reject unauthenticated/unauthorized requests early.
- Prefer server-side checks; never rely solely on client-side enforcement.
- Avoid role/permission bypass by verifying ownership and scope (e.g., multi-tenant boundaries).

## Input Validation & Output Encoding

- Validate and normalize all untrusted input (HTTP, headers, query, body, path, files, cookies).
- Apply output encoding for the target sink (HTML, JS, JSON, SQL parameters, shell, path) to prevent injection.
- Prefer allowlists and strict types over free-form strings when feasible.

## Common Web Vulnerabilities

- SQL/NoSQL: use parameterized queries/ORM; never string-concatenate queries.
- XSS: avoid `dangerouslySetInnerHTML` / raw HTML. If unavoidable, sanitize and encode.
- SSRF: do not fetch arbitrary user-provided URLs; if required, restrict schemes/hosts/ports and enforce DNS pinning where possible.
- Path traversal: normalize and restrict filesystem paths to allowed roots; disallow `..` traversal.
- Deserialization: avoid deserializing untrusted data; prefer vetted formats (JSON) and strict schemas.

## Cryptography and Passwords

- Never invent cryptography. Use battle-tested libs and defaults.
- Hash passwords with modern KDFs (bcrypt/scrypt/Argon2); never store plaintext or reversible forms.
- Do not roll your own token formats; use established standards (e.g., JWT with appropriate validation) when necessary and validate exp/iss/aud.

## Logging and Monitoring

- Log high-level events (auth decisions, permission denials) without leaking secrets or PII.
- Avoid verbose stack traces to end users; return safe error messages.

## Dependencies and Supply Chain

- Prefer maintained libraries. Avoid unvetted or obsolete packages for security-sensitive code.
- When adding a dependency for security functionality, justify why the dependency is necessary.

## Verification

- After changes touching auth, input handling, data access, or crypto, re-check for the above risks.
- Prefer targeted tests (unit/integration) that cover authz decisions, validation, and error handling.