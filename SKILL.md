---
name: codebase-analyzer
description: >
  Analyze a codebase and produce a structured, resumable analysis document.
  Trigger this skill whenever the user wants to analyze a project, understand
  a codebase, study a repo's architecture, or prepare project materials for
  resume writing or technical review. Also trigger when the user says things
  like "analyze this project", "help me understand this codebase", "read this
  repo", "document this project", or pastes a local path or GitHub URL and
  implies they want a breakdown. The skill produces a persistent markdown
  analysis document that serves as a checkpoint — if analysis is interrupted,
  it can be resumed from where it left off without re-reading already-analyzed
  parts.
---

# Codebase Analyzer

Produces a structured, resumable analysis document for any codebase. The
document is both the output and the checkpoint — each phase is written as it
completes, so interrupted runs can continue from the last saved state.

---

## Step 0: Read user input and check for existing analysis document

Before doing anything, ask the user (or infer from context) for:
- **Project path** (local directory or GitHub URL)
- **Analysis purpose** (optional): learning / resume / technical research /
  general understanding. Defaults to general if not specified.
- **Focus areas** (optional): specific modules or layers to prioritize.

Then check if an analysis document already exists at `<project-root>/.analysis.md`.

- **If it exists**: read it, identify which phases are complete (✅) or
  in-progress (🔄), and **resume from the earliest incomplete phase**.
  Do not re-read already-analyzed sections.
- **If it does not exist**: create it immediately with the progress tracker
  and empty section headers (see template below), then begin Phase 1.

---

## Analysis Document Template

Create `.analysis.md` at the project root with this structure on first run.
Open the file at the start and keep it open — append to it as each phase
completes. Never rewrite the whole file; always append or update in place.

```markdown
# [Project Name] — Analysis Document

> Purpose: [learning / resume / technical research / general]
> Focus: [user-specified focus areas, or "general"]
> Last updated: [timestamp]

---

## Progress

| Phase | Status | Notes |
|---|---|---|
| 1. Project documentation | ⬜ | |
| 2. Directory structure & tech stack | ⬜ | |
| 3. Git / PR history | ⬜ | |
| 4. Module analysis | ⬜ | Expands after Phase 2 |

---

## Tech Stack

_To be filled in Phase 2._

---

## Architecture Overview

_To be filled in Phase 2._

---

## PR / Commit Evolution

_To be filled in Phase 3._

---

## Module Analysis

_To be filled in Phase 4. Modules will be listed after Phase 2._

---

## Raw Notes

_Miscellaneous observations that don't fit elsewhere._
```

---

## Phase 1: Project Documentation

**Goal**: Understand the project from the author's own words before touching code.

Read in this priority order (stop when you have enough context — don't read
everything if early docs are already comprehensive):

1. `README.md` (or `README.rst`, `README`)
2. `CHANGELOG.md` / `HISTORY.md`
3. `docs/` directory — architecture docs, ADRs, design docs first
4. `CONTRIBUTING.md` — reveals project conventions and structure intent
5. Any `.github/` docs (issue templates, PR templates reveal workflow)

**Write to document** when done:
- Update Phase 1 status to ✅
- Fill in project name, purpose, and any architectural intent stated in docs
- Add anything notable to Raw Notes

---

## Phase 2: Directory Structure & Tech Stack

**Goal**: Build a mental model of the codebase shape before reading any logic.

Steps:
1. Run `tree -L 3 --gitignore` (or equivalent) to get directory structure
2. Read dependency/config files:
   - JS/TS: `package.json`, `tsconfig.json`
   - Python: `requirements.txt`, `pyproject.toml`, `setup.py`
   - Other: `Makefile`, `Dockerfile`, `docker-compose.yml`, `*.yaml` configs
3. Identify entry points (main files, index files, CLI entrypoints)
4. Identify major layers / modules from directory names

**Write to document** when done:
- Update Phase 2 status to ✅
- Fill in **Tech Stack** section: languages, frameworks, key libraries, infra
- Fill in **Architecture Overview**: high-level description of layers and how
  they relate (prose, not a diagram)
- **Expand the Module Analysis section** in the progress tracker — replace the
  single "Module analysis" row with one row per identified module, all ⬜

Example expansion:
```
| 4a. Module: auth | ⬜ | |
| 4b. Module: api-gateway | ⬜ | |
| 4c. Module: storage | ⬜ | |
```

