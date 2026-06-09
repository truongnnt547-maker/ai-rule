# Cline Rules

A collection of reusable `.clinerules` guidance for AI-assisted software engineering. These rules help AI coding agents (such as Cline) produce cleaner, safer, and more maintainable code by defining conventions, workflows, and constraints.

## Prerequisites

**MCP Server Requirement:** Rules 02 (CodeGraph) and 06 (Code Review Graph) require MCP servers to be installed, configured, and running. See [`00-mcp-configuration.md`](00-mcp-configuration.md) for installation and verification instructions.

## File Index

| File | Purpose |
|---|---|
| [`00-tool-routing.md`](00-tool-routing.md) | Always-active routing gate for graph tools before manual source reads |
| [`00-mcp-configuration.md`](00-mcp-configuration.md) | MCP server configuration, verification, and installation |
| [`01-context-management.md`](01-context-management.md) | Minimize token usage; avoid loading unnecessary files or sections |
| [`02-codegraph-first.md`](02-codegraph-first.md) | Use CodeGraph MCP as primary tool for symbol lookup, references, and dependency analysis |
| [`03-large-file-handling.md`](03-large-file-handling.md) | Strategies for reading and refactoring files over 300 lines |
| [`04-refactor-strategy.md`](04-refactor-strategy.md) | Incremental, safe refactoring — one responsibility at a time |
| [`05-coding-conventions-java.md`](05-coding-conventions-java.md) | Java 21 / Spring Boot 3.x conventions: architecture, naming, testing |
| [`05-coding-conventions-react.md`](05-coding-conventions-react.md) | ReactJS / npm conventions: components, hooks, state, testing |
| [`06-code-review-graph.md`](06-code-review-graph.md) | Use Code Review Graph MCP for impact analysis, blast radius, and architecture |
| [`07-git-discipline.md`](07-git-discipline.md) | Clean commit history: format, separation, and checkpoint rules |
| [`08-ignore-files.md`](08-ignore-files.md) | Files and directories the agent should never read, scan, or modify |
| [`09-general-behavior.md`](09-general-behavior.md) | Overarching principles: simplicity, surgical changes, goal-driven execution |
| [`10-security-and-privacy.md`](10-security-and-privacy.md) | Security checks: secrets handling, authn/z, validation, common web vulns, crypto, logging |
| [`11-dependency-and-build-discipline.md`](11-dependency-and-build-discipline.md) | Safe dependency/build changes: approval, lockfiles, validation, CI/config scope |

## Usage

Place these files inside a `.clinerules/` directory at the root of your repository or at the global Cline rules path (`Cline/Rules`). Cline automatically reads them and applies the guidance during coding sessions.

### Rule Application Order

1. **00-tool-routing.md** — Always-active gate: choose Code Review Graph / CodeGraph before manual source reads for non-trivial tasks
2. **00-mcp-configuration.md** — Prerequisite: documents MCP server availability and verification
3. **01-context-management.md** — Keep context small after routing has selected the right tool path
4. **02-codegraph-first.md**, **06-code-review-graph.md** — Detailed graph workflows used when the routing gate applies
5. **03-large-file-handling.md**, **04-refactor-strategy.md** — Apply during refactoring tasks
6. **05-coding-conventions-java.md**, **05-coding-conventions-react.md** — Automatically scoped to Java/React source files; apply after graph routing
7. **07-git-discipline.md**, **08-ignore-files.md**, **09-general-behavior.md** — Always active
8. **10-security-and-privacy.md**, **11-dependency-and-build-discipline.md** — Scoped checks (security/build); apply after graph routing

## Key Principles

### Context Efficiency
- Never load more files than necessary.
- Read only the relevant sections of large files.
- Summarize findings before moving on.

### MCP-Powered Graph-First Repository Understanding

**Requires MCP servers** (see `00-mcp-configuration.md`):
- **CodeGraph** (`@colbymchenry/codegraph`): symbol lookup, references, dependency graph, impact analysis at symbol level
- **Code Review Graph** (`code-review-graph`): change impact, blast radius, architecture overview, developer onboarding

Use `00-tool-routing.md` as the compact always-active gate. It decides whether Code Review Graph or CodeGraph should run before manual source reads, while `02` and `06` provide the detailed workflows only when needed.

### Incremental Refactoring
- Modify at most 3 files per iteration.
- Extract one responsibility at a time.
- Never perform large refactors in a single step.

### Surgical Changes
- Touch only what you must.
- No speculative abstractions.
- Remove imports that your changes made unused; leave pre-existing dead code alone.

### Testing & Validation
- **Java**: each public Service method needs a unit test; name tests `*Test` (unit) or `*IT` (integration).
- **React**: each custom hook needs a test; prefer React Testing Library.
- After each extraction: verify dependencies, imports, constructor wiring, and compile.

### Git Discipline
- Follow `type(scope): short description` format.
- Never mix refactoring with feature commits.
- Suggest checkpoints after each phase.

### Stack-Specific Conventions

| Stack | Key Rules |
|---|---|
| **Java / Spring Boot** | Constructor injection, no field injection, layered architecture (Controller → Service → Repository), record DTOs |
| **React / npm** | Functional components only, co-locate files by feature, custom hooks for reusable logic, CSS modules |

### Stack-Specific Scoping

Java and React conventions (`05-coding-conventions-*.md`) use frontmatter `paths:` to automatically apply only to relevant files. They should not replace CodeGraph / Code Review Graph for non-trivial code understanding, refactoring, review, or impact analysis tasks:
- Java rules apply to `src/main/java/**`, `src/test/java/**`, `**/*.java`
- React rules apply to `src/components/**`, `src/pages/**`, `**/*.tsx`, `**/*.jsx`

### Ignored Artifacts

Target directories, build output, lockfiles, IDE config, `.env.local` files — the agent must never read, scan, or modify these. See `08-ignore-files.md` for the complete list.

**Security:** `.env.local` and similar files may contain secrets and must never be read or included in output.

## License

MIT