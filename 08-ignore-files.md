# Ignore Files

## Goal

Never read, analyze, modify, or include in context the files and directories listed below.
These are generated, compiled, or dependency artifacts — not source code.

---

## Java (Maven)

### Directories

- `target/` — compiled classes, packaged JARs, Surefire reports
- `.mvn/` — Maven wrapper internals
- `target/generated-sources/` — annotation processor output
- `target/generated-test-sources/`

### Files

- `*.class` — compiled bytecode
- `*.jar` — packaged artifacts
- `*.war`
- `*.ear`
- `mvnw`, `mvnw.cmd` — Maven wrapper scripts (do not modify)

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

### Files

- `package-lock.json` — lockfile; never modify manually
- `*.map` — source maps (`.js.map`, `.css.map`)
- `*.min.js`, `*.min.css` — minified bundles

### Environment

- `.env.local`
- `.env.development.local`
- `.env.test.local`
- `.env.production.local`

> These may contain secrets. Never read, log, or include in output.

---

## General Rules

- If a task requires information from a build artifact, derive it from source code instead.
- If a directory is listed above, do not scan it even partially.
- If a file matches a pattern above, skip it without asking for confirmation.
- Never suggest edits to lockfiles (`package-lock.json`, `pom.xml`-generated sections).
- `pom.xml` itself is source — it may be read and modified normally.
