# structure_agent — Phase 2: Directory Structure & Tech Stack

**Mandate**: Build a complete mental model of the codebase shape before reading
any logic. Identify all modules and establish the analysis order for Phase 4.

---

## Steps (run all, in order)

1. **Map directory layout**
   Run `tree -L 3 --gitignore` (fallback: `find . -not -path '*/.git/*' -maxdepth 3`)

2. **Read dependency and config files** — read ALL that are present:
   - JS/TS: `package.json`, `tsconfig.json`, `webpack.config.*`, `vite.config.*`
   - Python: `requirements.txt`, `pyproject.toml`, `setup.py`, `setup.cfg`
   - Go: `go.mod`
   - Rust: `Cargo.toml`
   - Cross-platform: `Makefile`, `Dockerfile`, `docker-compose.yml`, `*.yaml` CI configs

3. **Identify entry points**
   Main files, index files, CLI entrypoints, server start scripts

4. **Identify modules and layers**
   From directory names, entry points, and import patterns — note dependency
   direction (which modules import which)

---

## Write to Document

Mark Phase 2 🔄 before starting, ✅ when done.

### Tech Stack section

```markdown
## Tech Stack

**Language(s)**: [with versions if specified]
**Framework(s)**: [with versions]
**Key libraries**: [grouped by purpose: HTTP, DB, auth, testing, build, etc.]
**Infrastructure**: [Docker, K8s, cloud provider, databases, queues]
**Build/tooling**: [bundler, linter, formatter, CI system]
**Test framework**: [framework + coverage tooling]
```

### Architecture Overview section

```markdown
## Architecture Overview

**Style**: [monolith / microservices / monorepo / library / CLI / etc.]
**Layer structure**: [describe layers and their relationships in prose]
**Data flow**: [how data moves through the system — brief narrative]
**External dependencies**: [databases, queues, third-party APIs]
**Notable patterns**: [DI, CQRS, event sourcing, etc. — only if clearly present]
```

### Expand progress tracker

Replace the placeholder module row with one row per identified module:

```
| 4a. Module: auth       | ⬜ | |
| 4b. Module: api        | ⬜ | |
| 4c. Module: storage    | ⬜ | |
```

**Ordering rule**: core business logic first, infrastructure/utilities last.
If the user specified focus areas, put those first.

---

## Checkpoint Output

```
Phase 2 complete ✅

Tech stack: [one-line summary]
Architecture: [one-line summary — style + key layers]

Identified [N] modules for analysis, in this order:
  4a. [module] — [one-line purpose]
  4b. [module] — [one-line purpose]
  ...

Does this order look right, or would you like to reorder or skip any?
```

Wait for user approval before Phase 3 begins.
