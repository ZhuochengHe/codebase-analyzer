# history_agent — Phase 3: Git / PR History

**Mandate**: Reconstruct *why* the project is the way it is — decisions, pivots,
and evolution. Focus on what's non-obvious from the current code.

---

## Source Priority

Use the highest-quality source available:

1. **PR descriptions** (richest signal)
   ```
   gh pr list --state merged --limit 50 --json number,title,body,mergedAt,author
   ```
2. **Commit messages** (medium signal)
   ```
   git log --oneline --no-merges -100
   ```
3. **Selective diffs** (low signal, high cost) — only if messages are too terse
   to understand intent; be very selective, read at most a handful

If none are available (empty repo, no history), note that and move on.

---

## What to Extract

- **Major phases** in the project's life — 3 to 6 is the right range
- **Key technical decisions** that are non-obvious from current code
- **Architecture changes or library swaps** with their rationale
- **Performance, scalability, or security decisions** if documented
- **Pain point patterns** — what gets fixed most reveals where the hard problems are

Do not attempt to cover every PR or commit. Depth over breadth.

---

## Write to Document

Mark Phase 3 🔄 before starting, ✅ when done.

```markdown
## PR / Commit Evolution

### Phase [N]: [name, e.g. "Initial scaffold"]
_[date range]_

- [PR #X / commit abc123]: [what changed] — [why, if discernible]
- ...

**Key decisions in this phase:**
- [decision and rationale]

### Phase [N+1]: [name]
_[date range]_
...

### Observations
- [notable patterns in team behavior, velocity, recurring themes]
```

If commit history is uninformative (terse messages, no PRs), note that explicitly
and summarize what can be inferred from code structure instead.

---

## Checkpoint Output

```
Phase 3 complete ✅

Project evolved through [N] phases from [start] to [end]:
  → [Phase 1 name]: [one-line summary]
  → [Phase 2 name]: [one-line summary]
  ...

Key non-obvious decisions found: [N]

Ready to begin module analysis. Continue?
```

Wait for user signal before Phase 4 begins.
