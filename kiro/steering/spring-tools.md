---
inclusion: fileMatch
fileMatchPattern: "**/*.{java,kt,gradle,gradle.kts,xml,properties,yml,yaml}"
description: Spring Tools MCP usage guide — bean lookup, DI wiring, endpoint mapping, diagnostics, and version management
---

## Spring Tools MCP

This project has a Spring Tools MCP server configured. Spring Tools understands the Spring application context — it knows bean wiring, stereotype roles, injection points, and endpoint mappings in a way that pure AST parsing cannot.

### Tool reference

#### Project & environment
| Question | Tool |
|---|---|
| "List all Java projects in workspace" | `getProjectList` |
| "What Spring Boot version is this project on?" | `getSpringBootVersion` |
| "What Java version is being used?" | `getJavaVersion` |
| "What JAR dependencies are on the classpath?" | `getResolvedProjectClasspath` |
| "Are there any Spring config errors or warnings?" | `getProjectDiagnostics` |

#### Beans & dependency injection
| Question | Tool |
|---|---|
| "List all beans / what beans are registered?" | `getBeanDetails` |
| "Where is bean X defined and where is it injected?" | `getBeanUsageInfo` |
| "Find all beans that implement interface / extend type Y" | `findBeansByType` |
| "What are all the stereotypes in this project?" | `getStereotypesList` |
| "Find all @Service / @Repository / @Controller components" | `findComponentsByStereotype` |
| "List every component and its stereotype" | `getListOfComponentsAndTheirStereotypes` |

#### REST endpoints
| Question | Tool |
|---|---|
| "List all REST endpoints in the project" | `getRequestMappings` |
| "Find all GET / POST / PUT / DELETE endpoints" | `findRequestMappingsByMethod` |

#### Spring ecosystem versions
| Question | Tool |
|---|---|
| "What is the latest stable Spring Boot version?" | `getLatestBootVersionsFromMavenRepo` |
| "What is the latest GA release and support timeline?" | `getLatestReleaseInformation` |
| "List all releases for a Spring project" | `getReleases` |
| "When does OSS/commercial support end for version X?" | `getGenerations` |
| "What Spring releases are coming up?" | `getUpcomingReleases` |

---

### Rules of thumb

- **Spring context questions → Spring Tools first.** For anything involving `@Component`, `@Service`, `@Repository`, `@Bean`, `@Autowired`, `@Controller`, or DI wiring, use Spring Tools before touching codegraph or grep.
- **Spring Tools gives you the name, codegraph gives you the body.** Typical two-step: `getBeanDetails` / `findComponentsByStereotype` to identify the exact class → `codegraph_node` or `codegraph_explore` to read its implementation. Do NOT grep source files to find beans.
- **Don't grep for `@RequestMapping` / `@GetMapping` etc.** Use `getRequestMappings` or `findRequestMappingsByMethod` — one call returns all resolved URLs with handler methods.
- **Don't use `codegraph_search` to find components by role** (e.g., "find all repositories"). Use `findComponentsByStereotype` — it is stereotype-aware and faster.
- **Check diagnostics before investigating config issues.** Run `getProjectDiagnostics` first — Spring Tools flags misconfigured beans, missing dependencies, and wiring errors directly without needing to trace source.
- **Version queries → Spring Tools, not web search.** Use `getLatestBootVersionsFromMavenRepo` / `getLatestReleaseInformation` instead of searching the web for Spring release info.

---

### Integration with the triage-to-fix pipeline

When following the observability triage pipeline and a stack trace mentions a Spring-managed class:

1. **Step 1 (Isolate):** `vietcap` to get the stack trace as usual.
2. **Step 1.5 (Spring context):** Before jumping to `codegraph_search`, run `getBeanUsageInfo` on the faulting class — confirms injection points and wiring. Also run `getProjectDiagnostics` to catch any pre-existing config errors that may be the root cause.
3. **Step 2 (Locate):** Then use `codegraph_node` / `codegraph_search` / `codegraph_callers` / `codegraph_callees` for logic-level analysis.
4. **Steps 3–4:** Proceed with `code-review-graph` and fix as normal.

---

### Strict boundary with codegraph

| Task | Correct tool |
|---|---|
| Bean registration, wiring, injection points | Spring Tools |
| Component role / stereotype lookup | Spring Tools |
| REST endpoint → handler mapping | Spring Tools |
| Spring config diagnostics (errors/warnings) | Spring Tools |
| Spring Boot / ecosystem version info | Spring Tools |
| Internal method logic, call hierarchy | `codegraph_*` |
| Cross-module structural impact | `codegraph_impact` |
| Git/PR change risk | `code-review-graph` |

**Never substitute codegraph structural search for Spring context queries** — codegraph sees annotations as raw syntax and cannot resolve bean wiring, stereotype roles, or injection point relationships.
