---
paths:
  - "package.json"
  - "pom.xml"
  - "build.gradle"
  - "build.gradle.kts"
  - "settings.gradle"
  - "settings.gradle.kts"
  - "vite.config.*"
  - "webpack.config.*"
  - "tsconfig.json"
  - "Dockerfile"
  - "docker-compose*.yml"
  - ".github/workflows/**"
---

# Dependency and Build Discipline

## Goal

Control dependency changes, lockfiles, and build/config updates without replacing graph-first repository understanding.

## Relationship to the graph-first workflow

Apply these dependency/build checks after selecting the relevant context path.

Do not use this always-active dependency rule as a reason to skip CodeGraph or Code Review Graph for non-trivial code understanding, refactoring, review, or impact analysis tasks.

## Installing or Updating Dependencies

- Do not install or upgrade packages without explicit approval.
- Prefer minimal additions; reuse existing dependencies when possible.
- Explain why a new dependency is needed (functionality, security fix, compatibility).

## Lockfiles and Generated Artifacts

- Never manually edit lockfiles (`package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`, `bun.lockb`, `bun.lock`).
- Do not regenerate lockfiles unless required for the task and approved.
- Do not edit generated build outputs; fix sources instead.

## Build, Tooling, and CI Changes

- Avoid changing build scripts, CI pipelines, or linters/formatters unless required for the task.
- If build/config changes are necessary, keep them minimal and scoped; document why.

## Validation After Dependency/Build Changes

- Recommend the smallest relevant checks after dependency or build config changes.
- Compile/build checks may run automatically when they are the relevant verification step.
- Run targeted tests (e.g., `npm test -- <scope>` or `mvn -pl <module> test`) only after explicit user confirmation.
- If validation is not run, state why and what should be run by the user.

## Security and Maintenance

- Prefer maintained libraries; avoid abandoned or unvetted packages.
- For security-related dependencies, verify the change addresses the specific need (e.g., CVE fix) and avoid broad version jumps without reason.

## Documentation

- Note any new scripts, env vars, or build steps introduced by the change.