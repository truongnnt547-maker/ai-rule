---
inclusion: fileMatch
fileMatchPattern: "**/*.{java,kt,gradle,gradle.kts,xml,properties,yml,yaml}"
description: Spring Tools MCP usage guide — bean lookup, DI wiring, endpoint mapping, diagnostics, and version management
---

## Spring Tools MCP

## Availability

Apply this steering only when Spring Tools MCP tools are available in the current session. If unavailable, use CodeGraph or targeted file/search workflow carefully.

Spring Tools understands the Spring application context: bean wiring, stereotype roles, injection points, endpoint mappings, diagnostics, and version metadata.

### Tool Reference

#### Project & Environment

| Question | Tool |
|---|---|
| "List all Java projects in workspace" | `getProjectList` |
| "What Spring Boot version is this project on?" | `getSpringBootVersion` |
| "What Java version is being used?" | `getJavaVersion` |
| "What JAR dependencies are on the classpath?" | `getResolvedProjectClasspath` |
| "Are there any Spring config errors or warnings?" | `getProjectDiagnostics` |

#### Beans & Dependency Injection

| Question | Tool |
|---|---|
| "List all beans / what beans are registered?" | `getBeanDetails` |
| "Where is bean X defined and where is it injected?" | `getBeanUsageInfo` |
| "Find all beans that implement interface / extend type Y" | `findBeansByType` |
| "What are all the stereotypes in this project?" | `getStereotypesList` |
| "Find all @Service / @Repository / @Controller components" | `findComponentsByStereotype` |
| "List every component and its stereotype" | `getListOfComponentsAndTheirStereotypes` |

#### REST Endpoints

| Question | Tool |
|---|---|
| "List all REST endpoints in the project" | `getRequestMappings` |
| "Find all GET / POST / PUT / DELETE endpoints" | `findRequestMappingsByMethod` |

#### Spring Ecosystem Versions

| Question | Tool |
|---|---|
| "What is the latest stable Spring Boot version?" | `getLatestBootVersionsFromMavenRepo` |
| "What is the latest GA release and support timeline?" | `getLatestReleaseInformation` |
| "List all releases for a Spring project" | `getReleases` |
| "When does OSS/commercial support end for version X?" | `getGenerations` |
| "What Spring releases are coming up?" | `getUpcomingReleases` |

### Rules Of Thumb

- Spring context questions should use Spring Tools first when available. This includes `@Component`, `@Service`, `@Repository`, `@Bean`, `@Autowired`, `@Controller`, DI wiring, conditional beans, profiles, and application configuration.
- Spring Tools gives the bean/handler name; CodeGraph gives the implementation body. Typical flow: identify exact class with Spring Tools, then use `codegraph_node` or `codegraph_explore` for method logic.
- Prefer `getRequestMappings` or `findRequestMappingsByMethod` over text search for resolved REST endpoint mappings.
- Prefer stereotype/component tools over raw symbol search when looking for components by Spring role.
- Run `getProjectDiagnostics` first for Spring configuration, profile, bean wiring, or dependency issues.
- Version queries should use Spring Tools instead of web search when available.

### Integration With The Triage-To-Fix Pipeline

When following `05-log-query-observability.md` and a stack trace mentions a Spring-managed class:

1. Use `log-query` to get the original relevant stack trace section.
2. Extract the faulting class name from the original stack trace.
3. Run `getBeanUsageInfo` on the class when available to confirm injection points and wiring.
4. Run `getProjectDiagnostics` when configuration or DI may be involved.
5. Use CodeGraph for logic-level analysis.
6. Proceed with code-review-graph risk review when a fix is planned and the tool is available.

### Boundary With CodeGraph

| Task | Correct tool |
|---|---|
| Bean registration, wiring, injection points | Spring Tools |
| Component role / stereotype lookup | Spring Tools |
| REST endpoint → handler mapping | Spring Tools |
| Spring config diagnostics | Spring Tools |
| Spring Boot / ecosystem version info | Spring Tools |
| Internal method logic, call hierarchy | `codegraph_*` |
| Cross-module structural impact | `codegraph_impact` |
| Git/PR change risk | code-review-graph |

Do not substitute CodeGraph structural search for Spring context queries when Spring Tools is available; CodeGraph sees annotations as syntax and cannot fully resolve bean wiring, stereotype roles, or injection point relationships.