Order modules by importance — core business logic first, utilities/infra last.
If the user specified focus areas, put those first.

---

## Phase 3: Git / PR History

**Goal**: Understand how the project evolved — what decisions were made and when.

Use your judgment on which git artifacts to read. Quality varies by project:

**Preferred sources (in order of signal quality):**
1. **PR descriptions** (via `gh pr list --state merged --limit 50 --json
   number,title,body,mergedAt` if GitHub CLI is available)
2. **Commit messages** (`git log --oneline --no-merges -100`)
3. **Commit diffs** — only if messages are too terse to understand intent;
   be selective, don't read all diffs

You don't need to cover every PR or commit. Aim to identify:
- **Major phases** in the project's life (initial scaffold, core feature
  build-out, refactors, scaling decisions)
- **Key technical turning points** (architecture changes, library swaps,
  significant new capabilities)
- **Design decisions that are non-obvious from the code**

**Write to document** when done:
- Update Phase 3 status to ✅
- Fill in **PR / Commit Evolution** section:

```markdown
## PR / Commit Evolution

### Phase: [name, e.g. "Initial scaffold"]
_[date range]_
- [PR/commit]: what changed and why (if discernible)

### Phase: [name]
_[date range]_
- ...
```

Group by logical phases, not calendar time. 3–6 phases is usually right.
If the project has few/poor commit messages, note that and summarize what
you can infer from the code structure instead.

---

## Phase 4: Module Analysis

Analyze modules one at a time in the order established in Phase 2.
For each module:

1. Mark it 🔄 in the progress tracker
2. Read the module's files — start with the most important ones, go deeper
   as needed
3. Write the module section
4. Mark it ✅ in the progress tracker
5. Move to the next module

**For each module, write:**

```markdown
### Module: [name]

**Purpose**: What problem does this module solve? Why does it exist?

**Design decisions**: What notable choices were made here? Why this approach
over alternatives? (Especially valuable if documented in PRs/comments.)

**Key technical points**: Frameworks, algorithms, patterns, anything
technically interesting or non-obvious.

**Quantitative details**: Numbers that matter — scale, counts, performance
characteristics. Extract from code/docs/comments if possible.

**Interfaces**: How does this module connect to others? Key inputs/outputs
or APIs (brief — not full signatures).

**Tradeoffs / limitations**: Known constraints or deliberate simplifications,
if discernible.
```

Depth should match module importance:
- **Core modules**: fill all fields thoroughly
- **Supporting modules**: Purpose + Key technical points is enough
- **Utility/infra modules**: one-sentence purpose + tech stack entry is fine

---

## Completion

When all phases are ✅:

1. Update the timestamp in the document header
2. Write a brief **Summary** section at the top (after the progress table):

```markdown
## Summary

[2–4 sentence project summary: what it does, how it's built, what's
technically interesting about it. Written for someone who needs to
quickly understand this project before an interview or technical discussion.]
```

3. Tell the user the analysis is complete and where the document lives.
   Mention that resume bullet generation and review prep can be done
   separately using the analysis document as input.

---

## Resumption Instructions

If called on a project that already has `.analysis.md`:

1. Read the progress table
2. Find the earliest 🔄 or ⬜ phase
3. If a module is 🔄 (in-progress): re-read that module's existing notes
   briefly, then continue from where it left off
4. If a phase is ⬜: start that phase fresh
5. Never re-do ✅ phases unless the user explicitly asks

---

## Notes for the Agent

- **Write incrementally**: don't batch up writes. Write each section as soon
  as it's complete, so the document is always current.
- **Be selective with context**: you don't need to read every file. Use
  directory structure and file names to judge what's worth reading deeply.
- **Prefer understanding over coverage**: a thorough analysis of the 3 most
  important modules beats a shallow pass over all 10.
- **git history judgment**: if PR descriptions are rich, use them. If commit
  messages are terse ("fix bug", "update"), lean on diffs selectively or note
  that history is uninformative.
- **Quantitative details matter**: if you see numbers in code (counts,
  thresholds, sizes, layers), capture them — these become resume bullet fodder.
- **Analysis purpose matters**: for `resume`, bias toward "what's technically
  impressive"; for `learning`, bias toward "what's the design intent and why".

