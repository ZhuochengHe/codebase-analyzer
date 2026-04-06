# module_agent — Phase 4: Module Analysis

**Mandate**: Deeply understand each module's purpose, design, and interfaces.
Runs once per module, sequentially, in the order established in Phase 2.

---

## Per-Module Steps

For each module:

1. Mark it 🔄 in the progress tracker
2. Scan the module directory to identify its files
3. Read files in this order:
   - Entry point / index / main file
   - Core logic files (largest, most imported by others)
   - Interface and type definitions
   - Tests — reveal intended behavior and edge cases
   - Configuration files
4. Write the module section using `templates/module_section_template.md`
   - Fill the **Lifecycle role** fields — what this module receives, produces, and whether it forks
5. **Update the Lifecycle section** in `.analysis.md`:
   - Find this module's node(s) in the skeleton (they are marked `[?]`)
   - Replace `[?]` with a brief annotation: what data arrives, what leaves, any branch conditions
   - If this module introduces a branch not in the skeleton, add it now
   - If this module is a terminal node (returns response, emits final event), mark it `[END]`
6. Mark it ✅ in the progress tracker
7. Checkpoint with user before moving to the next module

---

## Depth Calibration

Match reading depth to module importance:

| Module type | Reading depth | Fields to fill |
|-------------|--------------|----------------|
| Core business logic | Deep — read thoroughly | All fields |
| Supporting (adapters, middleware) | Medium | Purpose + Key technical + Interfaces |
| Utility / infra | Light — entry point only | Purpose + one-sentence tech note |

---

## What "deep" means for core modules

For core modules, do not stop at the index file. Read:
- The actual implementation of the most-called functions
- Error handling paths — these reveal assumptions and failure modes
- Test files — see what scenarios the author considered edge cases
- Any inline comments or docstrings explaining non-obvious choices
- Cross-references to other modules (reveals coupling)

---

## Purpose-Sensitive Framing

Adjust emphasis based on the analysis purpose set in Phase 0:

- **`resume`**: emphasize scale (numbers), technical decisions, what's impressive
- **`learning`**: emphasize why decisions were made, what's non-obvious, patterns
- **`technical research`**: emphasize interfaces, data flows, coupling, tradeoffs
- **`general`**: balanced coverage

---

## Checkpoint Output (after each module)

```
Module: [name] ✅

Key finding: [one sentence — the most interesting or non-obvious thing]

Lifecycle update: [describe what changed in the lifecycle tree — node filled, branch added, terminal found]

[N] modules remaining: [list]

Next: [module name]. Continue? (or ask questions about [name] first)
```

Wait for "go", "continue", or a question before starting the next module.

---

## Completion (after all modules ✅)

1. Update timestamp in document header
2. Write the Summary section at the top of `.analysis.md` (after progress table):

```markdown
## Summary

**What it does**: [one sentence]
**How it's built**: [tech stack + architecture in one sentence]
**Scale / scope**: [size signals — endpoints, users, lines of code, etc. if known]
**Lifecycle shape**: [one sentence — e.g. "single linear pipeline", "3 independent entry points with fan-out at the router", "event-driven branching after ingestion"]
**What's technically interesting**: [2–3 specific things worth discussing in depth]
**What I'd investigate next**: [what warrants deeper analysis if continuing]
```

3. Verify the `## Lifecycle` section is complete — no remaining `[?]` stubs for analyzed modules.
   If any stubs remain for skipped or unanalyzed modules, leave them as `[?]` but add a note:
   `<!-- [module-name] not analyzed — stub only -->`

3. Tell the user:
   - Analysis complete, document lives at `[project-root]/.analysis.md`
   - Can be used for resume bullet generation or interview prep
   - Re-run the skill to resume if the codebase changes
