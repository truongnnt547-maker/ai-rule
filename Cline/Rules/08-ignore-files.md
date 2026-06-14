# Ignore Files

## Goal

Never read, analyze, modify, or include in context the files and directories listed below.
These are generated, compiled, or dependency artifacts — not source code.

---

## Version Control Internals

### Directories

- `.git/` — repository internals; use Git commands instead of reading or modifying directly.

## Java (Maven)

### Directories

- `target/` — compiled classes, packaged JARs, Surefire reports
- `.mvn/` — Maven wrapper internals
- `target/generated-sources/` — annotation processor output
- `target/generated-test-sources/`

## Java (Gradle)

### Directories

- `.gradle/` — Gradle cache and metadata
- `build/` — compiled classes and packaged artifacts

### Files

- `gradlew`, `gradlew.bat` — Gradle wrapper scripts; do not modify unless explicitly requested

### Files

- `*.class` — compiled bytecode
- `*.jar` — packaged artifacts
- `*.war`
- `*.ear`
- `mvnw`, `mvnw.cmd` — Maven wrapper scripts (do not modify)
- `vendor/` — vendored dependencies; do not scan unless explicitly requested

### IDE / OS

**IntelliJ IDEA:**

- `.idea/` — project settings, run configs, inspections
- `*.iml` — module files
- `*.iws`, `*.ipr`
- `.factorypath`

**VSCode:**

- `.vscode/` — editor settings, launch configs, extensions list

**OS:**

- `.DS_Store`
- `Thumbs.db`

---

## ReactJS (npm)

### Directories

- `node_modules/` — npm dependencies; never read or scan
- `dist/` — production build output
- `build/` — Create React App build output
- `.next/` — Next.js build cache
- `out/` — Next.js static export
- `coverage/` — Jest coverage reports
- `.cache/` — Babel, ESLint, and bundler caches
- `storybook-static/` — Storybook build output
- `.turbo/` — Turborepo cache
- `.parcel-cache/` — Parcel cache
- `.vite/` — Vite cache
- `.nuxt/` — Nuxt build output
- `.svelte-kit/` — SvelteKit build output
- `.angular/` — Angular cache/build metadata
- `.expo/` — Expo generated state and cache

### Files

- `package-lock.json` — lockfile; never modify manually
- `yarn.lock` — lockfile; never modify manually
- `pnpm-lock.yaml` — lockfile; never modify manually
- `bun.lockb` — lockfile; never modify manually
- `bun.lock` — lockfile; never modify manually
- `*.map` — source maps (`.js.map`, `.css.map`)
- `*.min.js`, `*.min.css` — minified bundles
- `*.log` — log files; may contain sensitive runtime data

## Infrastructure / Deployment Caches

### Directories

- `.serverless/` — Serverless Framework deployment artifacts
- `.terraform/` — Terraform provider/modules cache and state-related metadata

## Python / Tooling Caches

### Directories

- `.venv/`
- `venv/`
- `__pycache__/`
- `.pytest_cache/`
- `.ruff_cache/`
- `.mypy_cache/`
- `.tox/`

### Environment

- `.env`
- `.env.*`
- `.env.local`
- `.env.development.local`
- `.env.test.local`
- `.env.production.local`

> These may contain secrets. Never read, log, or include in output.

> Exception: template files such as `.env.example`, `.env.sample`, or `.env.template` may be read if needed and should not contain secrets.

## Secrets and Credentials

- `*.pem`
- `*.key`
- `*.p12`
- `*.pfx`
- `id_rsa`, `id_dsa`, `id_ecdsa`, `id_ed25519`
- `credentials.json`
- `secrets.json`

These may contain credentials. Never read, log, summarize, or include them in context.

---

## Runtime Logs

### Directories

- `logs/` — runtime logs; may contain sensitive runtime data

## General Rules

- If a task requires information from a build artifact, derive it from source code instead.
- If a directory is listed above, do not scan it even partially.
- If a file matches a pattern above, skip it without asking for confirmation.
- Never manually edit lockfiles.
- Do not edit generated sections in build files if present.
- `pom.xml` itself is source — it may be read and modified normally.
