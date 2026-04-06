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
metadata:
  version: "2.0"
  last_updated: "2026-04-05"
  status: active
---

# Codebase Analyzer v2.0 — Multi-Phase Structured Codebase Analysis

Runs a structured, phased analysis of any codebase. Each phase is executed by
a dedicated agent with a specific mandate. Phases run sequentially with
checkpoints — the document is written after each phase so interrupted runs can
resume. Output is a persistent `.analysis.md` that doubles as a checkpoint.

---

## Quick Start

```
Analyze this project: [local path or GitHub URL]
```

Optional modifiers:
```
Analyze this project: [path]
Purpose: resume / learning / technical research
Focus: [specific modules or layers to prioritize]
```

---

## Agent Team (5 Agents)

| # | Agent | Mandate | Phase |
|---|-------|---------|-------|
| 1 | `intake_agent` | Read user input, check for existing doc, configure analysis | Phase 0 |
| 2 | `docs_agent` | Read project documentation to understand stated intent | Phase 1 |
| 3 | `structure_agent` | Map directory structure, tech stack, and module boundaries | Phase 2 |
| 4 | `history_agent` | Reconstruct project evolution from git/PR history | Phase 3 |
| 5 | `module_agent` | Deep-read each module one at a time, sequentially | Phase 4 |

Agent definitions: `agents/` directory.
Output template: `templates/analysis_document_template.md`.
Module section template: `templates/module_section_template.md`.

---

## Orchestration Workflow

```
User: "Analyze this project: /path/to/repo"
     |
=== Phase 0: INTAKE & CHECKPOINT CHECK ===
     |
     +-> [intake_agent]
         → See agents/intake_agent.md
     |
     ** CHECKPOINT 0: Confirm plan before proceeding **
     |
=== Phase 1: PROJECT DOCUMENTATION ===
     |
     +-> [docs_agent]
         → See agents/docs_agent.md
     |
     ** CHECKPOINT 1: Summarize doc findings (3–5 bullets), wait for "continue" **
     |
=== Phase 2: STRUCTURE & TECH STACK ===
     |
     +-> [structure_agent]
         → See agents/structure_agent.md
     |
     ** CHECKPOINT 2: Present module list with proposed order, wait for approval **
     |
=== Phase 3: GIT / PR HISTORY ===
     |
     +-> [history_agent]
         → See agents/history_agent.md
     |
     ** CHECKPOINT 3: Present evolution timeline summary, wait for "continue" **
     |
=== Phase 4: MODULE ANALYSIS (sequential, one module at a time) ===
     |
     For each module in the approved order:
     +-> [module_agent]  (one invocation per module)
         → See agents/module_agent.md
         |
         ** CHECKPOINT 4-N: Announce module complete, wait for "go" or questions **
     |
     After all modules ✅:
     +-> Write Summary section
     +-> Report completion to user
```

---

## Checkpoint Rules

1. **After Phase 0**: Confirm plan (new analysis or resume) before proceeding
2. **After Phase 1**: Summarize doc findings, wait for user signal
3. **After Phase 2**: Present module list for approval; user may reorder or skip
4. **After Phase 3**: Present timeline summary, wait for "continue"
5. **After each module**: Announce completion, wait for "go" or questions
6. **Never skip a checkpoint** even if user said "just run it" — surface findings
   at each boundary so the user stays informed and can correct course

**Exception**: If the user explicitly says "run all phases without stopping",
skip checkpoints 2–5 but still print a one-line transition announcement at each.

---

## Resumption

If `.analysis.md` already exists, `intake_agent` handles resumption:
- Reads progress table, finds earliest 🔄 or ⬜ phase
- Reports status to user, asks to confirm before resuming
- Never re-runs ✅ phases unless user explicitly requests

---

## Quality Standards

| Dimension | Requirement |
|-----------|-------------|
| Incrementality | Write to `.analysis.md` after each phase/module — never batch |
| Checkpoint discipline | Always surface findings at each phase boundary |
| Specificity | Cite actual file paths, function names, line counts, config values |
| Quantitative bias | "23 endpoints" not "many endpoints" |
| Design intent | Explain *why*, not just *what* |
| Depth calibration | Core modules: all fields. Utilities: one line |
| No fabrication | Only report what's documented or clearly evident in code |
| Purpose sensitivity | `resume`: flag what's impressive. `learning`: flag what's non-obvious |

---

## Output Language

Follows the language the user writes in. Technical identifiers stay as-is.

---

## Agent File References

| Agent | Definition |
|-------|-----------|
| intake_agent | `agents/intake_agent.md` |
| docs_agent | `agents/docs_agent.md` |
| structure_agent | `agents/structure_agent.md` |
| history_agent | `agents/history_agent.md` |
| module_agent | `agents/module_agent.md` |

## Template References

| Template | Purpose |
|----------|---------|
| `templates/analysis_document_template.md` | Initial `.analysis.md` structure |
| `templates/module_section_template.md` | Per-module section format |

---

## Changelog

| Version | Date | Changes |
|---------|------|---------|
| 2.0 | 2026-04-05 | Multi-agent team; strict checkpoints; detailed module/phase templates; quality standards |
| 1.0 | 2026-04 | Initial version: single-agent, 4 phases, resumable document |
