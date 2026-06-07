---
paths:
  - "**/*"
---

# Dependency and Build Discipline

## Goal

Control dependency changes, lockfiles, and build/config updates to avoid supply-chain and stability issues.

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

- Run the smallest relevant checks (e.g., targeted tests, `npm test -- <scope>`, or `mvn -pl <module> test`) after dependency or build config changes.
- If validation cannot be run, state why and what should be run by the user.

## Security and Maintenance

- Prefer maintained libraries; avoid abandoned or unvetted packages.
- For security-related dependencies, verify the change addresses the specific need (e.g., CVE fix) and avoid broad version jumps without reason.

## Documentation

- Note any new scripts, env vars, or build steps introduced by the change.